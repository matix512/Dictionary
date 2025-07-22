### **📊 Conceito de Agregação:**

#### **O que são Funções de Agregação?**

> Funções que processam um conjunto de valores e retornam um único valor. São essenciais para relatórios, estatísticas e análises de dados.

#### **Categorias Principais:**

textresponse-action-icon

```text
📈 Numéricas: SUM, AVG, MIN, MAX, COUNT
📊 Estatísticas: STDDEV, VARIANCE
📝 String: GROUP_CONCAT, STRING_AGG
📅 Data: MIN/MAX com datas
🔢 Matemáticas: STDDEV, VAR_POP, VAR_SAMP
```

### **🔢 Funções Numéricas Básicas:**

#### **COUNT - Contar Registos:**

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

```sql
-- Revenue por condições
SELECT 
    SUM(quantity * price) AS total_revenue,
    SUM(CASE WHEN salesperson = 'João' THEN quantity * price ELSE 0 END) AS joao_revenue,
    SUM(CASE WHEN salesperson = 'Maria' THEN quantity * price ELSE 0 END) AS maria_revenue,  
SUM(CASE WHEN MONTH(sale_date) = 1 THEN quantity * price ELSE 0 END) AS january_revenue,

```text
-- Percentuais de contribuição
ROUND(SUM(CASE WHEN salesperson = 'João' THEN quantity * price ELSE 0 END) * 100.0 / SUM(quantity * price), 2) AS joao_percentage
```


