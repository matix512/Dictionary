### **📊 Conceito de Agrupamento:**

#### **O que é GROUP BY?**

> GROUP BY agrupa linhas que têm os mesmos valores em colunas especificadas, permitindo aplicar funções de agregação (COUNT, SUM, AVG, etc.) a cada grupo.

#### **Fluxo de Execução SQL:**

```text
1. FROM - Selecionar tabelas
2. WHERE - Filtrar linhas individuais  
3. GROUP BY - Agrupar linhas
4. HAVING - Filtrar grupos
5. SELECT - Selecionar colunas
6. ORDER BY - Ordenar resultado
7. LIMIT - Limitar resultado
```

### **🎯 GROUP BY Básico:**

#### **Sintaxe:**

```sql
SELECT column1, aggregate_function(column2)
FROM table
WHERE condition
GROUP BY column1
HAVING group_condition
ORDER BY column1;
```

#### **Exemplos Fundamentais:**

```sql
-- Dataset expandido para exemplos
CREATE TABLE sales (
    id INT PRIMARY KEY,
    product VARCHAR(50),
    category VARCHAR(50),
    quantity INT,
    price DECIMAL(10,2),
    sale_date DATE,
    salesperson VARCHAR(50)
);

INSERT INTO sales VALUES 
(1, 'Laptop', 'Electronics', 2, 800.00, '2024-01-15', 'João'),
(2, 'Mouse', 'Electronics', 5, 25.00, '2024-01-16', 'Maria'),
(3, 'Chair', 'Furniture', 3, 150.00, '2024-01-17', 'João'),
(4, 'Laptop', 'Electronics', 1, 800.00, '2024-01-18', 'Ana'),
(5, 'Table', 'Furniture', 2, 300.00, '2024-01-19', 'Maria');

-- Contar vendas por vendedor
SELECT salesperson, COUNT(*) AS total_sales
FROM sales
GROUP BY salesperson;

-- Resultado:
-- +-------------+-------------+
-- | salesperson | total_sales |
-- +-------------+-------------+
-- | João        | 2           |
-- | Maria       | 2           |
-- | Ana         | 1           |
-- +-------------+-------------+
```

### **📈 Funções de Agregação:**

#### **COUNT - Contar:**

```sql
-- Contar todas as linhas por grupo
SELECT category, COUNT(*) AS total_items
FROM sales
GROUP BY category;

-- Contar valores não nulos
SELECT category, COUNT(price) AS items_with_price
FROM sales
GROUP BY category;

-- Contar valores únicos
SELECT category, COUNT(DISTINCT salesperson) AS unique_sellers
FROM sales
GROUP BY category;
```

#### **SUM - Somar:**
```sql
-- Total de vendas por categoria
SELECT 
    category,
    SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY category;

-- Total de quantidade e receita por vendedor
SELECT 
    salesperson,
    SUM(quantity) AS total_quantity,
    SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY salesperson;
```

#### **AVG - Média:**

```sql
-- Preço médio por categoria
SELECT 
    category,
    AVG(price) AS avg_price,
    ROUND(AVG(price), 2) AS avg_price_rounded
FROM sales
GROUP BY category;

-- Quantidade média por vendedor
SELECT 
    salesperson,
    AVG(quantity) AS avg_quantity,
    AVG(quantity * price) AS avg_sale_value
FROM sales
GROUP BY salesperson;
```

#### **MIN/MAX - Mínimo/Máximo:**

```sql
-- Faixa de preços por categoria
SELECT 
    category,
    MIN(price) AS cheapest,
    MAX(price) AS most_expensive,
    MAX(price) - MIN(price) AS price_range
FROM sales
GROUP BY category;

-- Primeira e última venda por vendedor
SELECT 
    salesperson,
    MIN(sale_date) AS first_sale,
    MAX(sale_date) AS last_sale,
    COUNT(*) AS total_sales
FROM sales
GROUP BY salesperson;
```

### **🔢 GROUP BY com Múltiplas Colunas:**

#### **Agrupamento Hierárquico:**

```sql
-- Vendas por categoria e vendedor
SELECT 
    category,
    salesperson,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS revenue
FROM sales
GROUP BY category, salesperson
ORDER BY category, revenue DESC;

-- Resultado:
-- +-------------+-------------+-------------+---------+
-- | category    | salesperson | sales_count | revenue |
-- +-------------+-------------+-------------+---------+
-- | Electronics | João        | 1           | 1600.00 |
-- | Electronics | Ana         | 1           | 800.00  |
-- | Electronics | Maria       | 1           | 125.00  |
-- | Furniture   | Maria       | 1           | 600.00  |
-- | Furniture   | João        | 1           | 450.00  |
-- +-------------+-------------+-------------+---------+
```

#### **Agrupamento por Expressões:**

```sql
-- Vendas por ano e mês
SELECT 
    YEAR(sale_date) AS year,
    MONTH(sale_date) AS month,
    MONTHNAME(sale_date) AS month_name,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS monthly_revenue
FROM sales
GROUP BY YEAR(sale_date), MONTH(sale_date), MONTHNAME(sale_date)
ORDER BY year, month;

-- Vendas por faixa de preço
SELECT 
    CASE 
        WHEN price < 100 THEN 'Cheap'
        WHEN price BETWEEN 100 AND 500 THEN 'Medium'
        ELSE 'Expensive'
    END AS price_range,
    COUNT(*) AS item_count,
    AVG(price) AS avg_price
    FROM sales  
GROUP BY  
CASE  
WHEN price < 100 THEN 'Cheap'  
WHEN price BETWEEN 100 AND 500 THEN 'Medium'  
ELSE 'Expensive'  
END  
ORDER BY avg_price;
```

### **🎯 HAVING - Filtrar Grupos:**

#### **HAVING vs WHERE:**

```sql
-- WHERE filtra ANTES do GROUP BY (linhas individuais)
-- HAVING filtra DEPOIS do GROUP BY (grupos)

-- ❌ Erro - não podes usar funções de agregação no WHERE
SELECT category, COUNT(*) 
FROM sales 
WHERE COUNT(*) > 1  -- ERRO!
GROUP BY category;

-- ✅ Correto - usar HAVING para filtrar grupos
SELECT category, COUNT(*) AS item_count
FROM sales 
GROUP BY category
HAVING COUNT(*) > 1;  -- Só categorias com mais de 1 item
```

#### **Exemplos HAVING:**

```sql
-- Vendedores com receita total > 1000
SELECT 
    salesperson,
    SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY salesperson
HAVING SUM(quantity * price) > 1000
ORDER BY total_revenue DESC;

-- Categorias com preço médio > 200
SELECT 
    category,
    AVG(price) AS avg_price,
    COUNT(*) AS item_count
FROM sales
GROUP BY category
HAVING AVG(price) > 200;

-- Combinando WHERE e HAVING
SELECT 
    category,
    COUNT(*) AS recent_sales,
    SUM(quantity * price) AS recent_revenue
FROM sales
WHERE sale_date >= '2024-01-17'  -- Filtrar ANTES de agrupar
GROUP BY category
HAVING COUNT(*) > 1;             -- Filtrar grupos DEPOIS
```

### **📅 Agrupamento por Tempo:**

#### **Por Períodos:**

sqlresponse-action-icon

```sql
-- Vendas por dia
SELECT 
    sale_date,
    COUNT(*) AS daily_sales,
    SUM(quantity * price) AS daily_revenue
FROM sales
GROUP BY sale_date
ORDER BY sale_date;

-- Vendas por dia da semana
SELECT 
    DAYNAME(sale_date) AS day_of_week,
    COUNT(*) AS sales_count,
    AVG(quantity * price) AS avg_sale_value
FROM sales
GROUP BY DAYNAME(sale_date), DAYOFWEEK(sale_date)
ORDER BY DAYOFWEEK(sale_date);

-- Vendas por trimestre
SELECT 
    YEAR(sale_date) AS year,
    QUARTER(sale_date) AS quarter,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS quarterly_revenue
FROM sales
GROUP BY YEAR(sale_date), QUARTER(sale_date)
ORDER BY year, quarter;
```

#### **Relatórios Temporais Avançados:**

sqlresponse-action-icon

```sql
-- Crescimento mês a mês (usando window functions se disponível)
SELECT 
    DATE_FORMAT(sale_date, '%Y-%m') AS month,
    SUM(quantity * price) AS monthly_revenue,
    LAG(SUM(quantity * price)) OVER (ORDER BY DATE_FORMAT(sale_date, '%Y-%m')) AS previous_month,
    ((SUM(quantity * price) - LAG(SUM(quantity * price)) OVER (ORDER BY DATE_FORMAT(sale_date, '%Y-%m'))) 
     / LAG(SUM(quantity * price)) OVER (ORDER BY DATE_FORMAT(sale_date, '%Y-%m')) * 100) AS growth_percent
FROM sales
GROUP BY DATE_FORMAT(sale_date, '%Y-%m')
ORDER BY month;
```

### **🔗 GROUP BY com JOINs:**

#### **Agrupamento Multi-Tabela:**

sqlresponse-action-icon

```sql
-- Assumindo tabelas relacionadas
CREATE TABLE customers (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    city VARCHAR(50),
    country VARCHAR(50)
);

ALTER TABLE sales ADD COLUMN customer_id INT;
ALTER TABLE sales ADD FOREIGN KEY (customer_id) REFERENCES customers(id);

-- Vendas por país do cliente
SELECT 
    c.country,
    COUNT(s.id) AS total_sales,
    SUM(s.quantity * s.price) AS total_revenue,
    AVG(s.quantity * s.price) AS avg_sale_value
FROM sales s
INNER JOIN customers c ON s.customer_id = c.id
GROUP BY c.country
ORDER BY total_revenue DESC;

-- Top 5 cidades por receita
SELECT 
    c.city,
    c.country,
    COUNT(s.id) AS sales_count,
    SUM(s.quantity * s.price) AS city_revenue
FROM customers c
INNER JOIN sales s ON c.id = s.customer_id
GROUP BY c.city, c.country
ORDER BY city_revenue DESC
LIMIT 5;
```

### **📊 Estatísticas Avançadas:**

#### **Múltiplas Agregações:**

sqlresponse-action-icon

```sql
-- Relatório completo por vendedor
SELECT 
    salesperson,
    COUNT(*) AS total_sales,
    COUNT(DISTINCT product) AS unique_products,
    SUM(quantity) AS total_items_sold,
    SUM(quantity * price) AS total_revenue,
    AVG(quantity * price) AS avg_sale_value,
    MIN(quantity * price) AS smallest_sale,
    MAX(quantity * price) AS largest_sale,
    STDDEV(quantity * price) AS sale_std_deviation
FROM sales
GROUP BY salesperson
ORDER BY total_revenue DESC;
```

#### **Percentuais e Ranking:**

sqlresponse-action-icon

```sql
-- Participação de cada categoria no total
SELECT 
    category,
    SUM(quantity * price) AS category_revenue,
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS percentage_of_total
FROM sales
GROUP BY category
ORDER BY category_revenue DESC;

-- Ranking de vendedores
SELECT 
    salesperson,
    SUM(quantity * price) AS revenue,
    RANK() OVER (ORDER BY SUM(quantity * price) DESC) AS rank_by_revenue
FROM sales
GROUP BY salesperson;
```

### **🎯 Casos de Uso Práticos:**

#### **Dashboard de Vendas:**

sqlresponse-action-icon

```sql
-- Métricas principais
SELECT 
    'Total Sales' AS metric,
    COUNT(*) AS value
FROM sales
UNION ALL
SELECT 
    'Total Revenue' AS metric,
    SUM(quantity * price) AS value
FROM sales
UNION ALL
SELECT 
    'Average Sale Value' AS metric,
    AVG(quantity * price) AS value
FROM sales
UNION ALL
SELECT 
    'Unique Products' AS metric,
    COUNT(DISTINCT product) AS value
FROM sales;
```

#### **Análise de Performance:**

sqlresponse-action-icon

```sql
-- Top produtos por receita e quantidade
SELECT 
    product,
    SUM(quantity) AS total_quantity,
    SUM(quantity * price) AS total_revenue,
    AVG(price) AS avg_price,
    COUNT(*) AS times_sold
FROM sales
GROUP BY product
HAVING COUNT(*) >= 2  -- Só produtos vendidos pelo menos 2 vezes
ORDER BY total_revenue DESC;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Básico:**

sqlresponse-action-icon

```sql
-- 1. Total de vendas por vendedor
SELECT salesperson, COUNT(*) AS sales_count
FROM sales
GROUP BY salesperson;

-- 2. Receita total por categoria
SELECT category, SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY category;

-- 3. Produto mais caro por categoria
SELECT category, MAX(price) AS max_price
FROM sales
GROUP BY category;
```

#### **Exercício 2 - Intermediário:**

sqlresponse-action-icon

```sql
-- 1. Vendedores com mais de 1 venda
SELECT salesperson, COUNT(*) AS sales_count
FROM sales
GROUP BY salesperson
HAVING COUNT(*) > 1;

-- 2. Receita média por produto, ordenada descendente
SELECT product, AVG(quantity * price) AS avg_revenue
FROM sales
GROUP BY product
ORDER BY avg_revenue DESC;

-- 3. Categorias com receita total > 500
SELECT category, SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY category
HAVING SUM(quantity * price) > 500;
```

#### **Exercício 3 - Avançado:**

sqlresponse-action-icon

```sql
-- 1. Relatório mensal completo
SELECT 
    MONTHNAME(sale_date) AS month,
    COUNT(*) AS sales_count,
    SUM(quantity) AS total_items,
    SUM(quantity * price) AS revenue,
    AVG(quantity * price) AS avg_sale
FROM sales
GROUP BY MONTH(sale_date), MONTHNAME(sale_date)
ORDER BY MONTH(sale_date);

-- 2. Performance por vendedor e categoria
SELECT 
    salesperson,
    category,
    COUNT(*) AS sales,
    SUM(quantity * price) AS revenue,
    ROUND(AVG(quantity * price), 2) AS avg_sale
FROM sales
GROUP BY salesperson, category
ORDER BY salesperson, revenue DESC;
```

### **⚡ Performance Tips:**

#### **1. Índices para GROUP BY:**

sqlresponse-action-icon

```sql
-- Indexar colunas do GROUP BY
CREATE INDEX idx_sales_category ON sales(category);
CREATE INDEX idx_sales_salesperson ON sales(salesperson);
CREATE INDEX idx_sales_date ON sales(sale_date);

-- Índice composto para múltiplas colunas
CREATE INDEX idx_sales_category_salesperson ON sales(category, salesperson);
```

#### **2. Ordem das Colunas no GROUP BY:**

sqlresponse-action-icon

```sql
-- Ordem deve corresponder ao índice para melhor performance
-- Se tem índice (category, salesperson)
SELECT category, salesperson, COUNT(*)
FROM sales
GROUP BY category, salesperson;  -- ✅ Usa o índice

SELECT salesperson, category, COUNT(*)
FROM sales  
GROUP BY salesperson, category;  -- ❌ Pode não usar o índice
```

#### **3. LIMIT com GROUP BY:**

sqlresponse-action-icon

```sql
-- Para top N, usar ORDER BY + LIMIT
SELECT category, SUM(quantity * price) AS revenue
FROM sales
GROUP BY category
ORDER BY revenue DESC
LIMIT 5;  -- Top 5 categorias
```

### **🚨 Erros Comuns:**

#### **1. Colunas não agregadas no SELECT:**

sqlresponse-action-icon

```sql
-- ❌ Erro - sale_date não está no GROUP BY nem agregada
SELECT salesperson, sale_date, COUNT(*)
FROM sales
GROUP BY salesperson;

-- ✅ Correto - todas as colunas estão no GROUP BY ou agregadas
SELECT salesperson, sale_date, COUNT(*)
FROM sales
GROUP BY salesperson, sale_date;

-- ✅ Ou agregar a coluna
SELECT salesperson, MAX(sale_date) AS last_sale, COUNT(*)
FROM sales
GROUP BY salesperson;
```

#### **2. Usar WHERE em vez de HAVING:**

sqlresponse-action-icon

```sql
-- ❌ Erro - função de agregação no WHERE
SELECT category, COUNT(*)
FROM sales
WHERE COUNT(*) > 1  -- ERRO!
GROUP BY category;

-- ✅ Correto
SELECT category, COUNT(*)
FROM sales
GROUP BY category
HAVING COUNT(*) > 1;
```