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

