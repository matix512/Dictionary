### **🔧 O que é ALTER TABLE?**

#### **Propósito:**

> ALTER TABLE permite modificar a estrutura de tabelas existentes sem perder dados. É essencial para evolução de esquemas de base de dados.

#### **Operações Principais:**


```text
➕ ADD - Adicionar colunas, constraints, índices
🔧 MODIFY/ALTER - Modificar colunas existentes
❌ DROP - Remover colunas, constraints, índices
🔄 RENAME - Renomear tabela ou colunas
```

### **➕ Adicionar Colunas:**

#### **Sintaxe Básica:**

```sql
ALTER TABLE table_name
ADD COLUMN column_name datatype [constraints] [position];
```

#### **Exemplos Práticos:**

```sql
-- Adicionar coluna simples
ALTER TABLE products 
ADD COLUMN description TEXT;

-- Adicionar com posição específica (MySQL)
ALTER TABLE products 
ADD COLUMN brand VARCHAR(100) AFTER name;

-- Adicionar como primeira coluna
ALTER TABLE products 
ADD COLUMN priority INT FIRST;

-- Adicionar múltiplas colunas
ALTER TABLE products
ADD COLUMN weight DECIMAL(8,2),
ADD COLUMN dimensions VARCHAR(100),
ADD COLUMN color VARCHAR(50) DEFAULT 'Unknown';

-- Com constraints
ALTER TABLE products
ADD COLUMN supplier_id INT NOT NULL,
ADD CONSTRAINT fk_products_supplier 
    FOREIGN KEY (supplier_id) REFERENCES suppliers(id);
```

#### **Adicionar com Valores Padrão:**

```sql
-- Para tabela com dados existentes
ALTER TABLE customers
ADD COLUMN loyalty_points INT DEFAULT 0 NOT NULL;

-- Para ENUM com default
ALTER TABLE orders
ADD COLUMN status ENUM('pending', 'confirmed', 'shipped', 'delivered') 
    DEFAULT 'pending' NOT NULL;

-- Timestamp com auto-update
ALTER TABLE products
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP 
    ON UPDATE CURRENT_TIMESTAMP;
```

### **🔧 Modificar Colunas Existentes:**

#### **MODIFY vs ALTER COLUMN:**

```sql
-- MySQL - MODIFY COLUMN
ALTER TABLE products 
MODIFY COLUMN name VARCHAR(300) NOT NULL;

-- SQL Server/PostgreSQL - ALTER COLUMN
ALTER TABLE products 
ALTER COLUMN name VARCHAR(300) NOT NULL;

-- Mudar tipo de dados
ALTER TABLE products
MODIFY COLUMN price DECIMAL(12,2) NOT NULL;

-- Adicionar/remover NOT NULL
ALTER TABLE customers
MODIFY COLUMN phone VARCHAR(20) NULL;  -- Permitir NULL

ALTER TABLE customers
MODIFY COLUMN email VARCHAR(200) NOT NULL;  -- Forçar NOT NULL
```

#### **Modificações Complexas:**

```sql
-- Mudar ENUM values
ALTER TABLE orders
MODIFY COLUMN status ENUM('pending', 'confirmed', 'processing', 'shipped', 'delivered', 'cancelled') 
DEFAULT 'pending';

-- Mudar AUTO_INCREMENT
ALTER TABLE products
MODIFY COLUMN id INT AUTO_INCREMENT;

-- Reset AUTO_INCREMENT value
ALTER TABLE products AUTO_INCREMENT = 1000;

-- Mudar charset/collation
ALTER TABLE customers
MODIFY COLUMN name VARCHAR(200) 
CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### **❌ Remover Colunas:**

#### **DROP COLUMN:**

```sql
-- Remover coluna simples
ALTER TABLE products
DROP COLUMN old_column;

-- Remover múltiplas colunas
ALTER TABLE products
DROP COLUMN old_col1,
DROP COLUMN old_col2,
DROP COLUMN old_col3;

-- Cuidado com foreign keys - remover constraint primeiro
ALTER TABLE orders
DROP FOREIGN KEY fk_orders_customer;

ALTER TABLE orders
DROP COLUMN customer_id;
```

### **🔑 Gerenciar Constraints:**

#### **Adicionar Constraints:**

```sql
-- Primary Key
ALTER TABLE old_table
ADD PRIMARY KEY (id);

-- Foreign Key
ALTER TABLE orders
ADD CONSTRAINT fk_orders_customer 
FOREIGN KEY (customer_id) REFERENCES customers(id);

-- Unique
ALTER TABLE customers
ADD CONSTRAINT uk_customers_email UNIQUE (email);

-- Check constraint
ALTER TABLE products
ADD CONSTRAINT ck_products_price CHECK (price > 0);

-- Default value
ALTER TABLE customers
ALTER COLUMN status SET DEFAULT 'active';  -- MySQL
```

#### **Remover Constraints:**

```sql
-- Remover Foreign Key
ALTER TABLE orders
DROP FOREIGN KEY fk_orders_customer;

-- Remover Primary Key
ALTER TABLE old_table
DROP PRIMARY KEY;

-- Remover Unique
ALTER TABLE customers
DROP INDEX uk_customers_email;  -- MySQL

-- Remover Check (MySQL 8.0.16+)
ALTER TABLE products
DROP CHECK ck_products_price;

-- Remover Default
ALTER TABLE customers
ALTER COLUMN status DROP DEFAULT;
```

### **📊 Gerenciar Índices:**

#### **Adicionar Índices:**

```sql
-- Índice simples
ALTER TABLE products
ADD INDEX idx_products_name (name);

-- Índice composto
ALTER TABLE orders
ADD INDEX idx_orders_customer_date (customer_id, order_date);

-- Índice único
ALTER TABLE customers
ADD UNIQUE INDEX uk_customers_email (email);

-- Full-text index (MySQL)
ALTER TABLE products
ADD FULLTEXT(name, description);
```

#### **Remover Índices:**

```sql
-- Remover índice
ALTER TABLE products
DROP INDEX idx_products_name;

-- Ver índices existentes primeiro
SHOW INDEXES FROM products;

-- Remover múltiplos índices
ALTER TABLE products
DROP INDEX idx_name,
DROP INDEX idx_price;
```

### **🔄 Renomear:**

#### **Renomear Tabela:**

```sql
-- MySQL
ALTER TABLE old_table_name 
RENAME TO new_table_name;

-- Ou usando RENAME TABLE
RENAME TABLE old_table_name TO new_table_name;

-- Renomear múltiplas tabelas
RENAME TABLE 
    old_name1 TO new_name1,
    old_name2 TO new_name2;
```

#### **Renomear Colunas:**

```sql
-- MySQL 8.0+
ALTER TABLE customers
RENAME COLUMN old_column_name TO new_column_name;

-- MySQL versões anteriores
ALTER TABLE customers
CHANGE old_column_name new_column_name VARCHAR(100) NOT NULL;

-- SQL Server
EXEC sp_rename 'customers.old_column', 'new_column', 'COLUMN';
```

### **🎯 Casos Práticos Completos:**

#### **Evolução de Esquema E-commerce:**

```sql
-- Situação: Expandir sistema de produtos

-- 1. Adicionar categorias hierárquicas
ALTER TABLE categories
ADD COLUMN parent_id INT NULL,
ADD COLUMN level INT DEFAULT 0,
ADD FOREIGN KEY (parent_id) REFERENCES categories(id);

-- 2. Melhorar tabela de produtos
ALTER TABLE products
ADD COLUMN sku VARCHAR(50) UNIQUE NOT NULL,
ADD COLUMN weight_kg DECIMAL(8,3),
ADD COLUMN dimensions VARCHAR(100),
ADD COLUMN brand_id INT,
ADD COLUMN is_digital BOOLEAN DEFAULT FALSE,
ADD COLUMN seo_title VARCHAR(200),
ADD COLUMN seo_description TEXT,
ADD COLUMN tags TEXT;

-- 3. Criar tabela de marcas e relacionar
CREATE TABLE brands (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    logo_url VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Adicionar foreign key depois de popular dados
ALTER TABLE products
ADD FOREIGN KEY (brand_id) REFERENCES brands(id);

-- 4. Melhorar performance
ALTER TABLE products
ADD INDEX idx_products_sku (sku),
ADD INDEX idx_products_brand (brand_id),
ADD INDEX idx_products_digital (is_digital),
ADD FULLTEXT idx_products_search (name, description, tags);

-- 5. Migrar dados antigos (exemplo)
UPDATE products 
SET is_digital = TRUE 
WHERE category_id IN (SELECT id FROM categories WHERE name LIKE '%Digital%');
```

#### **Sistema de Auditoria:**

```sql
-- Adicionar campos de auditoria a tabelas existentes

-- Template para auditoria
ALTER TABLE customers
ADD COLUMN created_by INT DEFAULT 1,
ADD COLUMN updated_by INT DEFAULT 1,
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
ADD COLUMN is_deleted BOOLEAN DEFAULT FALSE,
ADD COLUMN deleted_at TIMESTAMP NULL;

-- Aplicar a múltiplas tabelas
SET @tables = 'products,orders,categories,suppliers';

-- Script para aplicar a todas (executar linha por linha)
ALTER TABLE products ADD COLUMN created_by INT DEFAULT 1, ADD COLUMN updated_by INT DEFAULT 1, ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;

ALTER TABLE orders ADD COLUMN created_by INT DEFAULT 1, ADD COLUMN updated_by INT DEFAULT 1, ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;
```

### **📋 Migrações de Dados Durante ALTER:**

#### **Migração Segura:**

```sql
-- 1. Backup primeiro!
CREATE TABLE customers_backup AS SELECT * FROM customers;

-- 2. Adicionar nova coluna com default temporário
ALTER TABLE customers
ADD COLUMN full_name VARCHAR(200) DEFAULT 'temp';

-- 3. Popular com dados existentes
UPDATE customers 
SET full_name = CONCAT(first_name, ' ', last_name)
WHERE first_name IS NOT NULL AND last_name IS NOT NULL;

-- 4. Fazer obrigatória depois de popular
ALTER TABLE customers
MODIFY COLUMN full_name VARCHAR(200) NOT NULL;

-- 5. Remover colunas antigas (se necessário)
-- ALTER TABLE customers DROP COLUMN first_name, DROP COLUMN last_name;
```

#### **Migração de Tipos de Dados:**

```sql
-- Situação: price era VARCHAR, precisa ser DECIMAL

-- 1. Adicionar nova coluna
ALTER TABLE products
ADD COLUMN price_new DECIMAL(10,2);

-- 2. Migrar dados válidos
UPDATE products 
SET price_new = CAST(price AS DECIMAL(10,2))
WHERE price REGEXP '^[0-9]+\.?[0-9]*$';

-- 3. Identificar dados problemáticos
SELECT id, price 
FROM products 
WHERE price_new IS NULL AND price IS NOT NULL;

-- 4. Corrigir dados problemáticos manualmente
UPDATE products SET price_new = 0.00 WHERE price_new IS NULL;

-- 5. Substituir coluna antiga
ALTER TABLE products DROP COLUMN price;
ALTER TABLE products CHANGE price_new price DECIMAL(10,2) NOT NULL;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Modificações Básicas:**

```sql
-- 1. Adicionar colunas de auditoria à tabela students
ALTER TABLE students
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
ADD COLUMN status ENUM('active', 'inactive', 'graduated') DEFAULT 'active';

-- 2. Modificar campo email para ser obrigatório
ALTER TABLE students
MODIFY COLUMN email VARCHAR(200) NOT NULL UNIQUE;

-- 3. Adicionar constraint de idade
ALTER TABLE students
ADD CONSTRAINT ck_students_age CHECK (age >= 16 AND age <= 100);

-- 4. Criar índice para pesquisa
ALTER TABLE students
ADD INDEX idx_students_status (status),
ADD INDEX idx_students_name (first_name, last_name);
```

#### **Exercício 2 - Reestruturação Complexa:**

```sql
-- Cenário: Melhorar sistema de biblioteca

-- 1. Adicionar campos para tracking de livros
ALTER TABLE books
ADD COLUMN barcode VARCHAR(50) UNIQUE,
ADD COLUMN purchase_date DATE,
ADD COLUMN purchase_price DECIMAL(8,2),
ADD COLUMN publisher VARCHAR(200),
ADD COLUMN edition VARCHAR(50),
ADD COLUMN language VARCHAR(50) DEFAULT 'Portuguese';

-- 2. Separar endereço dos membros
CREATE TABLE addresses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    street VARCHAR(200),
    city VARCHAR(100),
    postal_code VARCHAR(20),
    country VARCHAR(100) DEFAULT 'Portugal',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE members
ADD COLUMN address_id INT,
ADD FOREIGN KEY (address_id) REFERENCES addresses(id);

-- 3. Melhorar sistema de empréstimos
ALTER TABLE loans
ADD COLUMN renewal_count INT DEFAULT 0,
ADD COLUMN notes TEXT,
ADD CONSTRAINT ck_loans_renewal CHECK (renewal_count >= 0 AND renewal_count <= 3);

-- 4. Adicionar soft delete
ALTER TABLE books ADD COLUMN is_deleted BOOLEAN DEFAULT FALSE;
ALTER TABLE members ADD COLUMN is_deleted BOOLEAN DEFAULT FALSE;
ALTER TABLE loans ADD COLUMN is_deleted BOOLEAN DEFAULT FALSE;
```

### **⚡ Performance e Considerações:**

#### **1. ALTER TABLE em Tabelas Grandes:**

```sql
-- ⚠️ Operações que podem ser lentas em tabelas grandes:
-- - ADD/DROP COLUMN (pode recriar tabela)
-- - MODIFY COLUMN (especialmente mudança de tipo)
-- - ADD PRIMARY KEY/UNIQUE (precisa verificar toda a tabela)

-- Estratégias para tabelas grandes:

-- 1. Fazer em horários de menor movimento  
-- 2. Usar pt-online-schema-change (Percona) para MySQL  
-- 3. Fazer ALTER em partes (se possível)

-- Exemplo de ALTER seguro em produção:  
-- Adicionar coluna NOT NULL em tabela grande

-- Passo 1: Adicionar coluna NULL primeiro  
ALTER TABLE large_table ADD COLUMN new_col VARCHAR(100) NULL;

-- Passo 2: Popular dados em lotes  
UPDATE large_table SET new_col = 'default_value' WHERE id BETWEEN 1 AND 10000;  
UPDATE large_table SET new_col = 'default_value' WHERE id BETWEEN 10001 AND 20000;  
-- Continuar em lotes...

-- Passo 3: Tornar NOT NULL depois  
ALTER TABLE large_table MODIFY COLUMN new_col VARCHAR(100) NOT NULL;

textresponse-action-icon

````text

#### **2. Bloqueio de Tabelas:**
```sql
-- Operações que fazem LOCK da tabela:
-- - ALTER TABLE (na maioria dos casos)
-- - ADD/DROP INDEX em algumas versões

-- Para minimizar impacto:
-- 1. Usar ALGORITHM=INPLACE quando disponível (MySQL 5.6+)
ALTER TABLE products 
ADD INDEX idx_name (name) 
ALGORITHM=INPLACE, LOCK=NONE;

-- 2. Verificar se operação suporta online DDL
-- MySQL 5.7+ suporta muitas operações online
````

### **🚨 Erros Comuns e Troubleshooting:**

#### **1. Constraint Violations:**

```sql
-- ❌ Erro: Tentar adicionar NOT NULL com dados NULL existentes
ALTER TABLE customers MODIFY COLUMN phone VARCHAR(20) NOT NULL;
-- Error: Column 'phone' cannot be null

-- ✅ Solução: Corrigir dados primeiro
UPDATE customers SET phone = 'N/A' WHERE phone IS NULL;
ALTER TABLE customers MODIFY COLUMN phone VARCHAR(20) NOT NULL;

-- ❌ Erro: Adicionar UNIQUE com dados duplicados
ALTER TABLE products ADD UNIQUE (name);
-- Error: Duplicate entry

-- ✅ Solução: Identificar e corrigir duplicados
SELECT name, COUNT(*) 
FROM products 
GROUP BY name 
HAVING COUNT(*) > 1;
```

#### **2. Foreign Key Issues:**

```sql
-- ❌ Erro: Não conseguir dropar coluna com FK
ALTER TABLE orders DROP COLUMN customer_id;
-- Error: Cannot drop column 'customer_id': needed in a foreign key constraint

-- ✅ Solução: Dropar FK primeiro
ALTER TABLE orders DROP FOREIGN KEY fk_orders_customer;
ALTER TABLE orders DROP COLUMN customer_id;

-- ❌ Erro: Adicionar FK com dados órfãos
ALTER TABLE orders ADD FOREIGN KEY (customer_id) REFERENCES customers(id);
-- Error: Cannot add foreign key constraint

-- ✅ Solução: Identificar e corrigir dados órfãos
SELECT DISTINCT o.customer_id 
FROM orders o 
LEFT JOIN customers c ON o.customer_id = c.id 
WHERE c.id IS NULL;
```

#### **3. Tipo de Dados Incompatível:**

```sql
-- ❌ Erro: Converter VARCHAR para INT com dados inválidos
ALTER TABLE temp_table MODIFY COLUMN price_text INT;
-- Error: Incorrect integer value

-- ✅ Solução: Identificar dados problemáticos primeiro
SELECT * FROM temp_table WHERE price_text NOT REGEXP '^[0-9]+$';

-- Corrigir ou limpar dados
UPDATE temp_table SET price_text = '0' WHERE price_text NOT REGEXP '^[0-9]+$';
ALTER TABLE temp_table MODIFY COLUMN price_text INT;
```

### **📋 Checklist para ALTER TABLE:**

#### **Antes de Executar:**

```sql
-- ✅ 1. Backup da tabela
CREATE TABLE table_backup AS SELECT * FROM original_table;

-- ✅ 2. Verificar dados atuais
SELECT COUNT(*) FROM table_name;
SELECT * FROM table_name LIMIT 10;

-- ✅ 3. Identificar problemas potenciais
-- Para NOT NULL:
SELECT COUNT(*) FROM table_name WHERE column_name IS NULL;

-- Para UNIQUE:
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name HAVING COUNT(*) > 1;

-- Para FK:
SELECT COUNT(*) FROM child_table c LEFT JOIN parent_table p ON c.fk_col = p.id WHERE p.id IS NULL;

-- ✅ 4. Verificar espaço em disco (para operações grandes)
SHOW TABLE STATUS WHERE Name = 'table_name';
```

#### **Depois de Executar:**

```sql
-- ✅ 1. Verificar estrutura
DESCRIBE table_name;

-- ✅ 2. Verificar constraints
SHOW CREATE TABLE table_name;

-- ✅ 3. Verificar dados
SELECT COUNT(*) FROM table_name;  -- Deve ser igual ao anterior

-- ✅ 4. Testar aplicação
-- Executar queries típicas para verificar se tudo funciona

-- ✅ 5. Atualizar documentação
-- Documentar mudanças feitas
```

### **📚 Scripts Úteis para ALTER TABLE:**

#### **Script para Adicionar Auditoria:**

```sql
-- Procedure para adicionar campos de auditoria (MySQL)
DELIMITER //
CREATE PROCEDURE AddAuditFields(IN table_name VARCHAR(64))
BEGIN
    SET @sql = CONCAT(
        'ALTER TABLE ', table_name, ' ',
        'ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, ',
        'ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, ',
        'ADD COLUMN created_by INT DEFAULT 1, ',
        'ADD COLUMN updated_by INT DEFAULT 1'
    );
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //
DELIMITER ;

-- Usar:
CALL AddAuditFields('customers');
CALL AddAuditFields('products');
```

#### **Script para Verificar Integridade:**

```sql
-- Verificar todas as FKs de uma base de dados
SELECT 
    TABLE_NAME,
    COLUMN_NAME,
    CONSTRAINT_NAME,
    REFERENCED_TABLE_NAME,
    REFERENCED_COLUMN_NAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE REFERENCED_TABLE_SCHEMA = 'your_database'
ORDER BY TABLE_NAME;

-- Verificar tamanhos de tabelas
SELECT 
    table_name,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) AS 'Size (MB)'
FROM information_schema.tables 
WHERE table_schema = 'your_database'
ORDER BY (data_length + index_length) DESC;
```

