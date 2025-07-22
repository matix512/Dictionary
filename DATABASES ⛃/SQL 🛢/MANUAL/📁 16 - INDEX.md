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
AND TABLE_NAME = 'orders'  
ORDER BY TABLE_NAME, INDEX_NAME, SEQ_IN_INDEX;

-- Ver tamanho dos índices  
SELECT  
TABLE_NAME,  
INDEX_NAME,  
ROUND(STAT_VALUE * @@innodb_page_size / 1024 / 1024, 2) AS 'Index Size (MB)'  
FROM INFORMATION_SCHEMA.INNODB_SYS_TABLESTATS;

-- SQL Server  
SELECT  
i.name AS IndexName,  
COL_NAME(ic.object_id, ic.column_id) AS ColumnName,  
ic.index_column_id,  
i.type_desc AS IndexType  
FROM sys.indexes i  
INNER JOIN sys.index_columns ic ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.object_id = OBJECT_ID('orders')  
ORDER BY i.name, ic.index_column_id;
```


#### **Remover Índices:**

```sql
-- Remover índice específico
DROP INDEX idx_customers_last_name ON customers;  -- MySQL
DROP INDEX idx_customers_last_name;               -- SQL Server

-- Ver índices não utilizados (MySQL)
SELECT 
    s.TABLE_SCHEMA,
    s.TABLE_NAME,
    s.INDEX_NAME,
    s.CARDINALITY
FROM INFORMATION_SCHEMA.STATISTICS s
LEFT JOIN INFORMATION_SCHEMA.INDEX_STATISTICS i 
    ON s.TABLE_SCHEMA = i.TABLE_SCHEMA 
    AND s.TABLE_NAME = i.TABLE_NAME 
    AND s.INDEX_NAME = i.INDEX_NAME
WHERE i.INDEX_NAME IS NULL
AND s.INDEX_NAME != 'PRIMARY';
```

### **📋 Índices Compostos (Multi-Column):**

#### **Ordem das Colunas é Crucial:**

```sql
-- Índice composto
CREATE INDEX idx_orders_customer_date_status ON orders(customer_id, order_date, status);

-- ✅ Queries que USAM o índice:
SELECT * FROM orders WHERE customer_id = 123;
SELECT * FROM orders WHERE customer_id = 123 AND order_date = '2024-01-15';
SELECT * FROM orders WHERE customer_id = 123 AND order_date = '2024-01-15' AND status = 'pending';
SELECT * FROM orders WHERE customer_id = 123 AND status = 'pending';  -- Parcial

-- ❌ Queries que NÃO usam o índice eficientemente:
SELECT * FROM orders WHERE order_date = '2024-01-15';  -- Não começa pela primeira coluna
SELECT * FROM orders WHERE status = 'pending';         -- Não começa pela primeira coluna
SELECT * FROM orders WHERE order_date = '2024-01-15' AND status = 'pending';  -- Pula a primeira
```

#### **Regra do "Left-Most Prefix":**

```sql
-- Índice: (A, B, C, D)
CREATE INDEX idx_example ON table(col_a, col_b, col_c, col_d);

-- ✅ Utiliza o índice:
WHERE col_a = ?
WHERE col_a = ? AND col_b = ?
WHERE col_a = ? AND col_b = ? AND col_c = ?
WHERE col_a = ? AND col_b = ? AND col_c = ? AND col_d = ?
WHERE col_a = ? AND col_c = ?  -- Usa parcialmente (só col_a)

-- ❌ NÃO utiliza o índice:
WHERE col_b = ?
WHERE col_c = ?
WHERE col_b = ? AND col_c = ?
```

#### **Estratégias para Ordem de Colunas:**

```sql
-- Regra geral: Mais seletivas primeiro, mais usadas primeiro
-- 1. Igualdade antes de ranges
CREATE INDEX idx_products_good ON products(category, status, price);  -- ✅

-- category = 'Electronics' AND status = 'active' AND price BETWEEN 100 AND 500

-- 2. Colunas mais seletivas primeiro
CREATE INDEX idx_users_email_status ON users(email, status);  -- ✅
-- email é mais seletivo que status

-- 3. Considerar queries mais frequentes
-- Se 90% das queries usam customer_id, coloque primeiro
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);
```

### **🔍 Analisar Performance dos Índices:**

#### **EXPLAIN - Ver Plano de Execução:**

```sql
-- MySQL
EXPLAIN SELECT * FROM orders WHERE customer_id = 123;

-- Resultado esperado:
-- +----+-------------+--------+------+-------------------+---------+---------+-------+------+-------+
-- | id | select_type | table  | type | possible_keys     | key     | key_len | ref   | rows | Extra |
-- +----+-------------+--------+------+-------------------+---------+---------+-------+------+-------+
-- |  1 | SIMPLE      | orders | ref  | idx_orders_customer| idx_... | 4       | const |    5 |       |
-- +----+-------------+--------+------+-------------------+---------+---------+-------+------+-------+

-- Tipos de acesso (melhor para pior):
-- const    - Chave primária/única com valor constante
-- eq_ref   - Uma linha por combinação de tabelas anteriores  
-- ref      - Não único, múltiplas linhas com valor constante
-- range    - BETWEEN, >, <, IN
-- index    - Full index scan
-- ALL      - Full table scan (EVITAR!)

-- EXPLAIN mais detalhado
EXPLAIN FORMAT=JSON SELECT * FROM orders WHERE customer_id = 123;
```

#### **Analisar Uso de Índices:**

```sql
-- MySQL - Ver estatísticas de uso
SELECT 
    object_schema,
    object_name,
    index_name,
    count_read,
    count_write,
    count_read/count_write as read_write_ratio
FROM performance_schema.table_io_waits_summary_by_index_usage 
WHERE object_schema = 'your_database'
ORDER BY count_read DESC;

-- Identificar índices não utilizados
SELECT 
    t.TABLE_SCHEMA,
    t.TABLE_NAME,
    s.INDEX_NAME,
    s.CARDINALITY
FROM INFORMATION_SCHEMA.TABLES t
INNER JOIN INFORMATION_SCHEMA.STATISTICS s ON t.TABLE_NAME = s.TABLE_NAME
LEFT JOIN performance_schema.table_io_waits_summary_by_index_usage p 
    ON s.TABLE_NAME = p.object_name AND s.INDEX_NAME = p.index_name
WHERE t.TABLE_SCHEMA = 'your_database' 
AND p.index_name IS NULL
AND s.INDEX_NAME != 'PRIMARY';
```

### **🎯 Tipos Específicos de Índices:**

#### **Índices Full-Text:**

```sql
-- MySQL Full-Text Search
CREATE TABLE articles (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(200),
    content TEXT,
    FULLTEXT(title, content)
);

-- Pesquisa full-text
SELECT * FROM articles 
WHERE MATCH(title, content) AGAINST('MySQL indexing' IN NATURAL LANGUAGE MODE);

-- Pesquisa booleana
SELECT * FROM articles 
WHERE MATCH(title, content) AGAINST('+MySQL -Oracle' IN BOOLEAN MODE);

-- Pesquisa com expansão de query
SELECT * FROM articles 
WHERE MATCH(title, content) AGAINST('database' WITH QUERY EXPANSION);
```

#### **Índices Funcionais:**


```sql
-- MySQL 8.0+ - Índices em expressões
CREATE INDEX idx_customers_email_domain ON customers((SUBSTRING_INDEX(email, '@', -1)));

-- Usar o índice
SELECT * FROM customers 
WHERE SUBSTRING_INDEX(email, '@', -1) = 'gmail.com';

-- Índice em campo JSON (MySQL 8.0+)
CREATE INDEX idx_user_preferences_theme ON users((JSON_EXTRACT(preferences, '$.theme')));

-- PostgreSQL - Índices funcionais
CREATE INDEX idx_customers_lower_email ON customers(LOWER(email));
CREATE INDEX idx_orders_year ON orders(EXTRACT(YEAR FROM order_date));
```

#### **Índices Parciais:**

```sql
-- PostgreSQL - Índice apenas para subset dos dados
CREATE INDEX idx_orders_pending ON orders(customer_id, order_date) 
WHERE status = 'pending';

-- MySQL 8.0+ - Similar com expressão
CREATE INDEX idx_active_products ON products(name) 
WHERE status = 'active';

-- Benefício: Índice menor, mais rápido para queries específicas
SELECT * FROM orders 
WHERE status = 'pending' AND customer_id = 123;
```

### **📊 Estratégias Avançadas:**

#### **Covering Indexes:**

```sql
-- Índice que inclui todas as colunas necessárias
CREATE INDEX idx_orders_covering ON orders(customer_id, order_date, status, total_amount);

-- Query que usa apenas o índice (não acessa a tabela)
SELECT customer_id, order_date, status, total_amount 
FROM orders 
WHERE customer_id = 123;

-- SQL Server - INCLUDE clause
CREATE INDEX idx_orders_covering ON orders(customer_id, order_date) 
INCLUDE (status, total_amount);
```

#### **Índices para Diferentes Padrões de Query:**

```sql
-- Para queries de range + igualdade
CREATE INDEX idx_orders_date_customer ON orders(order_date, customer_id);
-- Ótimo para: WHERE order_date BETWEEN '2024-01-01' AND '2024-01-31' AND customer_id = 123

-- Para ordenação
CREATE INDEX idx_products_price_name ON products(price DESC, name ASC);
-- Para: ORDER BY price DESC, name ASC

-- Para GROUP BY
CREATE INDEX idx_sales_date_product ON sales(sale_date, product_id);
-- Para: GROUP BY sale_date, product_id

-- Para queries com OR (criar índices separados)
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_customers_phone ON customers(phone);
-- Para: WHERE email = 'x@email.com' OR phone = '123456789'
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - E-commerce Indexes:**

sqlresponse-action-icon

```sql
-- Cenário: Sistema de e-commerce com consultas típicas

-- 1. Índices para busca de produtos
CREATE INDEX idx_products_category_status ON products(category_id, status);
CREATE INDEX idx_products_name_fulltext ON products(name, description);
CREATE INDEX idx_products_price_range ON products(price);

-- 2. Índices para pedidos
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date DESC);
CREATE INDEX idx_orders_status_date ON orders(status, order_date);
CREATE INDEX idx_order_items_product ON order_items(product_id);

-- 3. Índices para clientes
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_customers_status_created ON customers(status, created_at);

-- 4. Índice covering para relatório de vendas
CREATE INDEX idx_sales_report ON order_items(product_id, order_id) 
INCLUDE (quantity, unit_price);

-- Testar performance
EXPLAIN SELECT * FROM products 
WHERE category_id = 1 AND status = 'active' 
ORDER BY price;

EXPLAIN SELECT c.name, COUNT(o.id) as total_orders
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
WHERE c.status = 'active'
GROUP BY c.id, c.name;
```

#### **Exercício 2 - Otimização de Queries Lentas:**

sqlresponse-action-icon

```sql
-- Identificar queries lentas
-- 1. Ativar slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 1;  -- Queries > 1 segundo

-- 2. Query problemática identificada
SELECT p.name, c.name as category, AVG(oi.unit_price) as avg_price
FROM products p
JOIN categories c ON p.category_id = c.id
JOIN order_items oi ON p.id = oi.product_id
JOIN orders o ON oi.order_id = o.id
WHERE o.order_date >= '2024-01-01'
AND o.status = 'completed'
GROUP BY p.id, p.name, c.name
HAVING COUNT(oi.id) > 5
ORDER BY avg_price DESC;

-- 3. Analisar sem índices
EXPLAIN FORMAT=JSON [query acima];

-- 4. Criar índices apropriados
CREATE INDEX idx_orders_date_status ON orders(order_date, status);
CREATE INDEX idx_order_items_product_order ON order_items(product_id, order_id, unit_price);
CREATE INDEX idx_products_category ON products(category_id);

-- 5. Testar novamente
EXPLAIN FORMAT=JSON [query acima];

-- 6. Comparar tempos de execução
SELECT BENCHMARK(1000, [query]);
```

### **🚨 Armadilhas Comuns:**

#### **1. Muitos Índices:**

sqlresponse-action-icon

```sql
-- ❌ Problema: Tabela com 20+ índices
-- Cada INSERT/UPDATE/DELETE fica lento

-- ✅ Solução: Auditar e remover índices não utilizados
-- Script para encontrar índices duplicados/redundantes

-- Encontrar índices redundantes
SELECT 
    a.TABLE_NAME,
    a.INDEX_NAME as redundant_index,
    b.INDEX_NAME as covering_index,
    GROUP_CONCAT(a.COLUMN_NAME ORDER BY a.SEQ_IN_INDEX) as redundant_columns,
    GROUP_CONCAT(b.COLUMN_NAME ORDER BY b.SEQ_IN_INDEX) as covering_columns
FROM INFORMATION_SCHEMA.STATISTICS a
JOIN INFORMATION_SCHEMA.STATISTICS b ON a.TABLE_NAME = b.TABLE_NAME
WHERE a.INDEX_NAME != b.INDEX_NAME
AND a.TABLE_SCHEMA = 'your_database'
GROUP BY a.TABLE_NAME, a.INDEX_NAME, b.INDEX_NAME;
```

#### **2. Índices em Colunas de Baixa Seletividade:**

sqlresponse-action-icon

```sql
-- ❌ Má escolha: status com apenas 2-3 valores
CREATE INDEX idx_products_status ON products(status);

-- ✅ Melhor: Combinar com coluna seletiva
CREATE INDEX idx_products_status_created ON products(status, created_at);

-- Calcular seletividade
SELECT 
    COUNT(DISTINCT status) / COUNT(*) * 100 as selectivity_percent,
    COUNT(DISTINCT status) as unique_values,
    COUNT(*) as total_rows
FROM products;

-- Regra: Seletividade < 5% raramente vale a pena indexar sozinha
```

#### **3. Índices Não Utilizados por Funções:**

sqlresponse-action-icon

```sql
-- ❌ Índice não é usado
CREATE INDEX idx_customers_email ON customers(email);
SELECT * FROM customers WHERE UPPER(email) = 'JOAO@EMAIL.COM';

-- ✅ Soluções:
-- Opção 1: Índice funcional
CREATE INDEX idx_customers_email_upper ON customers((UPPER(email)));

-- Opção 2: Mudar query para não usar função
SELECT * FROM customers WHERE email = 'joao@email.com';  -- Se dados estão normalized

-- Opção 3: Stored column (MySQL 5.7+)
ALTER TABLE customers ADD COLUMN email_upper VARCHAR(200) AS (UPPER(email)) STORED;
CREATE INDEX idx_customers_email_upper ON customers(email_upper);
```

### **⚡ Performance Monitoring:**

#### **Scripts de Monitorização:**

sqlresponse-action-icon

```sql
-- Top 10 tabelas por tamanho de índices
SELECT 
    TABLE_NAME,
    ROUND(SUM(INDEX_LENGTH)/1024/1024, 2) AS 'Index Size MB',  
ROUND(SUM(DATA_LENGTH)/1024/1024, 2) AS 'Data Size MB',  
ROUND(SUM(INDEX_LENGTH)/SUM(DATA_LENGTH)*100, 2) AS 'Index/Data Ratio %'  
FROM INFORMATION_SCHEMA.TABLES  
WHERE TABLE_SCHEMA = 'your_database'  
GROUP BY TABLE_NAME  
ORDER BY SUM(INDEX_LENGTH) DESC  
LIMIT 10;

-- Índices que ocupam mais espaço  
SELECT  
s.TABLE_NAME,  
s.INDEX_NAME,  
s.CARDINALITY,  
ROUND(  
(SELECT SUM(stat_value * @@innodb_page_size)  
FROM INFORMATION_SCHEMA.INNODB_SYS_TABLESTATS  
WHERE name = CONCAT(s.TABLE_SCHEMA, '/', s.TABLE_NAME)  
) / 1024 / 1024, 2  
) AS 'Size MB'  
FROM INFORMATION_SCHEMA.STATISTICS s  
WHERE s.TABLE_SCHEMA = 'your_database'  
AND s.INDEX_NAME != 'PRIMARY'  
ORDER BY s.CARDINALITY DESC;

-- Performance de queries por índice usado  
SELECT  
object_name AS table_name,  
index_name,  
count_read,  
count_write,  
sum_timer_read / 1000000000 AS read_time_seconds,  
sum_timer_write / 1000000000 AS write_time_seconds  
FROM performance_schema.table_io_waits_summary_by_index_usage  
WHERE object_schema = 'your_database'  
ORDER BY count_read DESC;

textresponse-action-icon

````text

### **🛠️ Manutenção de Índices:**

#### **Análise e Otimização:**
```sql
-- MySQL - Analisar tabela para atualizar estatísticas
ANALYZE TABLE products;

-- Verificar fragmentação de índices
SELECT 
    TABLE_NAME,
    ENGINE,
    TABLE_ROWS,
    DATA_LENGTH,
    INDEX_LENGTH,
    DATA_FREE,
    ROUND(DATA_FREE / (DATA_LENGTH + INDEX_LENGTH) * 100, 2) AS fragmentation_percent
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = 'your_database'
AND DATA_FREE > 0
ORDER BY fragmentation_percent DESC;

-- Reorganizar tabela se fragmentação > 10%
OPTIMIZE TABLE products;

-- SQL Server - Verificar fragmentação
SELECT 
    a.index_id,
    name,
    avg_fragmentation_in_percent,
    fragment_count,
    avg_fragment_size_in_pages
FROM sys.dm_db_index_physical_stats(DB_ID('your_database'), NULL, NULL, NULL, 'LIMITED') a
JOIN sys.indexes b ON a.object_id = b.object_id AND a.index_id = b.index_id
WHERE avg_fragmentation_in_percent > 10;

-- Reorganizar ou reconstruir índices
-- Fragmentação 10-30%: REORGANIZE
-- Fragmentação >30%: REBUILD
ALTER INDEX idx_products_name ON products REORGANIZE;
ALTER INDEX idx_products_name ON products REBUILD;
````

#### **Estratégia de Manutenção:**

sqlresponse-action-icon

```sql
-- Script de manutenção semanal
-- 1. Atualizar estatísticas
ANALYZE TABLE customers, products, orders, order_items;

-- 2. Reorganizar índices fragmentados
-- (Executar durante horário de baixo movimento)

-- 3. Verificar crescimento de índices
SELECT 
    TABLE_NAME,
    ROUND(INDEX_LENGTH/1024/1024, 2) as index_size_mb,
    TABLE_ROWS
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = 'your_database'
AND INDEX_LENGTH > 100*1024*1024  -- Índices > 100MB
ORDER BY INDEX_LENGTH DESC;
```

### **📋 Checklist de Boas Práticas:**

#### **✅ Antes de Criar Índice:**

sqlresponse-action-icon

```sql
-- 1. Analisar padrões de query
SELECT * FROM slow_query_log WHERE query_time > 1;

-- 2. Verificar se não existe índice similar
SHOW INDEXES FROM table_name;

-- 3. Calcular seletividade da coluna
SELECT 
    COUNT(DISTINCT column_name) / COUNT(*) * 100 as selectivity
FROM table_name;

-- 4. Considerar impacto em INSERTs/UPDATEs
-- Tabelas com muitos writes = menos índices

-- 5. Testar em ambiente similar à produção
```

#### **✅ Monitorização Contínua:**

sqlresponse-action-icon

```sql
-- Weekly index health check
CREATE EVENT weekly_index_check
ON SCHEDULE EVERY 1 WEEK
DO
BEGIN
    -- Log índices não utilizados
    INSERT INTO index_monitoring
    SELECT NOW(), 'UNUSED_INDEX', TABLE_NAME, INDEX_NAME
    FROM INFORMATION_SCHEMA.STATISTICS s
    LEFT JOIN performance_schema.table_io_waits_summary_by_index_usage p
        ON s.TABLE_NAME = p.object_name AND s.INDEX_NAME = p.index_name
    WHERE s.TABLE_SCHEMA = 'your_database'
    AND p.index_name IS NULL
    AND s.INDEX_NAME != 'PRIMARY';
    
    -- Atualizar estatísticas de tabelas grandes
    ANALYZE TABLE products, customers, orders;
END;
```

### **🎯 Estratégias por Tipo de Aplicação:**

#### **OLTP (Online Transaction Processing):**

sqlresponse-action-icon

```sql
-- Características: Muitos INSERTs/UPDATEs, queries simples
-- Estratégia: Poucos índices, muito específicos

-- Índices essenciais apenas
CREATE INDEX idx_orders_customer ON orders(customer_id);  -- Para JOINs
CREATE INDEX idx_products_sku ON products(sku);           -- Para lookups
CREATE INDEX idx_customers_email ON customers(email);     -- Para login

-- Evitar índices compostos grandes
-- Priorizar performance de escrita
```

#### **OLAP (Online Analytical Processing):**

sqlresponse-action-icon

```sql
-- Características: Muitos SELECTs complexos, poucos writes
-- Estratégia: Muitos índices, otimizar para leitura

-- Índices compostos para relatórios
CREATE INDEX idx_sales_date_product_customer ON sales(sale_date, product_id, customer_id);
CREATE INDEX idx_orders_date_status_total ON orders(order_date, status, total_amount);

-- Índices covering para queries frequentes
CREATE INDEX idx_customer_stats ON customers(region, status) 
INCLUDE (total_orders, total_spent, last_order_date);
```

#### **Aplicações Híbridas:**

sqlresponse-action-icon

```sql
-- Balancear entre leitura e escrita
-- Índices críticos para funcionalidade core
-- Índices adicionais apenas se justificado por metrics

-- Usar índices parciais para reduzir overhead
CREATE INDEX idx_active_products ON products(category_id, price) 
WHERE status = 'active';

-- Considerar índices em horários específicos
-- Criar índices para relatórios noturnos
-- Remover após processamento (se aplicável)
```

### **🚨 Troubleshooting de Performance:**

#### **Query Lenta - Processo de Diagnóstico:**

sqlresponse-action-icon

```sql
-- 1. Capturar query problemática
SELECT * FROM products p
JOIN categories c ON p.category_id = c.id
WHERE p.price BETWEEN 100 AND 500
AND p.status = 'active'
AND c.name = 'Electronics'
ORDER BY p.created_at DESC
LIMIT 20;

-- 2. Analisar plano de execução
EXPLAIN FORMAT=JSON [query above];

-- 3. Identificar table scans/full index scans
-- Procurar por: "type": "ALL" (table scan), "rows": número alto

-- 4. Criar índices estratégicos
-- Para WHERE: índice composto nas colunas filtradas
CREATE INDEX idx_products_status_price_created ON products(status, price, created_at);

-- Para JOIN: índice na foreign key
CREATE INDEX idx_products_category ON products(category_id);

-- 5. Testar novamente
EXPLAIN FORMAT=JSON [query above];

-- 6. Monitorar impacto em outras queries
-- Verificar se novos índices não prejudicaram outras operações
```


