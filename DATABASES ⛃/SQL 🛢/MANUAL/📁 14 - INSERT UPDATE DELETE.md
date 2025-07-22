### **🎯 Data Manipulation Language (DML):**

#### **O que é DML?**

> DML são comandos SQL usados para manipular dados dentro das tabelas. Incluem INSERT (criar), UPDATE (modificar), DELETE (remover) e SELECT (consultar).

#### **Comandos Principais:**

textresponse-action-icon

```text
➕ INSERT - Inserir novos registos
🔧 UPDATE - Modificar registos existentes
❌ DELETE - Remover registos
🔄 REPLACE - Substituir registos (MySQL)
🔀 MERGE/UPSERT - Inserir ou atualizar
```

### **➕ INSERT - Inserir Dados:**

#### **INSERT Básico:**

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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
    first_name = EXCLUDED.first_
```