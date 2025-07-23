### **📊 Conceito de Agregação:**

#### **O que são Funções de Agregação?**

> Funções que processam um conjunto de valores e retornam um único valor. São essenciais para relatórios, estatísticas e análises de dados.

#### **Categorias Principais:**

```text
📈 Numéricas: SUM, AVG, MIN, MAX, COUNT
📊 Estatísticas: STDDEV, VARIANCE
📝 String: GROUP_CONCAT, STRING_AGG
📅 Data: MIN/MAX com datas
🔢 Matemáticas: STDDEV, VAR_POP, VAR_SAMP
```

### **🔢 Funções Numéricas Básicas:**

#### **COUNT - Contar Registos:**

```sql
-- Contar todas as linhas (incluindo NULL)
SELECT COUNT(*) AS total_rows FROM sales;

-- Contar valores não-nulos
SELECT COUNT(customer_id) AS customers_with_sales FROM sales;

-- Contar valores únicos
SELECT COUNT(DISTINCT product) AS unique_products FROM sales;
SELECT COUNT(DISTINCT salesperson) AS unique_salespeople FROM sales;

-- Contar com condição
SELECT 
    COUNT(*) AS total_sales,
    COUNT(CASE WHEN quantity > 2 THEN 1 END) AS large_quantity_sales,
    COUNT(CASE WHEN price > 500 THEN 1 END) AS expensive_items
FROM sales;
```

#### **SUM - Somar Valores:**

```sql
-- Soma simples
SELECT SUM(quantity) AS total_items_sold FROM sales;
SELECT SUM(quantity * price) AS total_revenue FROM sales;

-- Soma condicional
SELECT 
    SUM(quantity * price) AS total_revenue,
    SUM(CASE WHEN category = 'Electronics' THEN quantity * price ELSE 0 END) AS electronics_revenue,
    SUM(CASE WHEN category = 'Furniture' THEN quantity * price ELSE 0 END) AS furniture_revenue
FROM sales;

-- Soma com filtros
SELECT 
    SUM(quantity * price) AS total_revenue,
    SUM(quantity * price * 0.1) AS estimated_commission
FROM sales
WHERE sale_date >= '2024-01-01';
```

#### **AVG - Calcular Média:**

```sql
-- Média simples
SELECT AVG(price) AS average_price FROM sales;
SELECT AVG(quantity) AS average_quantity FROM sales;

-- Média ponderada
SELECT 
    AVG(price) AS simple_average,
    SUM(price * quantity) / SUM(quantity) AS weighted_average_by_quantity
FROM sales;

-- Média com arredondamento
SELECT 
    ROUND(AVG(price), 2) AS avg_price_rounded,
    ROUND(AVG(quantity * price), 2) AS avg_sale_value
FROM sales;
```

#### **MIN/MAX - Valores Extremos:**

```sql
-- Valores mínimos e máximos
SELECT 
    MIN(price) AS cheapest_item,
    MAX(price) AS most_expensive_item,
    MAX(price) - MIN(price) AS price_range
FROM sales;

-- Com detalhes completos
SELECT * FROM sales WHERE price = (SELECT MIN(price) FROM sales);
SELECT * FROM sales WHERE price = (SELECT MAX(price) FROM sales);

-- Min/Max por grupo
SELECT 
    category,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    COUNT(*) AS item_count
FROM sales
GROUP BY category;
```

### **📈 Funções Estatísticas Avançadas:**

#### **STDDEV e VARIANCE:**

```sql
-- Desvio padrão e variância
SELECT 
    AVG(price) AS avg_price,
    STDDEV(price) AS price_std_dev,
    VARIANCE(price) AS price_variance,
    MIN(price) AS min_price,
    MAX(price) AS max_price
FROM sales;

-- Por categoria
SELECT 
    category,
    COUNT(*) AS items,
    AVG(price) AS avg_price,
    STDDEV(price) AS std_dev,
    CASE 
        WHEN STDDEV(price) < 50 THEN 'Low Variation'
        WHEN STDDEV(price) < 200 THEN 'Medium Variation'
        ELSE 'High Variation'
    END AS price_variation_level
FROM sales
GROUP BY category;
```

### **📝 Agregação de Strings:**

#### **GROUP_CONCAT (MySQL) / STRING_AGG (SQL Server/PostgreSQL):**

```sql
-- MySQL - GROUP_CONCAT
SELECT 
    category,
    COUNT(*) AS product_count,
    GROUP_CONCAT(product) AS products,
    GROUP_CONCAT(DISTINCT product ORDER BY product) AS unique_products
FROM sales
GROUP BY category;

-- Com separador personalizado
SELECT 
    salesperson,
    GROUP_CONCAT(product ORDER BY sale_date SEPARATOR ' | ') AS products_sold
FROM sales
GROUP BY salesperson;

-- SQL Server - STRING_AGG (SQL Server 2017+)
SELECT 
    category,
    STRING_AGG(product, ', ') AS products
FROM sales
GROUP BY category;
```

### **📅 Agregação com Datas:**

#### **Datas Extremas:**

```sql
-- Primeira e última venda
SELECT 
    MIN(sale_date) AS first_sale,
    MAX(sale_date) AS last_sale,
    DATEDIFF(MAX(sale_date), MIN(sale_date)) AS days_between
FROM sales;

-- Por vendedor
SELECT 
    salesperson,
    MIN(sale_date) AS first_sale,
    MAX(sale_date) AS last_sale,
    COUNT(*) AS total_sales,
    ROUND(COUNT(*) / (DATEDIFF(MAX(sale_date), MIN(sale_date)) + 1), 2) AS sales_per_day
FROM sales
GROUP BY salesperson
HAVING COUNT(*) > 1;  -- Só vendedores com múltiplas vendas
```

### **🎯 Funções de Agregação Condicionais:**

#### **COUNT com CASE:**

```sql
-- Contar por condições específicas
SELECT 
    COUNT(*) AS total_sales,
    COUNT(CASE WHEN price > 500 THEN 1 END) AS expensive_sales,
    COUNT(CASE WHEN quantity > 2 THEN 1 END) AS bulk_sales,
    COUNT(CASE WHEN category = 'Electronics' THEN 1 END) AS electronics_sales,
    
    -- Percentuais
    ROUND(COUNT(CASE WHEN price > 500 THEN 1 END) * 100.0 / COUNT(*), 2) AS expensive_percentage
FROM sales;
```

#### **SUM com CASE:**

```sql
-- Revenue por condições
SELECT 
    SUM(quantity * price) AS total_revenue,
    SUM(CASE WHEN salesperson = 'João' THEN quantity * price ELSE 0 END) AS joao_revenue,
    SUM(CASE WHEN salesperson = 'Maria' THEN quantity * price ELSE 0 END) AS maria_revenue,  
SUM(CASE WHEN MONTH(sale_date) = 1 THEN quantity * price ELSE 0 END) AS january_revenue,

```text
-- Percentuais de contribuição
ROUND(SUM(CASE WHEN salesperson = 'João' THEN quantity * price ELSE 0 END) * 100.0 / SUM(quantity * price), 2) AS joao_percentage FROM sales;
```



#### **AVG com CASE:**

```sql
-- Médias condicionais
SELECT 
    AVG(price) AS overall_avg_price,
    AVG(CASE WHEN category = 'Electronics' THEN price END) AS electronics_avg_price,
    AVG(CASE WHEN category = 'Furniture' THEN price END) AS furniture_avg_price,
    AVG(CASE WHEN quantity > 2 THEN price END) AS bulk_avg_price
FROM sales;
````

### **🔄 Window Functions (Agregações Avançadas):**

#### **Agregações com OVER:**

```sql
-- MySQL 8.0+, SQL Server, PostgreSQL
SELECT 
    id,
    product,
    price,
    quantity * price AS sale_value,
    
    -- Totais acumulados
    SUM(quantity * price) OVER (ORDER BY sale_date) AS running_total,
    
    -- Média móvel
    AVG(quantity * price) OVER (ORDER BY sale_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg_3,
    
    -- Ranking
    RANK() OVER (ORDER BY quantity * price DESC) AS sale_rank,
    
    -- Percentual do total
    ROUND(quantity * price * 100.0 / SUM(quantity * price) OVER(), 2) AS percentage_of_total
FROM sales
ORDER BY sale_date;
```

#### **Partições por Grupos:**

```sql
-- Agregações por partição
SELECT 
    product,
    category,
    price,
    quantity * price AS sale_value,
    
    -- Rank dentro da categoria
    RANK() OVER (PARTITION BY category ORDER BY quantity * price DESC) AS category_rank,
    
    -- Percentual dentro da categoria
    ROUND(quantity * price * 100.0 / SUM(quantity * price) OVER (PARTITION BY category), 2) AS category_percentage,
    
    -- Média da categoria
    ROUND(AVG(quantity * price) OVER (PARTITION BY category), 2) AS category_avg
FROM sales
ORDER BY category, sale_value DESC;
```

### **📊 Análises Estatísticas Complexas:**

#### **Quartis e Percentis:**

```sql
-- Análise de quartis (MySQL 8.0+)
SELECT 
    product,
    price,
    NTILE(4) OVER (ORDER BY price) AS quartile,
    PERCENT_RANK() OVER (ORDER BY price) AS percent_rank,
    CUME_DIST() OVER (ORDER BY price) AS cumulative_distribution
FROM sales
ORDER BY price;

-- Resumo estatístico por quartil
SELECT 
    quartile,
    COUNT(*) AS items_count,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    AVG(price) AS avg_price
FROM (
    SELECT 
        price,
        NTILE(4) OVER (ORDER BY price) AS quartile
    FROM sales
) quartile_data
GROUP BY quartile
ORDER BY quartile;
```

### **🎯 Casos de Uso Práticos:**

#### **Dashboard Executivo:**

```sql
-- Métricas principais do negócio
SELECT 
    -- Vendas
    COUNT(*) AS total_transactions,
    COUNT(DISTINCT customer_id) AS unique_customers,
    COUNT(DISTINCT product) AS products_sold,
    
    -- Receita
    SUM(quantity * price) AS total_revenue,
    AVG(quantity * price) AS avg_transaction_value,
    
    -- Tendências
    SUM(CASE WHEN sale_date >= DATE_SUB(CURDATE(), INTERVAL 7 DAY) THEN quantity * price ELSE 0 END) AS last_7_days_revenue,
    SUM(CASE WHEN sale_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY) THEN quantity * price ELSE 0 END) AS last_30_days_revenue,
    
    -- Produtos
    MAX(quantity * price) AS largest_sale,
    MIN(quantity * price) AS smallest_sale
FROM sales
WHERE sale_date >= '2024-01-01';
```

#### **Análise de Performance por Vendedor:**

```sql
-- Relatório detalhado de vendedores
SELECT 
    salesperson,
    
    -- Volume
    COUNT(*) AS total_sales,
    SUM(quantity) AS items_sold,
    COUNT(DISTINCT product) AS unique_products_sold,
    
    -- Receita
    SUM(quantity * price) AS total_revenue,
    AVG(quantity * price) AS avg_sale_value,
    MIN(quantity * price) AS smallest_sale,
    MAX(quantity * price) AS largest_sale,
    
    -- Consistência
    STDDEV(quantity * price) AS sale_consistency,
    
    -- Produtividade
    ROUND(SUM(quantity * price) / COUNT(*), 2) AS revenue_per_transaction,
    
    -- Período
    MIN(sale_date) AS first_sale_date,
    MAX(sale_date) AS last_sale_date,
    DATEDIFF(MAX(sale_date), MIN(sale_date)) + 1 AS active_days
FROM sales
GROUP BY salesperson
ORDER BY total_revenue DESC;
```

#### **Análise de Produtos:**

```sql
-- Performance de produtos
SELECT 
    product,
    category,
    
    -- Volume
    COUNT(*) AS times_sold,
    SUM(quantity) AS total_quantity_sold,
    
    -- Pricing
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    AVG(price) AS avg_price,
    STDDEV(price) AS price_variation,
    
    -- Revenue
    SUM(quantity * price) AS total_revenue,
    AVG(quantity * price) AS avg_sale_value,
    
    -- Market share
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS market_share_percent,
    
    -- Vendedores
    COUNT(DISTINCT salesperson) AS sold_by_salespeople,
    GROUP_CONCAT(DISTINCT salesperson ORDER BY salesperson) AS salespeople_list
FROM sales
GROUP BY product, category
HAVING COUNT(*) > 1  -- Só produtos vendidos mais de uma vez
ORDER BY total_revenue DESC;
```

### **📈 Agregações Temporais:**

#### **Análise Temporal Detalhada:**

```sql
-- Vendas por períodos
SELECT 
    -- Por dia da semana
    DAYNAME(sale_date) AS day_of_week,
    DAYOFWEEK(sale_date) AS day_number,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS daily_revenue,
    AVG(quantity * price) AS avg_daily_sale,
    
    -- Comparação com média geral
    ROUND((AVG(quantity * price) - (SELECT AVG(quantity * price) FROM sales)) / (SELECT AVG(quantity * price) FROM sales) * 100, 2) AS vs_overall_avg_percent
FROM sales
GROUP BY DAYNAME(sale_date), DAYOFWEEK(sale_date)
ORDER BY day_number;

-- Tendência mensal
SELECT 
    YEAR(sale_date) AS year,
    MONTH(sale_date) AS month,
    MONTHNAME(sale_date) AS month_name,
    
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS monthly_revenue,
    AVG(quantity * price) AS avg_sale_value,
    
    -- Crescimento mês anterior (simulado)
    LAG(SUM(quantity * price)) OVER (ORDER BY YEAR(sale_date), MONTH(sale_date)) AS previous_month_revenue,
    
    ROUND(
        (SUM(quantity * price) - LAG(SUM(quantity * price)) OVER (ORDER BY YEAR(sale_date), MONTH(sale_date))) * 100.0 / 
        LAG(SUM(quantity * price)) OVER (ORDER BY YEAR(sale_date), MONTH(sale_date)), 2
    ) AS growth_percent
FROM sales
GROUP BY YEAR(sale_date), MONTH(sale_date), MONTHNAME(sale_date)
ORDER BY year, month;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Básico:**

```sql
-- 1. Estatísticas gerais de preços
SELECT 
    COUNT(*) AS total_items,
    AVG(price) AS avg_price,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    STDDEV(price) AS price_std_dev
FROM sales;

-- 2. Total de itens vendidos por categoria
SELECT 
    category,
    SUM(quantity) AS total_items_sold,
    COUNT(*) AS number_of_sales
FROM sales
GROUP BY category;

-- 3. Vendedor mais produtivo
SELECT 
    salesperson,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY salesperson
ORDER BY total_revenue DESC
LIMIT 1;
```

#### **Exercício 2 - Intermediário:**

```sql
-- 1. Análise de variação de preços por produto
SELECT 
    product,
    COUNT(*) AS times_sold,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    AVG(price) AS avg_price,
    MAX(price) - MIN(price) AS price_range,
    CASE 
        WHEN MAX(price) = MIN(price) THEN 'Fixed Price'
        WHEN (MAX(price) - MIN(price)) / AVG(price) > 0.1 THEN 'Variable Price'
        ELSE 'Stable Price'
    END AS price_strategy
FROM sales
GROUP BY product
HAVING COUNT(*) > 1;

-- 2. Receita por mês com percentual do total
SELECT 
    MONTHNAME(sale_date) AS month,
    SUM(quantity * price) AS monthly_revenue,
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS percent_of_total
FROM sales
GROUP BY MONTH(sale_date), MONTHNAME(sale_date)
ORDER BY monthly_revenue DESC;
```

#### **Exercício 3 - Avançado:**

```sql
-- 1. Análise de performance relativa por vendedor
SELECT 
    salesperson,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS revenue,
    AVG(quantity * price) AS avg_sale,
    
    -- Comparação com média geral
    (SELECT AVG(quantity * price) FROM sales) AS overall_avg,
    ROUND(AVG(quantity * price) - (SELECT AVG(quantity * price) FROM sales), 2) AS diff_from_avg,
    
    -- Percentual acima/abaixo da média
    ROUND((AVG(quantity * price) - (SELECT AVG(quantity * price) FROM sales)) * 100.0 / (SELECT AVG(quantity * price) FROM sales), 2) AS percent_diff_from_avg,
    
    -- Classificação
    CASE 
        WHEN AVG(quantity * price) > (SELECT AVG(quantity * price) FROM sales) * 1.1 THEN 'Above Average'
        WHEN AVG(quantity * price) < (SELECT AVG(quantity * price) FROM sales) * 0.9 THEN 'Below Average'
        ELSE 'Average'
    END AS performance_category
FROM sales
GROUP BY salesperson
ORDER BY revenue DESC;

-- 2. Análise de concentração de vendas (Pareto)
SELECT 
    product,
    SUM(quantity * price) AS revenue,
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS percent_of_total,
    
    -- Percentual cumulativo
    ROUND(SUM(SUM(quantity * price)) OVER (ORDER BY SUM(quantity * price) DESC) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS cumulative_percent
FROM sales
GROUP BY product
ORDER BY revenue DESC;
```

### **⚡ Performance e Otimização:**

#### **1. Índices para Agregações:**

```sql
-- Índices que aceleram agregações
CREATE INDEX idx_sales_category ON sales(category);
CREATE INDEX idx_sales_date ON sales(sale_date);
CREATE INDEX idx_sales_salesperson ON sales(salesperson);

-- Índice covering para consultas específicas
CREATE INDEX idx_sales_covering ON sales(category, salesperson, sale_date, quantity, price);
```

#### **2. Evitar Subconsultas Desnecessárias:**

```sql
-- ❌ Lento - subquery repetida
SELECT 
    product,
    SUM(quantity * price) AS revenue,
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS percent
FROM sales
GROUP BY product;

-- ✅ Rápido - usar window function
SELECT 
    product,
    SUM(quantity * price) AS revenue,
    ROUND(SUM(quantity * price) * 100.0 / SUM(SUM(quantity * price)) OVER(), 2) AS percent
FROM sales
GROUP BY product;
```

### **🚨 Erros Comuns:**

#### **1. NULL em Agregações:**
```sql
-- COUNT(*) conta NULLs, COUNT(column) não conta
SELECT 
    COUNT(*) AS total_rows,
    COUNT(customer_id) AS non_null_customers,
    AVG(price) AS avg_price  -- AVG ignora NULLs automaticamente
FROM sales;

-- Para incluir NULLs como zero
SELECT AVG(COALESCE(price, 0)) AS avg_including_nulls_as_zero FROM sales;
```

#### **2. Divisão por Zero:**
```sql
-- ❌ Pode gerar erro
SELECT SUM(quantity * price) / COUNT(DISTINCT customer_id) FROM sales;

-- ✅ Proteger contra divisão por zero
SELECT 
    CASE 
        WHEN COUNT(DISTINCT customer_id) > 0 THEN SUM(quantity * price) / COUNT(DISTINCT customer_id)
        ELSE 0 
    END AS avg_revenue_per_customer
FROM sales;
```