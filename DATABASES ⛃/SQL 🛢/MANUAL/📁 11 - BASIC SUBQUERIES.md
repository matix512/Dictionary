### **🎯 O que são Subqueries?**

#### **Definição:**

> Uma subquery (subconsulta) é uma query SQL dentro de outra query SQL. A subquery é executada primeiro e seu resultado é usado pela query principal.

#### **Tipos de Subqueries:**

textresponse-action-icon

```text
📝 Scalar: Retorna um único valor
📊 Table: Retorna uma tabela de resultados  
🔍 Column: Retorna uma coluna de valores
✅ Correlated: Referencia a query externa
❌ Non-correlated: Independente da query externa
```

#### **Onde Usar Subqueries:**

sqlresponse-action-icon

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

