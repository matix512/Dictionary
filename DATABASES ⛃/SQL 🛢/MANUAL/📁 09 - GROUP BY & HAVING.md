### **📊 Conceito de Agrupamento:**

#### **O que é GROUP BY?**

> GROUP BY agrupa linhas que têm os mesmos valores em colunas especificadas, permitindo aplicar funções de agregação (COUNT, SUM, AVG, etc.) a cada grupo.

#### **Fluxo de Execução SQL:**

textresponse-action-icon

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

sqlresponse-action-icon

```sql
SELECT column1, aggregate_function(column2)
FROM table
WHERE condition
GROUP BY column1
HAVING group_condition
ORDER BY column1;
```

#### **Exemplos Fundamentais:**

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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

sqlresponse-action-icon

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
