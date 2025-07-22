### **🔧 O que são Stored Procedures?**

#### **Definição:**

> Stored Procedures são conjuntos de instruções SQL pré-compiladas e armazenadas no servidor de base de dados. Podem aceitar parâmetros, conter lógica de negócio e retornar resultados.

#### **Vantagens:**

textresponse-action-icon

```text
⚡ Performance - Pré-compiladas, execução mais rápida
🛡️ Segurança - Encapsulam lógica, previnem SQL injection
🔄 Reutilização - Uma procedure, múltiplas aplicações
📊 Consistência - Lógica de negócio centralizada
🌐 Redução tráfego - Menos dados na rede
🔒 Controlo acesso - Permissões granulares
```

#### **Desvantagens:**

textresponse-action-icon

```text
🏃‍♂️ Portabilidade - Específicas do SGBD
🐛 Debug - Mais difícil de debuggar
📝 Versionamento - Controlo de versões complexo
🔧 Manutenção - Mudanças requerem deploy no BD
```

### **🆕 Criar Stored Procedures:**

#### **Sintaxe Básica (MySQL):**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE procedure_name(
    IN parameter1 datatype,
    OUT parameter2 datatype,
    INOUT parameter3 datatype
)
BEGIN
    -- Corpo da procedure
    SQL statements;
END //
DELIMITER ;
```

#### **Exemplo Simples:**

sqlresponse-action-icon

```sql
-- Procedure básica sem parâmetros
DELIMITER //
CREATE PROCEDURE GetAllCustomers()
BEGIN
    SELECT id, first_name, last_name, email 
    FROM customers 
    WHERE status = 'active'
    ORDER BY last_name, first_name;
END //
DELIMITER ;

-- Executar a procedure
CALL GetAllCustomers();
```

#### **Procedure com Parâmetros IN:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE GetCustomerOrders(
    IN customer_id INT
)
BEGIN
    SELECT 
        o.id,
        o.order_number,
        o.order_date,
        o.status,
        o.total_amount
    FROM orders o
    WHERE o.customer_id = customer_id
    ORDER BY o.order_date DESC;
END //
DELIMITER ;

-- Executar com parâmetro
CALL GetCustomerOrders(123);
```

#### **Procedure com Parâmetros OUT:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE GetCustomerStats(
    IN customer_id INT,
    OUT total_orders INT,
    OUT total_spent DECIMAL(10,2),
    OUT last_order_date DATE
)
BEGIN
    SELECT 
        COUNT(id),
        COALESCE(SUM(total_amount), 0),
        MAX(order_date)
    INTO total_orders, total_spent, last_order_date
    FROM orders 
    WHERE customer_id = customer_id AND status = 'completed';
END //
DELIMITER ;

-- Executar e obter resultados
CALL GetCustomerStats(123, @orders, @spent, @last_date);
SELECT @orders AS total_orders, @spent AS total_spent, @last_date AS last_order_date;
```

### **🔄 Controlo de Fluxo:**

#### **Condições IF-THEN-ELSE:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE ProcessOrder(
    IN order_id INT,
    OUT result_message VARCHAR(255)
)
BEGIN
    DECLARE order_status VARCHAR(50);
    DECLARE customer_credit_limit DECIMAL(10,2);
    DECLARE order_total DECIMAL(10,2);
    
    -- Obter informações do pedido
    SELECT o.status, o.total_amount, c.credit_limit
    INTO order_status, order_total, customer_credit_limit
    FROM orders o
    INNER JOIN customers c ON o.customer_id = c.id
    WHERE o.id = order_id;
    
    -- Lógica condicional
    IF order_status IS NULL THEN
        SET result_message = 'Order not found';
    ELSEIF order_status != 'pending' THEN
        SET result_message = 'Order already processed';
    ELSEIF order_total > customer_credit_limit THEN
        SET result_message = 'Order exceeds credit limit';
        UPDATE orders SET status = 'credit_hold' WHERE id = order_id;
    ELSE
        SET result_message = 'Order approved';
        UPDATE orders SET status = 'approved' WHERE id = order_id;
    END IF;
END //
DELIMITER ;

-- Teste
CALL ProcessOrder(100, @result);
SELECT @result;
```

#### **Loops - WHILE:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE GenerateTestData(
    IN num_customers INT
)
BEGIN
    DECLARE counter INT DEFAULT 1;
    
    WHILE counter <= num_customers DO
        INSERT INTO customers (first_name, last_name, email)
        VALUES (
            CONCAT('Test', counter),
            CONCAT('Customer', counter),
            CONCAT('test', counter, '@example.com')
        );
        
        SET counter = counter + 1;
    END WHILE;
END //
DELIMITER ;

-- Gerar 100 clientes de teste
CALL GenerateTestData(100);
```

#### **Loops - REPEAT:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE CalculateInterest(
    IN principal DECIMAL(10,2),
    IN rate DECIMAL(5,4),
    IN years INT,
    OUT final_amount DECIMAL(10,2)
)
BEGIN
    DECLARE year_counter INT DEFAULT 1;
    SET final_amount = principal;
    
    REPEAT
        SET final_amount = final_amount * (1 + rate);
        SET year_counter = year_counter + 1;
    UNTIL year_counter > years
    END REPEAT;
END //
DELIMITER ;

-- Calcular juros compostos
CALL CalculateInterest(1000.00, 0.05, 5, @result);
SELECT @result; -- Resultado após 5 anos a 5%
```

### **📊 Procedures com Cursors:**

#### **Processar Resultados Linha a Linha:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE UpdateCustomerTiers()
BEGIN
    DECLARE done INT DEFAULT FALSE;
    DECLARE customer_id INT;
    DECLARE total_spent DECIMAL(10,2);
    DECLARE new_tier VARCHAR(20);
    
    -- Declarar cursor
    DECLARE customer_cursor CURSOR FOR
        SELECT 
            c.id,
            COALESCE(SUM(o.total_amount), 0) AS total
        FROM customers c
        LEFT JOIN orders o ON c.id = o.customer_id AND o.status = 'completed'
        GROUP BY c.id;
    
    -- Handler para fim do cursor
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    OPEN customer_cursor;
    
    read_loop: LOOP
        FETCH customer_cursor INTO customer_id, total_spent;
        
        IF done THEN
            LEAVE read_loop;
        END IF;
        
        -- Determinar tier baseado no gasto
        IF total_spent >= 10000 THEN
            SET new_tier = 'VIP';
        ELSEIF total_spent >= 5000 THEN
            SET new_tier = 'Gold';
        ELSEIF total_spent >= 1000 THEN
            SET new_tier = 'Silver';
        ELSE
            SET new_tier = 'Bronze';
        END IF;
        
        -- Atualizar customer
        UPDATE customers 
        SET customer_tier = new_tier, total_spent = total_spent
        WHERE id = customer_id;
        
    END LOOP;
    
    CLOSE customer_cursor;
END //
DELIMITER ;

-- Executar atualização de tiers
CALL UpdateCustomerTiers();
```

### **🚨 Tratamento de Erros:**

#### **Exception Handling:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE TransferFunds(
    IN from_account INT,
    IN to_account INT,
    IN amount DECIMAL(10,2),
    OUT result_message VARCHAR(255)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SET result_message = 'Transfer failed - transaction rolled back';
    END;
    
    DECLARE EXIT HANDLER FOR SQLWARNING
    BEGIN
        ROLLBACK;
        SET result_message = 'Transfer warning - transaction rolled back';
    END;
    
    START TRANSACTION;
    
    -- Verificar saldo suficiente
    IF (SELECT balance FROM accounts WHERE id = from_account) < amount THEN
        SET result_message = 'Insufficient funds';
        ROLLBACK;
    ELSE
        -- Debitar conta origem
        UPDATE accounts 
        SET balance = balance - amount 
        WHERE id = from_account;
        
        -- Creditar conta destino
        UPDATE accounts 
        SET balance = balance + amount 
        WHERE id = to_account;
        
        -- Registrar transação
        INSERT INTO transactions (from_account, to_account, amount, transaction_date)
        VALUES (from_account, to_account, amount, NOW());
        
        COMMIT;
        SET result_message = 'Transfer completed successfully';
    END IF;
END //
DELIMITER ;

-- Teste da transferência
CALL TransferFunds(1, 2, 500.00, @result);
SELECT @result;
```

### **📈 Procedures para Relatórios:**

#### **Relatório de Vendas Mensal:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE GenerateMonthlySalesReport(
    IN report_year INT,
    IN report_month INT
)
BEGIN
    -- Declarar variáveis para totais
    DECLARE total_orders INT DEFAULT 0;
    DECLARE total_revenue DECIMAL(12,2) DEFAULT 0;
    DECLARE total_customers INT DEFAULT 0;
    
    -- Calcular métricas principais
    SELECT 
        COUNT(*),
        SUM(total_amount),
        COUNT(DISTINCT customer_id)
    INTO total_orders, total_revenue, total_customers
    FROM orders
    WHERE YEAR(order_date) = report_year
    AND MONTH(order_date) = report_month
    AND status = 'completed';
    
    -- Resultado 1: Resumo geral
    SELECT 
        report_year AS year,
        report_month AS month,
        MONTHNAME(STR_TO_DATE(report_month, '%m')) AS month_name,
        total_orders,
        total_revenue,
        total_customers,
        ROUND(total_revenue / total_orders, 2) AS avg_order_value,
        ROUND(total_revenue / total_customers, 2) AS revenue_per_customer;
    
    -- Resultado 2: Top 10 produtos
    SELECT 
        p.name AS product_name,
        SUM(oi.quantity) AS quantity_sold,
        SUM(oi.quantity * oi.unit_price) AS product_revenue,
        COUNT(DISTINCT oi.order_id) AS orders_count
    FROM order_items oi
    INNER JOIN orders o ON oi.order_id = o.id
    INNER JOIN products p ON oi.product_id = p.id
    WHERE YEAR(o.order_date) = report_year
    AND MONTH(o.order_date) = report_month
    AND o.status = 'completed'
    GROUP BY p.id, p.name
    ORDER BY product_revenue DESC
    LIMIT 10;
    
    -- Resultado 3: Vendas por categoria
    SELECT 
        c.name AS category_name,
        COUNT(DISTINCT o.id) AS orders_count,
        SUM(oi.quantity) AS total_quantity,
        SUM(oi.quantity * oi.unit_price) AS category_revenue,
        ROUND(
            SUM(oi.quantity * oi.unit_price) * 100.0 / total_revenue, 2
        ) AS revenue_percentage
    FROM order_items oi
    INNER JOIN orders o ON oi.order_id = o.id
    INNER JOIN products p ON oi.product_id = p.id
    INNER JOIN categories c ON p.category_id = c.id
    WHERE YEAR(o.order_date) = report_year
    AND MONTH(o.order_date) = report_month
    AND o.status = 'completed'
    GROUP BY c.id, c.name
    ORDER BY category_revenue DESC;
    
END //
DELIMITER ;

-- Gerar relatório para Janeiro 2024
CALL GenerateMonthlySalesReport(2024, 1);
```

#### **Procedure para Análise de Cohort:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE AnalyzeCustomerCohort(
    IN cohort_year INT,
    IN cohort_month INT
)
BEGIN
    -- Temporary table para cohort base
    CREATE TEMPORARY TABLE cohort_customers AS
    SELECT DISTINCT customer_id
    FROM orders
    WHERE YEAR(order_date) = cohort_year
    AND MONTH(order_date) = cohort_month
    AND customer_id NOT IN (
        SELECT DISTINCT customer_id
        FROM orders
        WHERE order_date < STR_TO_DATE(CONCAT(cohort_year, '-', cohort_month, '-01'), '%Y-%m-%d')
    );
    
    -- Análise de retenção por período
    SELECT 
        period_offset,
        customers_active,
        ROUND(customers_active * 100.0 / total_cohort_size, 2) AS retention_rate,
        total_orders,
        total_revenue,
        ROUND(total_revenue / customers_active, 2) AS avg_revenue_per_customer
    FROM (
        SELECT 
            PERIOD_DIFF(DATE_FORMAT(o.order_date, '%Y%m'), 
                        CONCAT(cohort_year, LPAD(cohort_month, 2, '0'))) AS period_offset,
            COUNT(DISTINCT o.customer_id) AS customers_active,
            COUNT(*) AS total_orders,
            SUM(o.total_amount) AS total_revenue,
            (SELECT COUNT(*) FROM cohort_customers) AS total_cohort_size
        FROM orders o
        INNER JOIN cohort_customers cc ON o.customer_id = cc.customer_id
        WHERE o.status = 'completed'
        GROUP BY PERIOD_DIFF(DATE_FORMAT(o.order_date, '%Y%m'), 
                            CONCAT(cohort_year, LPAD(cohort_month, 2, '0')))
        ORDER BY period_offset
    ) cohort_analysis;
    
    DROP TEMPORARY TABLE cohort_customers;
END //
DELIMITER ;

-- Analisar cohort de Janeiro 2024
CALL AnalyzeCustomerCohort(2024, 1);
```

### **🎯 Procedures de Manutenção:**

#### **Limpeza Automática de Dados:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE CleanupOldData()
BEGIN
    DECLARE rows_affected INT DEFAULT 0;
    DECLARE cleanup_log VARCHAR(500) DEFAULT '';
    
    -- Log início
    INSERT INTO maintenance_log (action, start_time) 
    VALUES ('CLEANUP_OLD_DATA', NOW());
    
    -- 1. Remover logs antigos (> 90 dias)
    DELETE FROM access_logs 
    WHERE created_at < DATE_SUB(NOW(), INTERVAL 90 DAY);
    SET rows_affected = ROW_COUNT();
    SET cleanup_log = CONCAT(cleanup_log, 'Access logs: ', rows_affected, ' rows; ');
    
    -- 2. Remover carrinho abandonado (> 30 dias)  
    DELETE FROM shopping_carts  
    WHERE status = 'abandoned'  
    AND updated_at < DATE_SUB(NOW(), INTERVAL 30 DAY);  
    SET rows_affected = ROW_COUNT();  
    SET cleanup_log = CONCAT(cleanup_log, 'Abandoned carts: ', rows_affected, ' rows; ');
    
    -- 3. Arquivar pedidos antigos  
    INSERT INTO orders_archive  
    SELECT * FROM orders  
    WHERE status IN ('completed', 'cancelled')  
    AND order_date < DATE_SUB(NOW(), INTERVAL 2 YEAR);  
    SET rows_affected = ROW_COUNT();  
    SET cleanup_log = CONCAT(cleanup_log, 'Archived orders: ', rows_affected, ' rows; ');
    
    -- 4. Remover pedidos arquivados da tabela principal  
    DELETE FROM orders  
    WHERE status IN ('completed', 'cancelled')  
    AND order_date < DATE_SUB(NOW(), INTERVAL 2 YEAR);  
    SET rows_affected = ROW_COUNT();  
    SET cleanup_log = CONCAT(cleanup_log, 'Deleted old orders: ', rows_affected, ' rows; ');
    
    -- 5. Atualizar estatísticas das tabelas  
    ANALYZE TABLE customers, products, orders;
    
    -- Log fim  
    UPDATE maintenance_log  
    SET end_time = NOW(),  
    details = cleanup_log,  
    status = 'COMPLETED'  
    WHERE action = 'CLEANUP_OLD_DATA'  
    AND start_time = (SELECT MAX(start_time) FROM (SELECT start_time FROM maintenance_log WHERE action = 'CLEANUP_OLD_DATA') t);
    

END //  
DELIMITER ;

-- Executar limpeza  
CALL CleanupOldData();
```


#### **Backup de Dados Críticos:**
```sql
DELIMITER //
CREATE PROCEDURE BackupCriticalData(
    IN backup_date DATE
)
BEGIN
    DECLARE backup_suffix VARCHAR(20);
    DECLARE sql_stmt TEXT;
    
    SET backup_suffix = DATE_FORMAT(backup_date, '%Y%m%d');
    
    -- Backup customers
    SET sql_stmt = CONCAT('CREATE TABLE customers_backup_', backup_suffix, ' AS SELECT * FROM customers WHERE status = "active"');
    SET @sql = sql_stmt;
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
    
    -- Backup orders (último mês)
    SET sql_stmt = CONCAT('CREATE TABLE orders_backup_', backup_suffix, ' AS SELECT * FROM orders WHERE order_date >= DATE_SUB("', backup_date, '", INTERVAL 30 DAY)');
    SET @sql = sql_stmt;
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
    
    -- Log do backup
    INSERT INTO backup_log (backup_date, tables_backed_up, status)
    VALUES (backup_date, CONCAT('customers_backup_', backup_suffix, ', orders_backup_', backup_suffix), 'SUCCESS');
    
END //
DELIMITER ;

-- Criar backup
CALL BackupCriticalData(CURDATE());
````


### **🔧 Procedures Administrativas:**

#### **Recalcular Estatísticas de Cliente:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE RecalculateCustomerStats()
BEGIN
    DECLARE done INT DEFAULT FALSE;
    DECLARE customer_id INT;
    DECLARE total_orders INT;
    DECLARE total_spent DECIMAL(12,2);
    DECLARE last_order DATE;
    DECLARE avg_days_between_orders DECIMAL(8,2);
    DECLARE customer_tier VARCHAR(20);
    
    DECLARE customer_cursor CURSOR FOR
        SELECT id FROM customers WHERE status = 'active';
    
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    -- Log início
    INSERT INTO process_log (process_name, start_time) 
    VALUES ('RECALCULATE_CUSTOMER_STATS', NOW());
    
    OPEN customer_cursor;
    
    customer_loop: LOOP
        FETCH customer_cursor INTO customer_id;
        
        IF done THEN
            LEAVE customer_loop;
        END IF;
        
        -- Calcular estatísticas para este cliente
        SELECT 
            COUNT(*),
            COALESCE(SUM(total_amount), 0),
            MAX(order_date),
            CASE 
                WHEN COUNT(*) > 1 THEN 
                    DATEDIFF(MAX(order_date), MIN(order_date)) / (COUNT(*) - 1)
                ELSE NULL 
            END
        INTO total_orders, total_spent, last_order, avg_days_between_orders
        FROM orders 
        WHERE customer_id = customer_id AND status = 'completed';
        
        -- Determinar tier
        CASE 
            WHEN total_spent >= 10000 THEN SET customer_tier = 'VIP';
            WHEN total_spent >= 5000 THEN SET customer_tier = 'Gold';
            WHEN total_spent >= 1000 THEN SET customer_tier = 'Silver';
            ELSE SET customer_tier = 'Bronze';
        END CASE;
        
        -- Atualizar customer
        UPDATE customers 
        SET 
            total_orders = total_orders,
            total_spent = total_spent,
            last_order_date = last_order,
            avg_days_between_orders = avg_days_between_orders,
            customer_tier = customer_tier,
            stats_updated_at = NOW()
        WHERE id = customer_id;
        
    END LOOP;
    
    CLOSE customer_cursor;
    
    -- Log fim
    UPDATE process_log 
    SET end_time = NOW(), status = 'COMPLETED'
    WHERE process_name = 'RECALCULATE_CUSTOMER_STATS'
    AND start_time = (SELECT MAX(start_time) FROM (SELECT start_time FROM process_log WHERE process_name = 'RECALCULATE_CUSTOMER_STATS') t);
    
END //
DELIMITER ;
```

### **📊 Functions vs Procedures:**

#### **Stored Functions:**

sqlresponse-action-icon

```sql
-- Function que retorna um valor
DELIMITER //
CREATE FUNCTION CalculateAge(birth_date DATE)
RETURNS INT
READS SQL DATA
DETERMINISTIC
BEGIN
    RETURN YEAR(CURDATE()) - YEAR(birth_date) - 
           (DATE_FORMAT(CURDATE(), '%m%d') < DATE_FORMAT(birth_date, '%m%d'));
END //
DELIMITER ;

-- Usar a function
SELECT 
    first_name, 
    last_name, 
    birth_date,
    CalculateAge(birth_date) AS age
FROM customers;

-- Function para calcular desconto
DELIMITER //
CREATE FUNCTION CalculateDiscount(
    customer_tier VARCHAR(20),
    order_amount DECIMAL(10,2)
)
RETURNS DECIMAL(5,2)
READS SQL DATA
DETERMINISTIC
BEGIN
    DECLARE discount_rate DECIMAL(5,2) DEFAULT 0.00;
    
    CASE customer_tier
        WHEN 'VIP' THEN 
            IF order_amount >= 1000 THEN SET discount_rate = 0.15;
            ELSE SET discount_rate = 0.10;
            END IF;
        WHEN 'Gold' THEN SET discount_rate = 0.08;
        WHEN 'Silver' THEN SET discount_rate = 0.05;
        WHEN 'Bronze' THEN SET discount_rate = 0.02;
        ELSE SET discount_rate = 0.00;
    END CASE;
    
    RETURN discount_rate;
END //
DELIMITER ;

-- Usar em queries
SELECT 
    order_id,
    customer_tier,
    order_amount,
    CalculateDiscount(customer_tier, order_amount) AS discount_rate,
    order_amount * CalculateDiscount(customer_tier, order_amount) AS discount_amount
FROM order_summary;
```

### **🎯 Procedures com Dynamic SQL:**

#### **Relatórios Configuráveis:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE DynamicSalesReport(
    IN date_from DATE,
    IN date_to DATE,
    IN group_by_field VARCHAR(50),
    IN filter_conditions TEXT
)
BEGIN
    DECLARE sql_query TEXT;
    DECLARE group_by_clause TEXT DEFAULT '';
    DECLARE where_clause TEXT DEFAULT '';
    
    -- Construir GROUP BY
    CASE group_by_field
        WHEN 'daily' THEN SET group_by_clause = 'GROUP BY DATE(o.order_date)';
        WHEN 'weekly' THEN SET group_by_clause = 'GROUP BY YEARWEEK(o.order_date)';
        WHEN 'monthly' THEN SET group_by_clause = 'GROUP BY YEAR(o.order_date), MONTH(o.order_date)';
        WHEN 'category' THEN SET group_by_clause = 'GROUP BY c.name';
        WHEN 'customer' THEN SET group_by_clause = 'GROUP BY cust.id';
        ELSE SET group_by_clause = '';
    END CASE;
    
    -- Construir WHERE
    SET where_clause = CONCAT('WHERE o.order_date BETWEEN "', date_from, '" AND "', date_to, '"');
    
    IF filter_conditions IS NOT NULL AND LENGTH(filter_conditions) > 0 THEN
        SET where_clause = CONCAT(where_clause, ' AND ', filter_conditions);
    END IF;
    
    -- Construir query dinâmica
    SET sql_query = CONCAT('
        SELECT 
            CASE 
                WHEN "', group_by_field, '" = "daily" THEN DATE(o.order_date)
                WHEN "', group_by_field, '" = "weekly" THEN CONCAT(YEAR(o.order_date), "-W", WEEK(o.order_date))
                WHEN "', group_by_field, '" = "monthly" THEN CONCAT(YEAR(o.order_date), "-", LPAD(MONTH(o.order_date), 2, "0"))
                WHEN "', group_by_field, '" = "category" THEN c.name
                WHEN "', group_by_field, '" = "customer" THEN CONCAT(cust.first_name, " ", cust.last_name)
                ELSE "Total"
            END AS group_label,
            COUNT(DISTINCT o.id) AS order_count,
            SUM(o.total_amount) AS total_revenue,
            AVG(o.total_amount) AS avg_order_value,
            COUNT(DISTINCT o.customer_id) AS unique_customers
        FROM orders o
        LEFT JOIN order_items oi ON o.id = oi.order_id
        LEFT JOIN products p ON oi.product_id = p.id
        LEFT JOIN categories c ON p.category_id = c.id
        LEFT JOIN customers cust ON o.customer_id = cust.id
        ', where_clause, '
        ', group_by_clause, '
        ORDER BY total_revenue DESC
    ');
    
    -- Executar query dinâmica
    SET @sql = sql_query;
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
    
END //
DELIMITER ;

-- Exemplos de uso
CALL DynamicSalesReport('2024-01-01', '2024-01-31', 'daily', 'o.status = "completed"');
CALL DynamicSalesReport('2024-01-01', '2024-12-31', 'monthly', 'o.total_amount > 100');
CALL DynamicSalesReport('2024-01-01', '2024-01-31', 'category', NULL);
```

### **🔐 Procedures de Segurança:**

#### **Auditoria de Login:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE LogUserLogin(
    IN user_email VARCHAR(200),
    IN login_ip VARCHAR(45),
    IN user_agent TEXT,
    IN login_successful BOOLEAN
)
BEGIN
    DECLARE user_id INT DEFAULT NULL;
    DECLARE risk_score INT DEFAULT 0;
    
    -- Encontrar user ID
    SELECT id INTO user_id FROM customers WHERE email = user_email;
    
    -- Calcular risk score
    -- Verificar múltiplos logins falhados
    SELECT COUNT(*) * 10 INTO risk_score
    FROM login_attempts 
    WHERE email = user_email 
    AND login_successful = FALSE
    AND attempt_time >= DATE_SUB(NOW(), INTERVAL 1 HOUR);
    
    -- Verificar novo IP
    IF NOT EXISTS (
        SELECT 1 FROM login_attempts 
        WHERE email = user_email 
        AND ip_address = login_ip 
        AND login_successful = TRUE
    ) THEN
        SET risk_score = risk_score + 25;
    END IF;
    
    -- Registrar tentativa
    INSERT INTO login_attempts (
        email, user_id, ip_address, user_agent, 
        login_successful, risk_score, attempt_time
    ) VALUES (
        user_email, user_id, login_ip, user_agent,
        login_successful, risk_score, NOW()
    );
    
    -- Bloquear conta se muitas tentativas falhadas
    IF risk_score >= 100 AND NOT login_successful THEN
        UPDATE customers 
        SET status = 'locked', locked_at = NOW()
        WHERE email = user_email;
        
        -- Notificar administradores
        INSERT INTO security_alerts (alert_type, user_email, details, created_at)
        VALUES ('ACCOUNT_LOCKED', user_email, 
                CONCAT('Account locked due to suspicious activity. Risk score: ', risk_score),
                NOW());
    END IF;
    
END //
DELIMITER ;

-- Usar no sistema de login
CALL LogUserLogin('user@example.com', '192.168.1.100', 'Mozilla/5.0...', TRUE);
```

### **⚡ Performance e Otimização:**

#### **Procedure com Análise de Performance:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE PROCEDURE OptimizedBatchUpdate()
BEGIN
    DECLARE batch_size INT DEFAULT 1000;
    DECLARE total_rows INT DEFAULT 0;
    DECLARE processed_rows INT DEFAULT 0;
    DECLARE start_time TIMESTAMP DEFAULT NOW();
    
    -- Contar total de linhas a processar
    SELECT COUNT(*) INTO total_rows 
    FROM products 
    WHERE last_updated < DATE_SUB(NOW(), INTERVAL 1 DAY);
    
    -- Log início
    INSERT INTO batch_processing_log (process_name, total_rows, start_time)
    VALUES ('OPTIMIZED_BATCH_UPDATE', total_rows, start_time);
    
    -- Loop de processamento em lotes
    batch_loop: LOOP
        -- Processar próximo lote
        UPDATE products 
        SET 
            search_keywords = CONCAT(name, ' ', COALESCE(description, '')),
            last_updated = NOW()
        WHERE last_updated < DATE_SUB(NOW(), INTERVAL 1 DAY)
        LIMIT batch_size;
        
        SET processed_rows = processed_rows + ROW_COUNT();
        
        -- Verificar se terminou
        IF ROW_COUNT() = 0 THEN
            LEAVE batch_loop;
        END IF;
        
        -- Log progresso a cada 10 lotes
        IF processed_rows % (batch_size * 10) = 0 THEN
            UPDATE batch_processing_log 
            SET processed_rows = processed_rows,
                progress_percent = ROUND(processed_rows * 100.0 / total_rows, 2)
            WHERE process_name = 'OPTIMIZED_BATCH_UPDATE'
            AND start_time = start_time;
        END IF;
        
        -- Pequena pausa para não sobrecarregar
        DO SLEEP(0.01);
        
    END LOOP;
    
    -- Log fim
    UPDATE batch_processing_log 
    SET 
        end_time = NOW(),
        processed_rows = processed_rows,
        progress_percent = 100.0,
        status = 'COMPLETED'
    WHERE process_name = 'OPTIMIZED_BATCH_UPDATE'
    AND start_time = start_time;
    
END //
DELIMITER ;
```

### **🛠️ Gerenciar Stored Procedures:**

#### **Ver Procedures Existentes:**

sqlresponse-action-icon

```sql
-- Listar todas as procedures
SHOW PROCEDURE STATUS WHERE Db = 'your_database_name';

-- Ver código de uma procedure específica  
SHOW CREATE PROCEDURE GetCustomerStats;

-- Informações detalhadas das procedures  
SELECT  
ROUTINE_NAME,  
ROUTINE_TYPE,  
DATA_TYPE,  
CREATED,  
LAST_ALTERED,  
SECURITY_TYPE,  
SQL_MODE  
FROM INFORMATION_SCHEMA.ROUTINES  
WHERE ROUTINE_SCHEMA = 'your_database_name'  
AND ROUTINE_TYPE = 'PROCEDURE'  
ORDER BY ROUTINE_NAME;

-- Ver parâmetros de uma procedure  
SELECT  
PARAMETER_NAME,  
PARAMETER_MODE,  
DATA_TYPE,  
ORDINAL_POSITION  
FROM INFORMATION_SCHEMA.PARAMETERS  
WHERE SPECIFIC_SCHEMA = 'your_database_name'  
AND SPECIFIC_NAME = 'GetCustomerStats'  
ORDER BY ORDINAL_POSITION;
```


#### **Alterar e Remover Procedures:**
```sql
-- Alterar procedure existente
DROP PROCEDURE IF EXISTS GetCustomerStats;

DELIMITER //
CREATE PROCEDURE GetCustomerStats(
    IN customer_id INT,
    OUT total_orders INT,
    OUT total_spent DECIMAL(10,2),
    OUT last_order_date DATE,
    OUT customer_tier VARCHAR(20)  -- Novo parâmetro
)
BEGIN
    SELECT 
        COUNT(id),
        COALESCE(SUM(total_amount), 0),
        MAX(order_date),
        CASE 
            WHEN COALESCE(SUM(total_amount), 0) >= 10000 THEN 'VIP'
            WHEN COALESCE(SUM(total_amount), 0) >= 5000 THEN 'Gold'
            WHEN COALESCE(SUM(total_amount), 0) >= 1000 THEN 'Silver'
            ELSE 'Bronze'
        END
    INTO total_orders, total_spent, last_order_date, customer_tier
    FROM orders 
    WHERE customer_id = customer_id AND status = 'completed';
END //
DELIMITER ;

-- Remover procedure
DROP PROCEDURE GetCustomerStats;

-- Remover se existir (não dá erro se não existir)
DROP PROCEDURE IF EXISTS GetCustomerStats;
````

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Sistema de Comissões:**

sqlresponse-action-icon

```sql
-- Procedure para calcular comissões de vendedores
DELIMITER //
CREATE PROCEDURE CalculateSalesCommissions(
    IN calculation_month INT,
    IN calculation_year INT
)
BEGIN
    DECLARE done INT DEFAULT FALSE;
    DECLARE salesperson_id INT;
    DECLARE monthly_sales DECIMAL(12,2);
    DECLARE commission_rate DECIMAL(5,4);
    DECLARE commission_amount DECIMAL(10,2);
    DECLARE base_salary DECIMAL(10,2);
    
    DECLARE sales_cursor CURSOR FOR
        SELECT 
            s.id,
            s.base_salary,
            COALESCE(SUM(o.total_amount), 0) as sales_total
        FROM salespeople s
        LEFT JOIN orders o ON s.id = o.salesperson_id
            AND YEAR(o.order_date) = calculation_year
            AND MONTH(o.order_date) = calculation_month
            AND o.status = 'completed'
        GROUP BY s.id, s.base_salary;
    
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    -- Limpar comissões do mês
    DELETE FROM sales_commissions 
    WHERE commission_month = calculation_month 
    AND commission_year = calculation_year;
    
    OPEN sales_cursor;
    
    commission_loop: LOOP
        FETCH sales_cursor INTO salesperson_id, base_salary, monthly_sales;
        
        IF done THEN
            LEAVE commission_loop;
        END IF;
        
        -- Calcular taxa de comissão baseada em escalões
        CASE
            WHEN monthly_sales >= 50000 THEN SET commission_rate = 0.08;  -- 8%
            WHEN monthly_sales >= 25000 THEN SET commission_rate = 0.06;  -- 6%
            WHEN monthly_sales >= 10000 THEN SET commission_rate = 0.04;  -- 4%
            WHEN monthly_sales >= 5000 THEN SET commission_rate = 0.02;   -- 2%
            ELSE SET commission_rate = 0.00;  -- 0%
        END CASE;
        
        SET commission_amount = monthly_sales * commission_rate;
        
        -- Inserir registo de comissão
        INSERT INTO sales_commissions (
            salesperson_id,
            commission_month,
            commission_year,
            monthly_sales,
            commission_rate,
            commission_amount,
            base_salary,
            total_earnings,
            calculation_date
        ) VALUES (
            salesperson_id,
            calculation_month,
            calculation_year,
            monthly_sales,
            commission_rate,
            commission_amount,
            base_salary,
            base_salary + commission_amount,
            NOW()
        );
        
    END LOOP;
    
    CLOSE sales_cursor;
    
    -- Retornar resumo
    SELECT 
        COUNT(*) as salespeople_count,
        SUM(monthly_sales) as total_sales,
        SUM(commission_amount) as total_commissions,
        AVG(commission_rate) as avg_commission_rate
    FROM sales_commissions
    WHERE commission_month = calculation_month
    AND commission_year = calculation_year;
    
END //
DELIMITER ;

-- Calcular comissões para Janeiro 2024
CALL CalculateSalesCommissions(1, 2024);
```

#### **Exercício 2 - Sistema de Alertas de Stock:**

sqlresponse-action-icon

```sql
-- Procedure para gerar alertas de stock
DELIMITER //
CREATE PROCEDURE GenerateStockAlerts()
BEGIN
    DECLARE product_id INT;
    DECLARE product_name VARCHAR(200);
    DECLARE current_stock INT;
    DECLARE min_level INT;
    DECLARE supplier_email VARCHAR(200);
    DECLARE alert_message TEXT;
    DECLARE alert_priority ENUM('LOW', 'MEDIUM', 'HIGH', 'CRITICAL');
    
    DECLARE done INT DEFAULT FALSE;
    DECLARE stock_cursor CURSOR FOR
        SELECT 
            p.id,
            p.name,
            p.stock_quantity,
            p.min_stock_level,
            s.email as supplier_email
        FROM products p
        LEFT JOIN suppliers s ON p.supplier_id = s.id
        WHERE p.status = 'active'
        AND (p.stock_quantity <= p.min_stock_level OR p.stock_quantity = 0);
    
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    -- Limpar alertas antigos (> 7 dias)
    DELETE FROM stock_alerts 
    WHERE created_at < DATE_SUB(NOW(), INTERVAL 7 DAY)
    AND status = 'resolved';
    
    OPEN stock_cursor;
    
    stock_loop: LOOP
        FETCH stock_cursor INTO product_id, product_name, current_stock, min_level, supplier_email;
        
        IF done THEN
            LEAVE stock_loop;
        END IF;
        
        -- Determinar prioridade e mensagem
        IF current_stock = 0 THEN
            SET alert_priority = 'CRITICAL';
            SET alert_message = CONCAT('STOCK OUT: ', product_name, ' is completely out of stock!');
        ELSEIF current_stock <= min_level * 0.25 THEN
            SET alert_priority = 'HIGH';
            SET alert_message = CONCAT('URGENT: ', product_name, ' has very low stock (', current_stock, ' units remaining)');
        ELSEIF current_stock <= min_level * 0.5 THEN
            SET alert_priority = 'MEDIUM';
            SET alert_message = CONCAT('WARNING: ', product_name, ' is below minimum stock level (', current_stock, '/', min_level, ')');
        ELSE
            SET alert_priority = 'LOW';
            SET alert_message = CONCAT('NOTICE: ', product_name, ' approaching minimum stock level (', current_stock, '/', min_level, ')');
        END IF;
        
        -- Verificar se já existe alerta ativo para este produto
        IF NOT EXISTS (
            SELECT 1 FROM stock_alerts 
            WHERE product_id = product_id 
            AND status = 'active'
            AND DATE(created_at) = CURDATE()
        ) THEN
    
-- Inserir novo alerta  
INSERT INTO stock_alerts (  
product_id,  
product_name,  
current_stock,  
min_stock_level,  
supplier_email,  
alert_message,  
priority_level,  
status,  
created_at  
) VALUES (  
product_id,  
product_name,  
current_stock,  
min_level,  
supplier_email,  
alert_message,  
alert_priority,  
'active',  
NOW()  
);
```