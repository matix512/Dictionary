### **🎯 Data Manipulation Language (DML):**

#### **O que é DML?**

> DML são comandos SQL usados para manipular dados dentro das tabelas. Incluem INSERT (criar), UPDATE (modificar), DELETE (remover) e SELECT (consultar).

#### **Comandos Principais:**

```text
➕ INSERT - Inserir novos registos
🔧 UPDATE - Modificar registos existentes
❌ DELETE - Remover registos
🔄 REPLACE - Substituir registos (MySQL)
🔀 MERGE/UPSERT - Inserir ou atualizar
```

### **➕ INSERT - Inserir Dados:**

#### **INSERT Básico:**

```sql
-- Sintaxe básica
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);

-- Exemplo simples
INSERT INTO customers (first_name, last_name, email)
VALUES ('João', 'Silva', 'joao@email.com');

-- Inserir sem especificar colunas (cuidado!)
INSERT INTO customers 
VALUES (1, 'Maria', 'Santos', 'maria@email.com', '2024-01-15');
```

#### **INSERT Múltiplo:**

```sql
-- Múltiplos valores de uma vez
INSERT INTO products (name, price, category, stock)
VALUES 
    ('Laptop Dell', 899.99, 'Electronics', 10),
    ('Mouse Wireless', 29.99, 'Electronics', 50),
    ('Cadeira Ergonómica', 199.99, 'Furniture', 25),
    ('Mesa de Escritório', 299.99, 'Furniture', 15);

-- Melhor performance que múltiplos INSERTs separados
```

#### **INSERT com Subquery:**

```sql
-- Inserir dados de outra tabela
INSERT INTO customers_backup (first_name, last_name, email)
SELECT first_name, last_name, email 
FROM customers 
WHERE created_at >= '2024-01-01';

-- Inserir dados agregados
INSERT INTO monthly_sales (month, total_revenue)
SELECT 
    DATE_FORMAT(sale_date, '%Y-%m') AS month,
    SUM(quantity * price) AS total_revenue
FROM sales
WHERE YEAR(sale_date) = 2024
GROUP BY DATE_FORMAT(sale_date, '%Y-%m');

-- Inserir com JOINs
INSERT INTO customer_summary (customer_id, total_orders, total_spent)
SELECT 
    c.id,
    COUNT(o.id) AS total_orders,
    COALESCE(SUM(o.total), 0) AS total_spent
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
GROUP BY c.id;
```

#### **INSERT com Valores Especiais:**

```sql
-- Usando funções
INSERT INTO logs (user_id, action, timestamp)
VALUES (1, 'login', NOW());

-- Com NULL explícito
INSERT INTO customers (first_name, last_name, phone, email)
VALUES ('Ana', 'Costa', NULL, 'ana@email.com');

-- Com DEFAULT
INSERT INTO products (name, price, status)
VALUES ('Novo Produto', 49.99, DEFAULT);

-- Com AUTO_INCREMENT (omitir o campo)
INSERT INTO customers (first_name, last_name, email)
VALUES ('Pedro', 'Oliveira', 'pedro@email.com');
-- id será automaticamente gerado
```

### **🔧 UPDATE - Modificar Dados:**

#### **UPDATE Básico:**

```sql
-- Sintaxe básica
UPDATE table_name 
SET column1 = value1, column2 = value2
WHERE condition;

-- Exemplo simples
UPDATE customers 
SET phone = '912345678' 
WHERE id = 1;

-- Múltiplas colunas
UPDATE products 
SET price = 799.99, stock = stock - 1, updated_at = NOW()
WHERE id = 5;
```

#### **UPDATE com Condições:**

```sql
-- Atualização condicional
UPDATE customers 
SET status = 'premium'
WHERE total_spent > 1000;

-- Com múltiplas condições
UPDATE products 
SET status = 'discontinued'
WHERE stock = 0 AND last_sale_date < DATE_SUB(NOW(), INTERVAL 6 MONTH);

-- Update com IN
UPDATE customers 
SET region = 'North'
WHERE city IN ('Porto', 'Braga', 'Viana do Castelo');

-- Update com subquery
UPDATE products 
SET category = 'Best Seller'
WHERE id IN (
    SELECT product_id 
    FROM sales 
    GROUP BY product_id 
    HAVING SUM(quantity) > 100
);
```

#### **UPDATE com Cálculos:**

```sql
-- Incrementar/decrementar
UPDATE products 
SET stock = stock + 50 
WHERE category = 'Electronics';

-- Cálculos percentuais
UPDATE products 
SET price = price * 1.10  -- Aumento de 10%
WHERE category = 'Luxury';

-- Update com CASE
UPDATE customers 
SET discount_rate = CASE 
    WHEN total_spent > 5000 THEN 0.15
    WHEN total_spent > 1000 THEN 0.10
    WHEN total_spent > 500 THEN 0.05
    ELSE 0.02
END;

-- Update com funções de string
UPDATE customers 
SET email = LOWER(email),
    full_name = CONCAT(first_name, ' ', last_name);
```

#### **UPDATE com JOINs:**

```sql
-- MySQL/SQL Server syntax
UPDATE customers c
INNER JOIN orders o ON c.id = o.customer_id
SET c.last_order_date = o.order_date
WHERE o.order_date = (
    SELECT MAX(order_date) 
    FROM orders o2 
    WHERE o2.customer_id = c.id
);

-- Atualizar com dados de outra tabela
UPDATE products p
INNER JOIN categories cat ON p.category_id = cat.id
SET p.category_name = cat.name;

-- Update com LEFT JOIN
UPDATE customers c
LEFT JOIN orders o ON c.id = o.customer_id
SET c.has_orders = CASE WHEN o.id IS NOT NULL THEN TRUE ELSE FALSE END;
```

### **❌ DELETE - Remover Dados:**

#### **DELETE Básico:**

```sql
-- Sintaxe básica
DELETE FROM table_name 
WHERE condition;

-- Exemplo simples
DELETE FROM customers 
WHERE id = 1;

-- Delete com múltiplas condições
DELETE FROM products 
WHERE stock = 0 AND status = 'discontinued';
```

#### **DELETE com Subqueries:**

```sql
-- Delete com subquery
DELETE FROM customers 
WHERE id IN (
    SELECT customer_id 
    FROM temp_inactive_customers
);

-- Delete registos antigos
DELETE FROM logs 
WHERE created_at < DATE_SUB(NOW(), INTERVAL 90 DAY);

-- Delete duplicados (manter apenas o primeiro)
DELETE c1 FROM customers c1
INNER JOIN customers c2 
WHERE c1.id > c2.id AND c1.email = c2.email;
```

#### **DELETE vs TRUNCATE:**

sqlresponse-action-icon

```sql
-- DELETE - remove linha por linha, permite WHERE, pode ser rollback
DELETE FROM temp_table WHERE status = 'processed';

-- TRUNCATE - remove todas as linhas rapidamente, reset AUTO_INCREMENT
TRUNCATE TABLE temp_table;
-- Mais rápido que DELETE sem WHERE, mas não permite condições
```

### **🔄 Operações Avançadas:**

#### **REPLACE (MySQL):**

sqlresponse-action-icon

```sql
-- REPLACE = DELETE + INSERT se a chave já existe
REPLACE INTO customers (id, first_name, last_name, email)
VALUES (1, 'João', 'Silva', 'joao.novo@email.com');

-- Se id=1 existe, deleta e insere novo
-- Se id=1 não existe, apenas insere
```

#### **INSERT ... ON DUPLICATE KEY UPDATE (MySQL):**

sqlresponse-action-icon

```sql
-- Inserir ou atualizar se chave duplicada
INSERT INTO customer_stats (customer_id, total_orders, total_spent)
VALUES (1, 1, 100.00)
ON DUPLICATE KEY UPDATE 
    total_orders = total_orders + VALUES(total_orders),
    total_spent = total_spent + VALUES(total_spent);

-- Útil para contadores e acumuladores
```

#### **UPSERT (PostgreSQL/SQL Server):**

sqlresponse-action-icon

```sql
-- PostgreSQL
INSERT INTO customers (id, first_name, last_name, email)
VALUES (1, 'João', 'Silva', 'joao@email.com')
ON CONFLICT (id) 
DO UPDATE SET 
    first_name = EXCLUDED.first_name,  
last_name = EXCLUDED.last_name,  
email = EXCLUDED.email,  
updated_at = NOW();

-- SQL Server MERGE  
MERGE customers AS target  
USING (SELECT 1 as id, 'João' as first_name, 'Silva' as last_name, '[joao@email.com](mailto:joao@email.com)' as email) AS source  
ON target.id = source.id  
WHEN MATCHED THEN  
UPDATE SET first_name = source.first_name, last_name = source.last_name  
WHEN NOT MATCHED THEN  
INSERT (id, first_name, last_name, email)  
VALUES (source.id, source.first_name, source.last_name, source.email);

textresponse-action-icon

````text

### **🎯 Casos Práticos Completos:**

#### **Sistema de E-commerce - Processar Pedido:**
```sql
-- Cenário: Cliente faz um pedido, precisa atualizar stock e criar registos

START TRANSACTION;

-- 1. Criar o pedido
INSERT INTO orders (customer_id, order_date, status, total)
VALUES (1, NOW(), 'pending', 0);

SET @order_id = LAST_INSERT_ID();

-- 2. Adicionar itens do pedido
INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES 
    (@order_id, 10, 2, 29.99),
    (@order_id, 15, 1, 199.99);

-- 3. Atualizar stock dos produtos
UPDATE products 
SET stock = stock - 2 
WHERE id = 10 AND stock >= 2;

UPDATE products 
SET stock = stock - 1 
WHERE id = 15 AND stock >= 1;

-- 4. Calcular total do pedido
UPDATE orders 
SET total = (
    SELECT SUM(quantity * price) 
    FROM order_items 
    WHERE order_id = @order_id
)
WHERE id = @order_id;

-- 5. Atualizar estatísticas do cliente
UPDATE customers 
SET 
    total_orders = total_orders + 1,
    total_spent = total_spent + (SELECT total FROM orders WHERE id = @order_id),
    last_order_date = NOW()
WHERE id = 1;

COMMIT;
````

#### **Sistema de Auditoria - Log de Mudanças:**

sqlresponse-action-icon

```sql
-- Trigger para log automático de mudanças (conceito)
-- Antes de UPDATE em products, inserir em audit_log

DELIMITER //
CREATE TRIGGER products_audit_update
AFTER UPDATE ON products
FOR EACH ROW
BEGIN
    INSERT INTO audit_log (
        table_name, record_id, action, 
        old_values, new_values, 
        changed_by, changed_at
    ) VALUES (
        'products', NEW.id, 'UPDATE',
        CONCAT('price:', OLD.price, ',stock:', OLD.stock),
        CONCAT('price:', NEW.price, ',stock:', NEW.stock),
        @user_id, NOW()
    );
END //
DELIMITER ;
```

#### **Limpeza e Manutenção de Dados:**

sqlresponse-action-icon

```sql
-- Script de limpeza semanal

-- 1. Remover logs antigos
DELETE FROM access_logs 
WHERE created_at < DATE_SUB(NOW(), INTERVAL 30 DAY);

-- 2. Remover carrinho abandonado
DELETE FROM cart_items 
WHERE cart_id IN (
    SELECT id FROM shopping_carts 
    WHERE updated_at < DATE_SUB(NOW(), INTERVAL 7 DAY)
    AND status = 'abandoned'
);

-- 3. Atualizar status de produtos sem stock
UPDATE products 
SET status = 'out_of_stock'
WHERE stock = 0 AND status = 'active';

-- 4. Arquivar pedidos antigos
INSERT INTO orders_archive 
SELECT * FROM orders 
WHERE order_date < DATE_SUB(NOW(), INTERVAL 1 YEAR)
AND status IN ('completed', 'cancelled');

DELETE FROM orders 
WHERE order_date < DATE_SUB(NOW(), INTERVAL 1 YEAR)
AND status IN ('completed', 'cancelled');

-- 5. Atualizar estatísticas de produtos
UPDATE products p
SET total_sold = (
    SELECT COALESCE(SUM(oi.quantity), 0)
    FROM order_items oi
    INNER JOIN orders o ON oi.order_id = o.id
    WHERE oi.product_id = p.id 
    AND o.status = 'completed'
);
```

### **📊 Operações em Lote (Batch Operations):**

#### **INSERT em Lotes:**

sqlresponse-action-icon

```sql
-- Para grandes volumes de dados
INSERT INTO large_table (col1, col2, col3) VALUES
(1, 'data1', 'value1'),
(2, 'data2', 'value2'),
-- ... até 1000 linhas por vez
(1000, 'data1000', 'value1000');

-- Script para inserir milhões de registos
DELIMITER //
CREATE PROCEDURE InsertBatchData()
BEGIN
    DECLARE i INT DEFAULT 1;
    WHILE i <= 1000000 DO
        INSERT INTO test_table (name, value) 
        VALUES (CONCAT('Name', i), RAND() * 100);
        
        IF i % 10000 = 0 THEN
            COMMIT;  -- Commit a cada 10k registos
        END IF;
        
        SET i = i + 1;
    END WHILE;
END //
DELIMITER ;
```

#### **UPDATE em Lotes:**

sqlresponse-action-icon

```sql
-- Atualizar em lotes para evitar locks longos
UPDATE products 
SET updated_at = NOW() 
WHERE id BETWEEN 1 AND 10000;

UPDATE products 
SET updated_at = NOW() 
WHERE id BETWEEN 10001 AND 20000;

-- Script automático para lotes
SET @batch_size = 10000;
SET @max_id = (SELECT MAX(id) FROM products);
SET @current_id = 1;

WHILE @current_id <= @max_id DO
    UPDATE products 
    SET price = price * 1.05 
    WHERE id BETWEEN @current_id AND @current_id + @batch_size - 1;
    
    SET @current_id = @current_id + @batch_size;
    
    -- Pequena pausa para não sobrecarregar
    SELECT SLEEP(0.1);
END WHILE;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Operações Básicas:**

sqlresponse-action-icon

```sql
-- 1. Inserir novos clientes
INSERT INTO customers (first_name, last_name, email, phone)
VALUES 
    ('Ana', 'Ferreira', 'ana.ferreira@email.com', '912345678'),
    ('Carlos', 'Mendes', 'carlos.mendes@email.com', '923456789'),
    ('Sofia', 'Rodrigues', 'sofia.rodrigues@email.com', '934567890');

-- 2. Atualizar preços com aumento de 8%
UPDATE products 
SET price = ROUND(price * 1.08, 2)
WHERE category = 'Electronics';

-- 3. Remover produtos descontinuados sem stock
DELETE FROM products 
WHERE status = 'discontinued' AND stock = 0;

-- 4. Inserir dados de vendas do último mês
INSERT INTO monthly_reports (month, total_sales, total_revenue)
SELECT 
    DATE_FORMAT(sale_date, '%Y-%m') as month,
    COUNT(*) as total_sales,
    SUM(quantity * price) as total_revenue
FROM sales
WHERE sale_date >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH)
GROUP BY DATE_FORMAT(sale_date, '%Y-%m');
```

#### **Exercício 2 - Operações Avançadas:**

sqlresponse-action-icon

```sql
-- 1. Sistema de pontos de fidelidade
UPDATE customers c
SET loyalty_points = loyalty_points + (
    SELECT COALESCE(SUM(o.total * 0.1), 0)
    FROM orders o
    WHERE o.customer_id = c.id 
    AND o.order_date >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH)
);

-- 2. Atualizar ranking de produtos
UPDATE products p
SET popularity_rank = (
    SELECT rank_num FROM (
        SELECT 
            product_id,
            RANK() OVER (ORDER BY total_sold DESC) as rank_num
        FROM (
            SELECT 
                product_id,
                SUM(quantity) as total_sold
            FROM order_items oi
            INNER JOIN orders o ON oi.order_id = o.id
            WHERE o.status = 'completed'
            GROUP BY product_id
        ) sales_summary
    ) ranked_products
    WHERE ranked_products.product_id = p.id
);

-- 3. Migrar dados antigos
INSERT INTO customers_archive
SELECT * FROM customers 
WHERE last_login_date < DATE_SUB(NOW(), INTERVAL 2 YEAR)
AND total_orders = 0;

DELETE FROM customers 
WHERE last_login_date < DATE_SUB(NOW(), INTERVAL 2 YEAR)
AND total_orders = 0;

-- 4. Normalizar dados inconsistentes
UPDATE customers 
SET email = LOWER(TRIM(email)),
    phone = REGEXP_REPLACE(phone, '[^0-9]', '')
WHERE email IS NOT NULL OR phone IS NOT NULL;
```

### **⚡ Performance e Otimização:**

#### **1. Índices para DML:**

sqlresponse-action-icon

```sql
-- Índices essenciais para WHERE clauses
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);
CREATE INDEX idx_products_category_status ON products(category, status);

-- Para UPDATEs frequentes
CREATE INDEX idx_products_stock ON products(stock);
CREATE INDEX idx_customers_status ON customers(status);
```

#### **2. Otimizar UPDATEs:**

sqlresponse-action-icon

```sql
-- ❌ Lento - subquery correlacionada
UPDATE products p
SET total_sold = (
    SELECT SUM(quantity) 
    FROM order_items oi 
    WHERE oi.product_id = p.id
);

-- ✅ Rápido - JOIN
UPDATE products p
INNER JOIN (
    SELECT product_id, SUM(quantity) as total_sold
    FROM order_items
    GROUP BY product_id
) totals ON p.id = totals.product_id
SET p.total_sold = totals.total_sold;
```

#### **3. Bulk Operations:**

sqlresponse-action-icon

```sql
-- Para grandes volumes, usar LOAD DATA (MySQL)
LOAD DATA INFILE '/path/to/data.csv'
INTO TABLE products
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
(name, price, category, stock);

-- Ou INSERT com múltiplos VALUES (melhor que loops)
INSERT INTO logs (user_id, action, timestamp) VALUES
(1, 'action1', NOW()),
(2, 'action2', NOW()),
-- ... muitos valores
(1000, 'action1000', NOW());
```

### **🚨 Segurança e Boas Práticas:**

#### **1. Sempre usar WHERE em UPDATE/DELETE:**

sqlresponse-action-icon

```sql
-- ❌ PERIGO - sem WHERE atualiza TUDO
UPDATE customers SET status = 'inactive';

-- ✅ Seguro - sempre com WHERE
UPDATE customers SET status = 'inactive' WHERE last_login < '2022-01-01';

-- Dica: Testar com SELECT primeiro
SELECT COUNT(*) FROM customers WHERE last_login < '2022-01-01';
-- Se o resultado for o esperado, então fazer UPDATE
```

#### **2. Usar Transações para Operações Críticas:**

sqlresponse-action-icon

```sql
START TRANSACTION;

-- Várias operações relacionadas
UPDATE account SET balance = balance - 100 WHERE id = 1;
UPDATE account SET balance = balance + 100 WHERE id = 2;
INSERT INTO transactions (from_account, to_account, amount) VALUES (1, 2, 100);

-- Verificar se tudo correu bem
-- Se sim: COMMIT;
-- Se não: ROLLBACK;

COMMIT;
```

#### **3. Validar Dados Antes de Inserir:**

sqlresponse-action-icon

```sql
-- INSERT com validação
INSERT INTO customers (first_name, last_name, email)
SELECT 'João', 'Silva', 'joao@email.com'
WHERE NOT EXISTS (
    SELECT 1 FROM customers WHERE email = 'joao@email.com'
);

-- UPDATE com validação
UPDATE products 
SET stock = stock - 5 
WHERE id = 1 AND stock >= 5;  -- Só atualiza se tiver stock suficiente
```
