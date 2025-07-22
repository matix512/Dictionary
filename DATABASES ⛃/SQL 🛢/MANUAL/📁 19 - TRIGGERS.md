### **🔥 O que são Triggers?**

#### **Definição:**

> Triggers são procedimentos especiais que executam automaticamente (são "disparados") em resposta a eventos específicos numa tabela ou vista. Não podem ser chamados diretamente - apenas executam quando o evento associado ocorre.

#### **Características dos Triggers:**

```text
🔄 Automáticos - Executam sem intervenção manual
⚡ Transparentes - Aplicação não precisa saber que existem
🛡️ Integridade - Garantem regras de negócio
📊 Auditoria - Registam mudanças automaticamente
⚠️ Invisíveis - Difíceis de debuggar e rastrear
🐌 Performance - Podem tornar operações mais lentas
```

#### **Tipos de Triggers:**

```text
⏰ Timing:
   - BEFORE: Antes da operação (validação, transformação)
   - AFTER: Depois da operação (log, notificação)
   
🎯 Eventos:
   - INSERT: Quando novos registos são inseridos
   - UPDATE: Quando registos são modificados
   - DELETE: Quando registos são removidos
```

### **🆕 Criar Triggers:**

#### **Sintaxe Básica:**

```sql
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
    -- Código do trigger
END;
```

#### **Trigger BEFORE INSERT:**

```sql
-- Validação antes de inserir
DELIMITER //
CREATE TRIGGER validate_customer_before_insert
BEFORE INSERT ON customers
FOR EACH ROW
BEGIN
    -- Validar email
    IF NEW.email NOT LIKE '%@%' THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Email inválido - deve conter @';
    END IF;
    
    -- Normalizar dados
    SET NEW.email = LOWER(TRIM(NEW.email));
    SET NEW.first_name = TRIM(NEW.first_name);
    SET NEW.last_name = TRIM(NEW.last_name);
    
    -- Definir valores padrão
    IF NEW.status IS NULL THEN
        SET NEW.status = 'active';
    END IF;
    
    -- Gerar customer number se não fornecido
    IF NEW.customer_number IS NULL THEN
        SET NEW.customer_number = CONCAT('CUST', LPAD((SELECT COALESCE(MAX(id), 0) + 1 FROM customers), 6, '0'));
    END IF;
END //
DELIMITER ;

-- Teste do trigger
INSERT INTO customers (first_name, last_name, email)
VALUES ('João', ' Silva ', 'JOAO@EMAIL.COM ');

-- Resultado: email = 'joao@email.com', nomes trimmed, customer_number gerado
```

#### **Trigger AFTER INSERT:**

```sql
-- Log de auditoria após inserção
DELIMITER //
CREATE TRIGGER audit_customer_insert
AFTER INSERT ON customers
FOR EACH ROW
BEGIN
    INSERT INTO audit_log (
        table_name,
        operation,
        record_id,
        new_values,
        changed_by,
        changed_at
    ) VALUES (
        'customers',
        'INSERT',
        NEW.id,
        CONCAT('name: ', NEW.first_name, ' ', NEW.last_name, 
               ', email: ', NEW.email,
               ', status: ', NEW.status),
        USER(),
        NOW()
    );
    
    -- Atualizar contador global
    UPDATE system_stats 
    SET customer_count = customer_count + 1,
        last_updated = NOW()
    WHERE stat_name = 'total_customers';
    
    -- Enviar notificação de boas-vindas
    INSERT INTO notification_queue (
        notification_type,
        recipient_email,
        subject,
        message,
        priority,
        created_at
    ) VALUES (
        'WELCOME_EMAIL',
        NEW.email,
        'Bem-vindo!',
        CONCAT('Olá ', NEW.first_name, ', obrigado por se registar!'),
        'NORMAL',
        NOW()
    );
END //
DELIMITER ;
```

### **🔧 Triggers de UPDATE:**

#### **BEFORE UPDATE - Validação e Transformação:**

```sql
DELIMITER //
CREATE TRIGGER validate_product_before_update
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    -- Verificar se preço não diminui mais de 50%
    IF NEW.price < OLD.price * 0.5 THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Preço não pode diminuir mais de 50% de uma vez';
    END IF;
    
    -- Atualizar timestamp automaticamente
    SET NEW.updated_at = NOW();
    
    -- Se stock mudou, atualizar data da última movimentação
    IF NEW.stock_quantity != OLD.stock_quantity THEN
        SET NEW.last_stock_movement = NOW();
    END IF;
    
    -- Calcular margem automaticamente se cost_price mudou
    IF NEW.cost_price != OLD.cost_price THEN
        SET NEW.profit_margin = ((NEW.price - NEW.cost_price) / NEW.price) * 100;
    END IF;
    
    -- Validar dados críticos
    IF NEW.price <= 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Preço deve ser positivo';
    END IF;
    
    IF NEW.stock_quantity < 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Stock não pode ser negativo';  
	END IF;


-- Auto-atualizar status baseado no stock
	IF NEW.stock_quantity = 0 AND OLD.stock_quantity > 0 THEN
	    SET NEW.status = 'out_of_stock';
	ELSEIF NEW.stock_quantity > 0 AND OLD.status = 'out_of_stock' THEN
	    SET NEW.status = 'active';
	END IF;
END //
DELIMITER;
```

#### **AFTER UPDATE - Auditoria e Notificações:**

```sql
DELIMITER //
CREATE TRIGGER audit_product_changes
AFTER UPDATE ON products
FOR EACH ROW
BEGIN
    DECLARE changes TEXT DEFAULT '';
    
    -- Construir string de mudanças
    IF OLD.name != NEW.name THEN
        SET changes = CONCAT(changes, 'name: "', OLD.name, '" -> "', NEW.name, '"; ');
    END IF;
    
    IF OLD.price != NEW.price THEN
        SET changes = CONCAT(changes, 'price: ', OLD.price, ' -> ', NEW.price, '; ');
    END IF;
    
    IF OLD.stock_quantity != NEW.stock_quantity THEN
        SET changes = CONCAT(changes, 'stock: ', OLD.stock_quantity, ' -> ', NEW.stock_quantity, '; ');
        
        -- Log movimento de stock separadamente
        INSERT INTO stock_movements (
            product_id,
            movement_type,
            old_quantity,
            new_quantity,
            difference,
            movement_date,
            notes
        ) VALUES (
            NEW.id,
            CASE 
                WHEN NEW.stock_quantity > OLD.stock_quantity THEN 'INCREASE'
                ELSE 'DECREASE'
            END,
            OLD.stock_quantity,
            NEW.stock_quantity,
            NEW.stock_quantity - OLD.stock_quantity,
            NOW(),
            'Automatic stock movement logged by trigger'
        );
    END IF;
    
    -- Se houve mudanças, registar no audit log
    IF LENGTH(changes) > 0 THEN
        INSERT INTO audit_log (
            table_name,
            operation,
            record_id,
            old_values,
            new_values,
            changes,
            changed_by,
            changed_at
        ) VALUES (
            'products',
            'UPDATE',
            NEW.id,
            CONCAT('name: ', OLD.name, ', price: ', OLD.price, ', stock: ', OLD.stock_quantity),
            CONCAT('name: ', NEW.name, ', price: ', NEW.price, ', stock: ', NEW.stock_quantity),
            changes,
            USER(),
            NOW()
        );
    END IF;
    
    -- Alertas de stock baixo
    IF NEW.stock_quantity <= NEW.min_stock_level AND OLD.stock_quantity > NEW.min_stock_level THEN
        INSERT INTO stock_alerts (
            product_id,
            alert_type,
            message,
            priority_level,
            created_at
        ) VALUES (
            NEW.id,
            'LOW_STOCK',
            CONCAT('Product "', NEW.name, '" is now below minimum stock level (', NEW.stock_quantity, '/', NEW.min_stock_level, ')'),
            CASE 
                WHEN NEW.stock_quantity = 0 THEN 'CRITICAL'
                WHEN NEW.stock_quantity <= NEW.min_stock_level * 0.5 THEN 'HIGH'
                ELSE 'MEDIUM'
            END,
            NOW()
        );
    END IF;
    
END //
DELIMITER ;
```

### **🗑️ Triggers de DELETE:**

#### **BEFORE DELETE - Validação:**

```sql
DELIMITER //
CREATE TRIGGER prevent_customer_delete
BEFORE DELETE ON customers
FOR EACH ROW
BEGIN
    DECLARE order_count INT DEFAULT 0;
    
    -- Verificar se cliente tem pedidos
    SELECT COUNT(*) INTO order_count
    FROM orders 
    WHERE customer_id = OLD.id;
    
    -- Não permitir deletar cliente com histórico
    IF order_count > 0 THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = CONCAT('Não é possível apagar cliente com ', order_count, ' pedidos. Use soft delete.');
    END IF;
    
    -- Verificar se é um cliente VIP
    IF OLD.customer_tier = 'VIP' THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Clientes VIP não podem ser apagados diretamente. Contacte administrador.';
    END IF;
    
END //
DELIMITER ;
```

#### **AFTER DELETE - Cleanup e Auditoria:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE TRIGGER cleanup_after_customer_delete
AFTER DELETE ON customers
FOR EACH ROW
BEGIN
    -- Log da eliminação
    INSERT INTO audit_log (
        table_name,
        operation,
        record_id,
        old_values,
        changed_by,
        changed_at
    ) VALUES (
        'customers',
        'DELETE',
        OLD.id,
        CONCAT('name: ', OLD.first_name, ' ', OLD.last_name, 
               ', email: ', OLD.email,
               ', tier: ', OLD.customer_tier),
        USER(),
        NOW()
    );
    
    -- Cleanup de tabelas relacionadas
    DELETE FROM customer_preferences WHERE customer_id = OLD.id;
    DELETE FROM shopping_carts WHERE customer_id = OLD.id;
    DELETE FROM wishlist_items WHERE customer_id = OLD.id;
    
    -- Atualizar estatísticas
    UPDATE system_stats 
    SET customer_count = customer_count - 1,
        last_updated = NOW()
    WHERE stat_name = 'total_customers';
    
    -- Notificar administradores de clientes VIP deletados
    IF OLD.customer_tier IN ('VIP', 'Gold') THEN
        INSERT INTO admin_notifications (
            notification_type,
            message,
            priority,
            created_at
        ) VALUES (
            'CUSTOMER_DELETED',
            CONCAT('Cliente ', OLD.customer_tier, ' eliminado: ', OLD.first_name, ' ', OLD.last_name, ' (', OLD.email, ')'),
            'HIGH',
            NOW()
        );
    END IF;
    
END //
DELIMITER ;
```

### **💰 Triggers para Sistema Financeiro:**

#### **Controlo de Transações:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE TRIGGER validate_financial_transaction
BEFORE INSERT ON transactions
FOR EACH ROW
BEGIN
    DECLARE account_balance DECIMAL(12,2);
    DECLARE account_status VARCHAR(20);
    DECLARE daily_limit DECIMAL(12,2);
    DECLARE daily_spent DECIMAL(12,2);
    
    -- Verificar se conta existe e está ativa
    SELECT balance, status, daily_transaction_limit
    INTO account_balance, account_status, daily_limit
    FROM accounts 
    WHERE id = NEW.from_account_id;
    
    IF account_status IS NULL THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Conta de origem não encontrada';
    END IF;
    
    IF account_status != 'active' THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Conta de origem não está ativa';
    END IF;
    
    -- Verificar saldo suficiente para débitos
    IF NEW.transaction_type = 'DEBIT' AND NEW.amount > account_balance THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Saldo insuficiente';
    END IF;
    
    -- Verificar limite diário
    SELECT COALESCE(SUM(amount), 0) INTO daily_spent
    FROM transactions 
    WHERE from_account_id = NEW.from_account_id
    AND transaction_type = 'DEBIT'
    AND DATE(transaction_date) = CURDATE();
    
    IF daily_spent + NEW.amount > daily_limit THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = CONCAT('Limite diário excedido: ', daily_spent + NEW.amount, ' > ', daily_limit);
    END IF;
    
    -- Gerar número de referência único
    SET NEW.reference_number = CONCAT(
        DATE_FORMAT(NOW(), '%Y%m%d'),
        LPAD(NEW.from_account_id, 4, '0'),
        LPAD((SELECT COALESCE(MAX(id), 0) + 1 FROM transactions), 6, '0')
    );
    
    -- Definir status inicial
    IF NEW.amount > 10000 THEN
        SET NEW.status = 'pending_approval';  -- Transações grandes precisam aprovação
    ELSE
        SET NEW.status = 'completed';
    END IF;
    
END //
DELIMITER ;

-- Trigger para atualizar saldos após transação
DELIMITER //
CREATE TRIGGER update_account_balances
AFTER INSERT ON transactions
FOR EACH ROW
BEGIN
    -- Só atualizar se transação foi completada
    IF NEW.status = 'completed' THEN
        
        -- Debitar conta de origem
        IF NEW.transaction_type IN ('DEBIT', 'TRANSFER') THEN
            UPDATE accounts 
            SET balance = balance - NEW.amount,
                last_transaction_date = NOW()
            WHERE id = NEW.from_account_id;
        END IF;
        
        -- Creditar conta de destino (para transferências)
        IF NEW.transaction_type = 'TRANSFER' AND NEW.to_account_id IS NOT NULL THEN
            UPDATE accounts 
            SET balance = balance + NEW.amount,
                last_transaction_date = NOW()
            WHERE id = NEW.to_account_id;
        END IF;
        
        -- Creditar conta (para depósitos)
        IF NEW.transaction_type = 'CREDIT' THEN
            UPDATE accounts 
            SET balance = balance + NEW.amount,
                last_transaction_date = NOW()
            WHERE id = NEW.from_account_id;
        END IF;
        
    END IF;
    
    -- Log da transação para auditoria
    INSERT INTO financial_audit_log (
        transaction_id,
        account_id,
        action,
        amount,
        balance_before,
        balance_after,
        timestamp
    ) VALUES (
        NEW.id,
        NEW.from_account_id,
        NEW.transaction_type,
        NEW.amount,
        -- Calcular saldo anterior (simplificado)
        (SELECT balance FROM accounts WHERE id = NEW.from_account_id) + 
        CASE WHEN NEW.transaction_type IN ('DEBIT', 'TRANSFER') THEN NEW.amount ELSE -NEW.amount END,
        (SELECT balance FROM accounts WHERE id = NEW.from_account_id),
        NOW()
    );
    
END //
DELIMITER ;
```

### **📊 Triggers para Análise e Relatórios:**

#### **Agregações Automáticas:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE TRIGGER update_sales_statistics
AFTER INSERT ON order_items
FOR EACH ROW
BEGIN
    DECLARE order_date DATE;
    DECLARE customer_id INT;
    
    -- Obter informações do pedido
    SELECT o.order_date, o.customer_id
    INTO order_date, customer_id
    FROM orders o
    WHERE o.id = NEW.order_id;
    
    -- Atualizar estatísticas de produto
    INSERT INTO product_statistics (
        product_id, 
        sales_date, 
        quantity_sold, 
        revenue_generated,
        orders_count
    )
    VALUES (
        NEW.product_id,
        order_date,
        NEW.quantity,
        NEW.quantity * NEW.unit_price,
        1
    )
    ON DUPLICATE KEY UPDATE
        quantity_sold = quantity_sold + NEW.quantity,
        revenue_generated = revenue_generated + (NEW.quantity * NEW.unit_price),
        orders_count = orders_count + 1,
        updated_at = NOW();
    
    -- Atualizar estatísticas mensais
    INSERT INTO monthly_sales_summary (
        year_month,
        total_orders,
        total_revenue,
        unique_customers,
        items_sold
    )
    VALUES (
        DATE_FORMAT(order_date, '%Y-%m'),
        1,
        NEW.quantity * NEW.unit_price,
        1,  -- Será ajustado pelo DUPLICATE KEY UPDATE
        NEW.quantity
    )
    ON DUPLICATE KEY UPDATE
        total_revenue = total_revenue + (NEW.quantity * NEW.unit_price),
        items_sold = items_sold + NEW.quantity,
        updated_at = NOW();
    
    -- Atualizar ranking de produtos
    UPDATE products 
    SET 
        total_sold = total_sold + NEW.quantity,
        total_revenue = total_revenue + (NEW.quantity * NEW.unit_price),
        times_ordered = times_ordered + 1,
        last_sold_date = order_date
    WHERE id = NEW.product_id;
    
END //
DELIMITER ;
```

### **⚠️ Triggers de Segurança:**

#### **Detecção de Atividade Suspeita:**

sqlresponse-action-icon

```sql
DELIMITER //
CREATE TRIGGER detect_suspicious_login
AFTER INSERT ON login_attempts
FOR EACH ROW
BEGIN
    DECLARE failed_attempts INT DEFAULT 0;
    DECLARE different_ips INT DEFAULT 0;
    DECLARE risk_level VARCHAR(20) DEFAULT 'LOW';
    
    -- Contar tentativas falhadas na última hora
    SELECT COUNT(*) INTO failed_attempts
    FROM login_attempts
    WHERE user_email = NEW.user_email
    AND success = FALSE
    AND attempt_time >= DATE_SUB(NOW(), INTERVAL 1 HOUR);
    
    -- Contar IPs diferentes nas últimas 24 horas
    SELECT COUNT(DISTINCT ip_address) INTO different_ips
    FROM login_attempts
    WHERE user_email = NEW.user_email
    AND attempt_time >= DATE_SUB(NOW(), INTERVAL 24 HOUR);
    
    -- Calcular nível de risco
    IF failed_attempts >= 10 OR different_ips >= 5 THEN
        SET risk_level = 'CRITICAL';
    ELSEIF failed_attempts >= 5 OR different_ips >= 3 THEN
        SET risk_level = 'HIGH';
    ELSEIF failed_attempts >= 3 OR different_ips >= 2 THEN
        SET risk_level = 'MEDIUM';
    END IF;
    
    -- Se risco alto, criar alerta de segurança
    IF risk_level IN ('HIGH', 'CRITICAL') THEN
        INSERT INTO security_alerts (
            alert_type,
            user_email,
            ip_address,
            risk_level,
            details,
            created_at
        ) VALUES (
            'SUSPICIOUS_LOGIN_PATTERN',
            NEW.user_email,
            NEW.ip_address,
            risk_level,
            CONCAT('Failed attempts: ', failed_attempts, ', Different IPs: ', different_ips),
            NOW()
        );
        
        -- Se crítico, bloquear conta temporariamente
        IF risk_level = 'CRITICAL' THEN
            UPDATE customers 
            SET status = 'temporarily_locked',
                locked_until = DATE_ADD(NOW(), INTERVAL 1 HOUR),
                lock_reason = 'Suspicious login activity detected'
            WHERE email = NEW.user_email;
        END IF;
    END IF;
    
END //
DELIMITER ;
```

### **🔧 Gestão de Triggers:**

#### **Ver Triggers Existentes:**

sqlresponse-action-icon

```sql
-- Listar todos os triggers
SHOW TRIGGERS;

-- Triggers de uma tabela específica
SHOW TRIGGERS WHERE `Table` = 'customers';

-- Informações detalhadas
SELECT 
    TRIGGER_NAME,
    EVENT_MANIPULATION,
    EVENT_OBJECT_TABLE,
    ACTION_TIMING,
    ACTION_STATEMENT
FROM INFORMATION_SCHEMA.TRIGGERS
WHERE TRIGGER_SCHEMA = 'your_database_name'
ORDER BY EVENT_OBJECT_TABLE, ACTION_TIMING, EVENT_MANIPULATION;

-- Ver código de um trigger específico
SHOW CREATE TRIGGER audit_customer_insert;
```

#### **Alterar e Remover Triggers:**

sqlresponse-action-icon

```sql
-- Remover trigger
DROP TRIGGER IF EXISTS audit_customer_insert;

-- Recriar com alterações
DELIMITER //
CREATE TRIGGER audit_customer_insert_v2
AFTER INSERT ON customers
FOR EACH ROW
BEGIN
    -- Nova lógica melhorada
    INSERT INTO audit_log (
        table_name,
        operation,
        record_id,
        new_values,
        changed_by,
        changed_at,
        version  -- Nova coluna
    ) VALUES (
        'customers',
        'INSERT',
        NEW.id,
        JSON_OBJECT(
            'name', CONCAT(NEW.first_name, ' ', NEW.last_name),
            'email', NEW.email,
            'status', NEW.status
        ),
        USER(),
        NOW(),
        2  -- Versão do trigger
    );
END //
DELIMITER ;

-- Desabilitar todos os triggers de uma tabela (MySQL 8.0+)
ALTER TABLE customers DISABLE TRIGGER ALL;
ALTER TABLE customers ENABLE TRIGGER ALL;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Sistema de Pontos de Fidelidade:**

sqlresponse-action-icon

```sql
-- Trigger para calcular pontos automaticamente
DELIMITER //
CREATE TRIGGER calculate_loyalty_points
AFTER INSERT ON orders
FOR EACH ROW
BEGIN
    DECLARE points_earned INT DEFAULT 0;
    DECLARE customer_tier VARCHAR(20);
    DECLARE multiplier DECIMAL(3,2) DEFAULT 1.00;
    
    -- Obter tier do cliente
    SELECT customer_tier INTO customer_tier
    FROM customers 
    WHERE id = NEW.customer_id;
    
    -- Definir multiplicador baseado no tier
    CASE customer_tier
        WHEN 'VIP' THEN SET multiplier = 2.00;
        WHEN 'Gold' THEN SET multiplier = 1.50;
        WHEN 'Silver' THEN SET multiplier = 1.25;
        ELSE SET multiplier = 1.00;
    END CASE;
    
    -- Calcular pontos (
```