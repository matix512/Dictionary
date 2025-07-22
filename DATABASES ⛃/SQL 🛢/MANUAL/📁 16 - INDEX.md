### **📊 O que são Índices?**

#### **Definição:**

> Índices são estruturas de dados que melhoram a velocidade de consulta numa tabela, criando um "atalho" para encontrar dados rapidamente. São como um índice num livro - em vez de procurar página por página, vais diretamente ao local correto.

#### **Como Funcionam:**

textresponse-action-icon

```text
📚 Sem Índice: SELECT * FROM users WHERE email = 'joao@email.com'
   → Procura linha por linha (Table Scan) - O(n)

🔍 Com Índice: SELECT * FROM users WHERE email = 'joao@email.com'  
   → Vai diretamente ao registo (Index Seek) - O(log n)
```

#### **Tipos de Índices:**

textresponse-action-icon

```text
🔑 Clustered - Ordena fisicamente os dados (1 por tabela)
📋 Non-Clustered - Ponteiros para dados (múltiplos por tabela)
✨ Unique - Garante valores únicos
📝 Composite - Múltiplas colunas
🔍 Partial - Apenas parte dos dados (com WHERE)
📊 Full-text - Pesquisa textual avançada
```

### **🎯 Quando Usar Índices:**

#### **✅ Usar Índices Quando:**

sqlresponse-action-icon

```sql
-- 1. Colunas frequentemente usadas em WHERE
SELECT * FROM orders WHERE customer_id = 123;
CREATE INDEX idx_orders_customer ON orders(customer_id);

-- 2. Colunas usadas em JOINs
SELECT * FROM orders o JOIN customers c ON o.customer_id = c.id;
CREATE INDEX idx_orders_customer ON orders(customer_id);

-- 3. Colunas usadas em ORDER BY
SELECT * FROM products ORDER BY price DESC;
CREATE INDEX idx_products_price ON products(price);

-- 4. Colunas usadas em GROUP BY
SELECT category, COUNT(*) FROM products GROUP BY category;
CREATE INDEX idx_products_category ON products(category);

-- 5. Colunas com alta seletividade (muitos valores únicos)
CREATE INDEX idx_customers_email ON customers(email);  -- Emails únicos
```

#### **❌ NÃO Usar Índices Quando:**

sqlresponse-action-icon

```sql
-- 1. Tabelas muito pequenas (< 1000 linhas)
-- O overhead do índice > benefício

-- 2. Colunas com poucos valores distintos
-- Exemplo: status com apenas 'active'/'inactive'
-- Baixa seletividade = pouco benefício

-- 3. Colunas frequentemente atualizadas
-- Cada UPDATE precisa atualizar o índice também

-- 4. Tabelas com muitos INSERTs
-- Cada INSERT precisa atualizar todos os índices
```

### **🔧 Criar e Gerenciar Índices:**

#### **Criar Índices:**

sqlresponse-action-icon

```sql
-- Índice simples
CREATE INDEX idx_customers_last_name ON customers(last_name);

-- Índice único
CREATE UNIQUE INDEX idx_customers_email ON customers(email);

-- Índice composto (múltiplas colunas)
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);

-- Índice com ordem específica
CREATE INDEX idx_products_price_desc ON products(price DESC);

-- Índice parcial (MySQL 8.0+/PostgreSQL)
CREATE INDEX idx_orders_pending ON orders(order_date) WHERE status = 'pending';

-- Índice covering (inclui colunas extras)
CREATE INDEX idx_orders_covering ON orders(customer_id, order_date) 
INCLUDE (total_amount, status);  -- SQL Server syntax
```

#### **Ver Índices Existentes:**

sqlresponse-action-icon

```sql
-- MySQL
SHOW INDEXES FROM orders;

-- Informações detalhadas
SELECT 
    TABLE_NAME,
    INDEX_NAME,
    COLUMN_NAME,
    SEQ_IN_INDEX,
    NON_UNIQUE,
    INDEX_TYPE
FROM INFORMATION_SCHEMA.STATISTICS 
WHERE TABLE_SCHEMA = 'your_database'
```