### **🎯 O que são Subqueries?**

#### **Definição:**

> Uma subquery (subconsulta) é uma query SQL dentro de outra query SQL. A subquery é executada primeiro e seu resultado é usado pela query principal.

#### **Tipos de Subqueries:**

```text
📝 Scalar: Retorna um único valor
📊 Table: Retorna uma tabela de resultados  
🔍 Column: Retorna uma coluna de valores
✅ Correlated: Referencia a query externa
❌ Non-correlated: Independente da query externa
```

#### **Onde Usar Subqueries:**

```sql
SELECT ... (scalar subquery)
FROM ... (table subquery)
WHERE ... (most common)
HAVING ... (with aggregations)
```

### **🔢 Scalar Subqueries:**

#### **Retornando Um Valor:**

```sql
-- Preço médio na base de dados
SELECT 
    product,
    price,  
(SELECT AVG(price) FROM sales) AS average_price,  
price - (SELECT AVG(price) FROM sales) AS difference_from_avg  
FROM sales;

-- Produto mais caro  
SELECT product, price  
FROM sales  
WHERE price = (SELECT MAX(price) FROM sales);

-- Vendas acima da média  
SELECT COUNT(*) AS above_average_sales  
FROM sales  
WHERE quantity * price > (SELECT AVG(quantity * price) FROM sales);
```


#### **Subqueries em SELECT:**

```sql
-- Múltiplas subqueries escalares
SELECT 
    salesperson,
    COUNT(*) AS sales_count,
    SUM(quantity * price) AS total_revenue,
    (SELECT AVG(quantity * price) FROM sales) AS overall_avg,
    (SELECT MAX(quantity * price) FROM sales) AS highest_sale,
    (SELECT COUNT(DISTINCT salesperson) FROM sales) AS total_salespeople
FROM sales
GROUP BY salesperson;

-- Percentuais com subqueries
SELECT 
    category,
    SUM(quantity * price) AS category_revenue,
    ROUND(
        SUM(quantity * price) * 100.0 / 
        (SELECT SUM(quantity * price) FROM sales), 
        2
    ) AS percentage_of_total
FROM sales
GROUP BY category;
````

### **📊 Subqueries em WHERE:**

#### **Comparações Simples:**

```sql
-- Produtos com preço acima da média
SELECT product, price
FROM sales
WHERE price > (SELECT AVG(price) FROM sales);

-- Vendas do vendedor mais produtivo
SELECT *
FROM sales
WHERE salesperson = (
    SELECT salesperson 
    FROM sales 
    GROUP BY salesperson 
    ORDER BY SUM(quantity * price) DESC 
    LIMIT 1
);

-- Vendas da data com maior volume
SELECT *
FROM sales
WHERE sale_date = (
    SELECT sale_date 
    FROM sales 
    GROUP BY sale_date 
    ORDER BY COUNT(*) DESC 
    LIMIT 1
);
```

#### **IN e NOT IN:**

```sql
-- Produtos vendidos por João
SELECT DISTINCT product
FROM sales
WHERE salesperson = 'João';

-- Outros vendedores que venderam os mesmos produtos
SELECT DISTINCT salesperson
FROM sales
WHERE product IN (
    SELECT DISTINCT product 
    FROM sales 
    WHERE salesperson = 'João'
)
AND salesperson != 'João';

-- Produtos nunca vendidos por Maria
SELECT DISTINCT product
FROM sales
WHERE product NOT IN (
    SELECT product 
    FROM sales 
    WHERE salesperson = 'Maria'
    AND product IS NOT NULL  -- Importante com NOT IN!
);
```

#### **EXISTS e NOT EXISTS:**

```sql
-- Vendedores que venderam eletrônicos
SELECT DISTINCT s1.salesperson
FROM sales s1
WHERE EXISTS (
    SELECT 1 
    FROM sales s2 
    WHERE s2.salesperson = s1.salesperson 
    AND s2.category = 'Electronics'
);

-- Categorias sem vendas hoje
SELECT DISTINCT category
FROM sales s1
WHERE NOT EXISTS (
    SELECT 1 
    FROM sales s2 
    WHERE s2.category = s1.category 
    AND s2.sale_date = CURDATE()
);

-- Produtos vendidos por todos os vendedores
SELECT DISTINCT product
FROM sales s1
WHERE NOT EXISTS (
    SELECT DISTINCT salesperson 
    FROM sales s2
    WHERE NOT EXISTS (
        SELECT 1 
        FROM sales s3 
        WHERE s3.product = s1.product 
        AND s3.salesperson = s2.salesperson
    )
);
```

### **📋 Subqueries como Tabelas (FROM):**

#### **Derived Tables:**

```sql
-- Top 3 vendedores como tabela
SELECT 
    top_sellers.salesperson,
    top_sellers.revenue,
    sales.product
FROM (
    SELECT 
        salesperson, 
        SUM(quantity * price) AS revenue
    FROM sales
    GROUP BY salesperson
    ORDER BY revenue DESC
    LIMIT 3
) AS top_sellers
JOIN sales ON top_sellers.salesperson = sales.salesperson;

-- Estatísticas mensais
SELECT 
    monthly_stats.*,
    CASE 
        WHEN revenue > avg_monthly_revenue THEN 'Above Average'
        ELSE 'Below Average'
    END AS performance
FROM (
    SELECT 
        MONTH(sale_date) AS month,
        MONTHNAME(sale_date) AS month_name,
        COUNT(*) AS sales_count,
        SUM(quantity * price) AS revenue,
        AVG(SUM(quantity * price)) OVER() AS avg_monthly_revenue
    FROM sales
    GROUP BY MONTH(sale_date), MONTHNAME(sale_date)
) AS monthly_stats
ORDER BY month;
```

#### **Common Table Expressions (CTEs):**

sqlresponse-action-icon

```sql
-- MySQL 8.0+, SQL Server, PostgreSQL
WITH top_products AS (
    SELECT 
        product,
        SUM(quantity * price) AS revenue
    FROM sales
    GROUP BY product
    ORDER BY revenue DESC
    LIMIT 5
),
product_stats AS (
    SELECT 
        AVG(revenue) AS avg_product_revenue,
        MAX(revenue) AS max_product_revenue
    FROM top_products
)
SELECT 
    tp.product,
    tp.revenue,
    ps.avg_product_revenue,
    ROUND(tp.revenue - ps.avg_product_revenue, 2) AS diff_from_avg
FROM top_products tp
CROSS JOIN product_stats ps;
```

### **🔄 Correlated Subqueries:**

#### **Referenciando Query Externa:**

sqlresponse-action-icon

```sql
-- Vendas de cada vendedor acima de sua própria média
SELECT 
    salesperson,
    product,
    quantity * price AS sale_value
FROM sales s1
WHERE quantity * price > (
    SELECT AVG(quantity * price)
    FROM sales s2
    WHERE s2.salesperson = s1.salesperson  -- Correlação aqui!
);

-- Última venda de cada vendedor
SELECT 
    salesperson,
    product,
    sale_date
FROM sales s1
WHERE sale_date = (
    SELECT MAX(sale_date)
    FROM sales s2
    WHERE s2.salesperson = s1.salesperson
);

-- Vendas do produto mais vendido por categoria
SELECT 
    category,
    product,
    quantity
FROM sales s1
WHERE product = (
    SELECT product
    FROM sales s2
    WHERE s2.category = s1.category
    GROUP BY product
    ORDER BY SUM(quantity) DESC
    LIMIT 1
);
```

### **📊 Subqueries com Agregações:**

#### **HAVING com Subqueries:**

sqlresponse-action-icon

```sql
-- Vendedores com receita acima da média geral
SELECT 
    salesperson,
    SUM(quantity * price) AS revenue
FROM sales
GROUP BY salesperson
HAVING SUM(quantity * price) > (
    SELECT AVG(revenue_per_salesperson)
    FROM (
        SELECT SUM(quantity * price) AS revenue_per_salesperson
        FROM sales
        GROUP BY salesperson
    ) AS salesperson_revenues
);

-- Categorias que representam mais de 20% das vendas
SELECT 
    category,
    SUM(quantity * price) AS revenue,
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS percentage
FROM sales
GROUP BY category
HAVING SUM(quantity * price) > (SELECT SUM(quantity * price) FROM sales) * 0.2;
```

### **🎯 Casos Práticos Avançados:**

#### **Ranking e Top N:**

sqlresponse-action-icon

```sql
-- Top 3 produtos por categoria
SELECT 
    category,
    product,
    total_revenue
FROM (
    SELECT 
        category,
        product,
        SUM(quantity * price) AS total_revenue,
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY SUM(quantity * price) DESC) as rank_in_category
    FROM sales
    GROUP BY category, product
) ranked_products
WHERE rank_in_category <= 3;

-- Produtos que estão no top 20% de receita
SELECT 
    product,
    SUM(quantity * price) AS revenue
FROM sales
GROUP BY product
HAVING SUM(quantity * price) >= (
    SELECT PERCENTILE_CONT(0.8) WITHIN GROUP (ORDER BY product_revenue)
    FROM (
        SELECT SUM(quantity * price) AS product_revenue
        FROM sales
        GROUP BY product
    ) AS product_revenues
);
```

#### **Análise Temporal:**

sqlresponse-action-icon

```sql
-- Crescimento mês a mês
SELECT 
    YEAR(sale_date) AS year,
    MONTH(sale_date) AS month,
    SUM(quantity * price) AS current_month_revenue,
    (
        SELECT SUM(quantity * price)
        FROM sales s2
        WHERE YEAR(s2.sale_date) = YEAR(s1.sale_date)
        AND MONTH(s2.sale_date) = MONTH(s1.sale_date) - 1
    ) AS previous_month_revenue,
    ROUND(
        (SUM(quantity * price) - (
            SELECT COALESCE(SUM(quantity * price), 0)
            FROM sales s2
            WHERE YEAR(s2.sale_date) = YEAR(s1.sale_date)
            AND MONTH(s2.sale_date) = MONTH(s1.sale_date) - 1
        )) * 100.0 / NULLIF((
            SELECT SUM(quantity * price)
            FROM sales s2
            WHERE YEAR(s2.sale_date) = YEAR(s1.sale_date)
            AND MONTH(s2.sale_date) = MONTH(s1.sale_date) - 1
        ), 0),
        2
    ) AS growth_percentage
FROM sales s1
GROUP BY YEAR(sale_date), MONTH(sale_date)
ORDER BY year, month;

-- Clientes que não compraram nos últimos 30 dias
SELECT DISTINCT customer_id
FROM sales
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id
    FROM sales
    WHERE sale_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
    AND customer_id IS NOT NULL
);
```

### **🔍 Subqueries vs JOINs:**

#### **Quando Usar Cada Um:**

sqlresponse-action-icon

```sql
-- ✅ Subquery é melhor - apenas verificar existência
SELECT product
FROM sales
WHERE EXISTS (
    SELECT 1 FROM categories c WHERE c.name = 'Electronics'
);

-- ✅ JOIN é melhor - precisar de dados de ambas tabelas
SELECT s.product, c.description
FROM sales s
INNER JOIN categories c ON s.category = c.name
WHERE c.name = 'Electronics';

-- Performance comparison
-- ❌ Lento - subquery não correlacionada repetitiva
SELECT *
FROM sales s1
WHERE (SELECT COUNT(*) FROM sales s2 WHERE s2.salesperson = s1.salesperson) > 3;

-- ✅ Rápido - JOIN com agregação
SELECT s1.*
FROM sales s1
INNER JOIN (
    SELECT salesperson
    FROM sales
    GROUP BY salesperson
    HAVING COUNT(*) > 3
) active_sellers ON s1.salesperson = active_sellers.salesperson;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Básico:**

sqlresponse-action-icon

```sql
-- 1. Produtos mais caros que a média
SELECT product, price
FROM sales
WHERE price > (SELECT AVG(price) FROM sales);

-- 2. Vendedores que venderam o produto mais caro
SELECT DISTINCT salesperson
FROM sales
WHERE price = (SELECT MAX(price) FROM sales);

-- 3. Número de vendas acima da média por vendedor
SELECT 
    salesperson,
    COUNT(*) AS sales_above_average
FROM sales
WHERE quantity * price > (SELECT AVG(quantity * price) FROM sales)
GROUP BY salesperson;
```

#### **Exercício 2 - Intermediário:**

sqlresponse-action-icon

```sql
-- 1. Produtos vendidos por mais vendedores que a média
SELECT 
    product,
    COUNT(DISTINCT salesperson) AS sellers_count
FROM sales
GROUP BY product
HAVING COUNT(DISTINCT salesperson) > (
    SELECT AVG(sellers_per_product)
    FROM (
        SELECT COUNT(DISTINCT salesperson) AS sellers_per_product
        FROM sales
        GROUP BY product
    ) AS product_sellers
);

-- 2. Vendedores que nunca venderam eletrônicos
SELECT DISTINCT salesperson
FROM sales s1
WHERE NOT EXISTS (
    SELECT 1
    FROM sales s2
    WHERE s2.salesperson = s1.salesperson
    AND s2.category = 'Electronics'
);

-- 3. Receita de cada vendedor vs receita total
SELECT 
    salesperson,
    SUM(quantity * price) AS individual_revenue,
    (SELECT SUM(quantity * price) FROM sales) AS total_revenue,
    ROUND(SUM(quantity * price) * 100.0 / (SELECT SUM(quantity * price) FROM sales), 2) AS percentage_of_total
FROM sales
GROUP BY salesperson;
```

#### **Exercício 3 - Avançado:**

sqlresponse-action-icon

```sql
-- 1. Produtos que tiveram vendas em todos os meses do ano
SELECT DISTINCT product
FROM sales s1
WHERE NOT EXISTS (
    SELECT month_num
    FROM (
        SELECT 1 AS month_num UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 
        UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 
        UNION SELECT 9 UNION SELECT 10 UNION SELECT 11 UNION SELECT 12
    ) months
    WHERE NOT EXISTS (
        SELECT 1
        FROM sales s2
        WHERE s2.product = s1.product
        AND MONTH(s2.sale_date) = months.month_num
    )
);

-- 2. Vendedores que sempre vendem acima do preço médio do produto
SELECT DISTINCT salesperson
FROM sales s1
WHERE NOT EXISTS (
    SELECT 1
    FROM sales s2
    WHERE s2.salesperson = s1.salesperson
    AND s2.price < (
        SELECT AVG(price)
        FROM sales s3
        WHERE s3.product = s2.product
    )
);

-- 3. Segunda maior venda de cada vendedor
SELECT 
    salesperson,
    quantity * price AS second_highest_sale
FROM sales s1
WHERE (
    SELECT COUNT(DISTINCT quantity * price)
    FROM sales s2
    WHERE s2.salesperson = s1.salesperson
    AND s2.quantity * s2.price > s1.quantity * s1.price
) = 1;
```

### **⚡ Performance e Otimização:**

#### **1. Subqueries Eficientes:**

sqlresponse-action-icon

```sql
-- ❌ Lento - subquery correlacionada pesada
SELECT *
FROM sales s1
WHERE quantity * price > (
    SELECT AVG(quantity * price)
    FROM sales s2
    WHERE s2.category = s1.category
    AND s2.salesperson = s1.salesperson
);

-- ✅ Rápido - pré-calcular em CTE ou tabela temporária
WITH category_salesperson_avg AS (
    SELECT 
        category,
        salesperson,
        AVG(quantity * price) AS avg_sale_value
    FROM sales
    GROUP BY category, salesperson
)
SELECT s.*
FROM sales s
INNER JOIN category_salesperson_avg csa 
    ON s.category = csa.category 
    AND s.salesperson = csa.salesperson
WHERE s.quantity * s.price > csa.avg_sale_value;
```

#### **2. Evitar Subqueries em SELECT quando possível:**

sqlresponse-action-icon

```sql
-- ❌ Lento - múltiplas subqueries escalares
SELECT 
    salesperson,
    (SELECT COUNT(*) FROM sales s2 WHERE s2.salesperson = s1.salesperson) AS sales_count,
    (SELECT AVG(quantity * price) FROM sales s2 WHERE s2.salesperson = s1.salesperson) AS avg_sale
FROM sales s1
GROUP BY salesperson;

-- ✅ Rápido - usar agregação direta
SELECT 
    salesperson,
    COUNT(*) AS sales_count,
    AVG(quantity * price) AS avg_sale
FROM sales
GROUP BY salesperson;
```

### **🚨 Erros Comuns:**

#### **1. NULL com NOT IN:**

sqlresponse-action-icon

```sql
-- ❌ Pode retornar 0 linhas se houver NULL
SELECT product
FROM sales
WHERE salesperson NOT IN (
    SELECT salesperson FROM sales WHERE category = 'Electronics'
    -- Se qualquer salesperson for NULL, NOT IN retorna vazio!
);

-- ✅ Usar NOT EXISTS ou filtrar NULLs
SELECT product
FROM sales s1
WHERE NOT EXISTS (
    SELECT 1
    FROM sales s2
    WHERE s2.category = 'Electronics'
    AND s2.salesperson = s1.salesperson
);
```

#### **2. Subquery retornando múltiplos valores:**

sqlresponse-action-icon

```sql
-- ❌ Erro se subquery retorna mais de um valor
SELECT *
FROM sales
WHERE price = (SELECT price FROM sales WHERE category = 'Electronics');

-- ✅ Usar IN ou ANY/ALL
SELECT *
FROM sales
WHERE price IN (SELECT price FROM sales WHERE category = 'Electronics');
```