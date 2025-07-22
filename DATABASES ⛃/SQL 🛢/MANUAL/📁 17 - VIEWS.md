### **🔍 O que são Views?**

#### **Definição:**

> Uma View é uma "tabela virtual" baseada no resultado de uma query SQL. Não armazena dados fisicamente, mas apresenta dados de uma ou mais tabelas de forma organizada e simplificada.

#### **Características das Views:**

```text
📊 Virtual - Não ocupam espaço (só a definição)
🔄 Dinâmica - Dados sempre atualizados
🛡️ Segurança - Escondem colunas/linhas sensíveis
🎯 Simplificação - Queries complexas ficam simples
📋 Reutilização - Lógica de negócio centralizada
```

#### **Tipos de Views:**

```text
👁️ Simple View - Baseada numa tabela
🔗 Complex View - JOINs, agregações, subqueries
✏️ Updatable View - Permite INSERT/UPDATE/DELETE
👁️‍🗨️ Read-Only View - Apenas consulta
🏃 Materialized View - Cache físico (PostgreSQL/Oracle)
```

### **🆕 Criar Views:**

#### **Sintaxe Básica:**

```sql
CREATE VIEW view_name AS
SELECT columns
FROM tables
WHERE conditions;
```

#### **View Simples:**

```sql
-- View de produtos ativos
CREATE VIEW active_products AS
SELECT 
    id,
    name,
    price,
    stock_quantity,
    category_id
FROM products
WHERE status = 'active'
AND stock_quantity > 0;

-- Usar a view
SELECT * FROM active_products WHERE price < 100;
```

#### **View com Cálculos:**

```sql
-- View com campos calculados
CREATE VIEW product_summary AS
SELECT 
    id,
    name,
    price,
    stock_quantity,
    price * stock_quantity AS inventory_value,
    CASE 
        WHEN stock_quantity = 0 THEN 'Out of Stock'
        WHEN stock_quantity < 10 THEN 'Low Stock'
        ELSE 'In Stock'
    END AS stock_status,
    DATEDIFF(CURDATE(), created_at) AS days_since_created
FROM products;

-- Usar view
SELECT * FROM product_summary 
WHERE stock_status = 'Low Stock' 
ORDER BY inventory_value DESC;
```

### **🔗 Views com JOINs:**

#### **View com Relacionamentos:**

```sql
-- View completa de pedidos
CREATE VIEW order_details AS
SELECT 
    o.id AS order_id,
    o.order_number,
    o.order_date,
    o.status,
    o.total_amount,
    
    -- Customer info
    c.id AS customer_id,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    c.email AS customer_email,
    
    -- Address info
    o.shipping_address,
    
    -- Timing info
    DATEDIFF(o.shipped_date, o.order_date) AS processing_days,
    
    -- Status info
    CASE 
        WHEN o.status = 'delivered' THEN 'Complete'
        WHEN o.status = 'shipped' THEN 'In Transit'
        WHEN o.status = 'cancelled' THEN 'Cancelled'
        ELSE 'Processing'
    END AS order_status_description

FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;

-- Usar view com filtros
SELECT * FROM order_details 
WHERE order_date >= '2024-01-01'
AND order_status_description = 'Complete'
ORDER BY total_amount DESC;
```

#### **View com Múltiplas Tabelas:**

```sql
-- View completa do catálogo
CREATE VIEW product_catalog AS
SELECT 
    p.id,
    p.name AS product_name,
    p.sku,
    p.price,
    p.stock_quantity,
    
    -- Category info
    c.name AS category_name,
    c.parent_id AS parent_category_id,
    
    -- Supplier info
    s.name AS supplier_name,
    s.country AS supplier_country,
    
    -- Calculated fields
    ROUND(p.price * 1.23, 2) AS price_with_tax,
    p.price * p.stock_quantity AS inventory_value,
    
    -- Status
    CASE 
        WHEN p.stock_quantity = 0 THEN 'Out of Stock'
        WHEN p.stock_quantity < p.min_stock_level THEN 'Reorder Required'
        ELSE 'Available'
    END AS availability_status,
    
    -- Timestamps
    p.created_at,
    p.updated_at

FROM products p
LEFT JOIN categories c ON p.category_id = c.id
LEFT JOIN suppliers s ON p.supplier_id = s.id
WHERE p.status = 'active';
```

### **📊 Views com Agregações:**

#### **View de Estatísticas:**

```sql
-- View de estatísticas de clientes
CREATE VIEW customer_stats AS
SELECT 
    c.id,
    c.first_name,
    c.last_name,
    c.email,
    c.created_at AS customer_since,
    
    -- Order statistics
    COUNT(o.id) AS total_orders,
    COALESCE(SUM(o.total_amount), 0) AS total_spent,
    COALESCE(AVG(o.total_amount), 0) AS avg_order_value,
    MAX(o.order_date) AS last_order_date,
    
    -- Customer classification
    CASE 
        WHEN COUNT(o.id) = 0 THEN 'New Customer'
        WHEN COUNT(o.id) <= 2 THEN 'Occasional'
        WHEN COUNT(o.id) <= 10 THEN 'Regular'
        ELSE 'VIP'
    END AS customer_tier,
    
    -- Activity status
    CASE 
        WHEN MAX(o.order_date) IS NULL THEN 'Never Ordered'
        WHEN MAX(o.order_date) >= DATE_SUB(CURDATE(), INTERVAL 30 DAY) THEN 'Active'
        WHEN MAX(o.order_date) >= DATE_SUB(CURDATE(), INTERVAL 90 DAY) THEN 'Recent'
        ELSE 'Inactive'
    END AS activity_status

FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
GROUP BY c.id, c.first_name, c.last_name, c.email, c.created_at;

-- Usar para análise
SELECT 
    customer_tier,
    COUNT(*) AS customer_count,
    AVG(total_spent) AS avg_lifetime_value
FROM customer_stats
GROUP BY customer_tier
ORDER BY avg_lifetime_value DESC;
```

#### **View de Relatórios de Vendas:**

```sql
-- View de vendas mensais
CREATE VIEW monthly_sales_report AS  
SELECT  
YEAR(o.order_date) AS year,  
MONTH(o.order_date) AS month,  
MONTHNAME(o.order_date) AS month_name,  
DATE_FORMAT(o.order_date, '%Y-%m') AS year_month,

textresponse-action-icon

```text
-- Volume metrics
COUNT(DISTINCT o.id) AS total_orders,
COUNT(DISTINCT o.customer_id) AS unique_customers,
SUM(oi.quantity) AS total_items_sold,

-- Revenue metrics
SUM(o.total_amount) AS total_revenue,
AVG(o.total_amount) AS avg_order_value,
SUM(o.total_amount) / COUNT(DISTINCT o.customer_id) AS revenue_per_customer,

-- Product metrics
COUNT(DISTINCT oi.product_id) AS unique_products_sold,

-- Growth calculations (simplified)
LAG(SUM(o.total_amount)) OVER (ORDER BY YEAR(o.order_date), MONTH(o.order_date)) AS previous_month_revenue,

-- Status breakdown
SUM(CASE WHEN o.status = 'completed' THEN o.total_amount ELSE 0 END) AS completed_revenue,
SUM(CASE WHEN o.status = 'cancelled' THEN 1 ELSE 0 END) AS cancelled_orders

FROM orders o  
INNER JOIN order_items oi ON o.id = oi.order_id  
GROUP BY YEAR(o.order_date), MONTH(o.order_date), MONTHNAME(o.order_date)  
ORDER BY year DESC, month DESC;

-- View de produtos mais vendidos  
CREATE VIEW top_selling_products AS  
SELECT  
p.id,  
p.name,  
p.sku,  
p.price,  
c.name AS category,

-- Sales metrics
SUM(oi.quantity) AS total_quantity_sold,
COUNT(DISTINCT oi.order_id) AS times_ordered,
SUM(oi.quantity * oi.unit_price) AS total_revenue,
AVG(oi.unit_price) AS avg_selling_price,

-- Performance metrics
ROUND(SUM(oi.quantity * oi.unit_price) / SUM(oi.quantity), 2) AS revenue_per_unit,

-- Ranking
RANK() OVER (ORDER BY SUM(oi.quantity) DESC) AS quantity_rank,
RANK() OVER (ORDER BY SUM(oi.quantity * oi.unit_price) DESC) AS revenue_rank

FROM products p  
INNER JOIN order_items oi ON p.id = oi.product_id  
INNER JOIN orders o ON oi.order_id = o.id  
LEFT JOIN categories c ON p.category_id = c.id  
WHERE o.status = 'completed'  
GROUP BY p.id, p.name, p.sku, p.price, c.name  
HAVING total_quantity_sold > 0  
ORDER BY total_revenue DESC;

```

### **🛡️ Views para Segurança:**

#### **Controlar Acesso a Dados Sensíveis:**
```sql
-- View que esconde informações sensíveis
CREATE VIEW public_customer_info AS
SELECT 
    id,
    first_name,
    last_name,
    -- Email mascarado
    CONCAT(LEFT(email, 3), '***@', SUBSTRING_INDEX(email, '@', -1)) AS masked_email,
    -- Telefone mascarado
    CONCAT('***-***-', RIGHT(phone, 4)) AS masked_phone,
    city,
    country,
    created_at,
    -- Status público apenas
    CASE WHEN status = 'active' THEN 'Active' ELSE 'Inactive' END AS account_status
FROM customers
WHERE status != 'deleted';  -- Não mostrar contas deletadas

-- View para departamento financeiro
CREATE VIEW finance_customer_view AS
SELECT 
    c.id,
    CONCAT(c.first_name, ' ', c.last_name) AS full_name,
    c.email,
    
    -- Financial information
    c.credit_limit,
    c.total_spent,
    c.loyalty_points,
    
    -- Payment history
    COUNT(o.id) AS total_orders,
    SUM(CASE WHEN o.status = 'completed' THEN o.total_amount ELSE 0 END) AS paid_amount,
    SUM(CASE WHEN o.status = 'pending' THEN o.total_amount ELSE 0 END) AS pending_amount,
    
    -- Risk assessment
    CASE 
        WHEN c.total_spent > c.credit_limit * 0.8 THEN 'High Risk'
        WHEN c.total_spent > c.credit_limit * 0.5 THEN 'Medium Risk'
        ELSE 'Low Risk'
    END AS credit_risk

FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
WHERE c.status = 'active'
GROUP BY c.id, c.first_name, c.last_name, c.email, c.credit_limit, c.total_spent, c.loyalty_points;
````

#### **Views por Departamento:**

```sql
-- View para equipa de marketing
CREATE VIEW marketing_customer_view AS
SELECT 
    c.id,
    c.first_name,
    c.last_name,
    c.email,
    c.city,
    c.country,
    c.birth_date,
    
    -- Marketing metrics
    YEAR(CURDATE()) - YEAR(c.birth_date) AS age,
    cs.customer_tier,
    cs.total_orders,
    cs.total_spent,
    cs.last_order_date,
    cs.activity_status,
    
    -- Segmentation
    CASE 
        WHEN c.country = 'Portugal' THEN 'Domestic'
        ELSE 'International'
    END AS market_segment,
    
    -- Preferences (from order history)
    (SELECT GROUP_CONCAT(DISTINCT cat.name) 
     FROM orders o 
     JOIN order_items oi ON o.id = oi.order_id
     JOIN products p ON oi.product_id = p.id
     JOIN categories cat ON p.category_id = cat.id
     WHERE o.customer_id = c.id
     AND o.status = 'completed'
     LIMIT 5) AS preferred_categories

FROM customers c
INNER JOIN customer_stats cs ON c.id = cs.id
WHERE c.status = 'active';

-- View para equipa de suporte
CREATE VIEW support_customer_view AS
SELECT 
    c.id,
    c.first_name,
    c.last_name,
    c.email,
    c.phone,
    c.created_at,
    
    -- Account info
    c.status,
    cs.total_orders,
    cs.last_order_date,
    
    -- Recent activity
    (SELECT COUNT(*) 
     FROM orders o 
     WHERE o.customer_id = c.id 
     AND o.order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)) AS orders_last_30_days,
     
    -- Issues tracking
    (SELECT COUNT(*) 
     FROM support_tickets st 
     WHERE st.customer_id = c.id 
     AND st.status = 'open') AS open_tickets,
     
    -- Order status
    (SELECT GROUP_CONCAT(DISTINCT o.status)
     FROM orders o 
     WHERE o.customer_id = c.id 
     AND o.order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)) AS recent_order_statuses

FROM customers c
LEFT JOIN customer_stats cs ON c.id = cs.id;
```

### **🔄 Views Atualizáveis:**

#### **Quando Uma View é Atualizável:**

```sql
-- ✅ View atualizável (simples, uma tabela, sem agregações)
CREATE VIEW active_customers AS
SELECT id, first_name, last_name, email, phone
FROM customers
WHERE status = 'active';

-- Operações permitidas:
INSERT INTO active_customers (first_name, last_name, email) 
VALUES ('João', 'Silva', 'joao@email.com');

UPDATE active_customers 
SET phone = '912345678' 
WHERE id = 1;

DELETE FROM active_customers WHERE id = 1;
```

#### **Views Não Atualizáveis:**

```sql
-- ❌ View NÃO atualizável (tem JOINs, agregações)
CREATE VIEW customer_order_summary AS
SELECT 
    c.id,
    c.name,
    COUNT(o.id) AS order_count,  -- Agregação
    SUM(o.total) AS total_spent  -- Agregação
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id  -- JOIN
GROUP BY c.id, c.name;  -- GROUP BY

-- Estas operações falharão:
-- INSERT INTO customer_order_summary ...  -- ERRO
-- UPDATE customer_order_summary ...       -- ERRO
```

#### **WITH CHECK OPTION:**

```sql
-- Garantir que INSERTs/UPDATEs respeitam a condição da view
CREATE VIEW premium_customers AS
SELECT id, first_name, last_name, email, total_spent
FROM customers
WHERE total_spent > 1000
WITH CHECK OPTION;

-- ✅ Funcionará
INSERT INTO premium_customers (first_name, last_name, email, total_spent)
VALUES ('Maria', 'Santos', 'maria@email.com', 1500);

-- ❌ Falhará (total_spent <= 1000)
INSERT INTO premium_customers (first_name, last_name, email, total_spent)
VALUES ('Ana', 'Costa', 'ana@email.com', 500);  -- ERRO: CHECK OPTION failed
```

### **🎯 Views para Relatórios:**

#### **Dashboard Executivo:**

```sql
-- View para KPIs principais
CREATE VIEW executive_dashboard AS
SELECT 
    'Today' AS period,
    CURDATE() AS date,
    
    -- Sales metrics
    (SELECT COUNT(*) FROM orders WHERE DATE(order_date) = CURDATE()) AS orders_today,
    (SELECT COALESCE(SUM(total_amount), 0) FROM orders WHERE DATE(order_date) = CURDATE()) AS revenue_today,
    
    -- Customer metrics
    (SELECT COUNT(*) FROM customers WHERE DATE(created_at) = CURDATE()) AS new_customers_today,
    (SELECT COUNT(DISTINCT customer_id) FROM orders WHERE DATE(order_date) = CURDATE()) AS active_customers_today,
    
    -- Product metrics
    (SELECT COUNT(*) FROM products WHERE stock_quantity = 0) AS out_of_stock_products,
    (SELECT COUNT(*) FROM products WHERE stock_quantity < min_stock_level) AS low_stock_products,
    
    -- Comparison with yesterday
    (SELECT COALESCE(SUM(total_amount), 0) FROM orders WHERE DATE(order_date) = DATE_SUB(CURDATE(), INTERVAL 1 DAY)) AS revenue_yesterday

UNION ALL

SELECT 
    'This Month' AS period,
    LAST_DAY(CURDATE()) AS date,
    
    (SELECT COUNT(*) FROM orders WHERE YEAR(order_date) = YEAR(CURDATE()) AND MONTH(order_date) = MONTH(CURDATE())) AS orders_this_month,
    (SELECT COALESCE(SUM(total_amount), 0) FROM orders WHERE YEAR(order_date) = YEAR(CURDATE()) AND MONTH(order_date) = MONTH(CURDATE())) AS revenue_this_month,
    (SELECT COUNT(*) FROM customers WHERE YEAR(created_at) = YEAR(CURDATE()) AND MONTH(created_at) = MONTH(CURDATE())) AS new_customers_this_month,
    (SELECT COUNT(DISTINCT customer_id) FROM orders WHERE YEAR(order_date) = YEAR(CURDATE()) AND MONTH(order_date) = MONTH(CURDATE())) AS active_customers_this_month,
    (SELECT COUNT(*) FROM products WHERE stock_quantity = 0) AS out_of_stock_products,
    (SELECT COUNT(*) FROM products WHERE stock_quantity < min_stock_level) AS low_stock_products,
    (SELECT COALESCE(SUM(total_amount), 0) FROM orders WHERE YEAR(order_date) = YEAR(DATE_SUB(CURDATE(), INTERVAL 1 MONTH)) AND MONTH(order_date) = MONTH(DATE_SUB(CURDATE(), INTERVAL 1 MONTH))) AS revenue_last_month;
```

#### **View de Análise de Cohort:**

```sql
-- Análise de retenção de clientes por cohort
CREATE VIEW customer_cohort_analysis AS
SELECT 
    DATE_FORMAT(first_order_date, '%Y-%m') AS cohort_month,
    period_number,
    customers_in_period,
    ROUND(customers_in_period * 100.0 / total_customers, 2) AS retention_rate
FROM (
    SELECT 
        cohort_month,
        period_number,
        COUNT(*) AS customers_in_period,
        FIRST_VALUE(COUNT(*)) OVER (PARTITION BY cohort_month ORDER BY period_number) AS total_customers
    FROM (
        SELECT 
            c.id,
            DATE_FORMAT(c.first_order_date, '%Y-%m') AS cohort_month,
            PERIOD_DIFF(
                DATE_FORMAT(o.order_date, '%Y%m'),
                DATE_FORMAT(c.first_order_date, '%Y%m')
            ) AS period_number
        FROM (
            SELECT 
                customer_id,
                MIN(order_date) AS first_order_date
            FROM orders
            GROUP BY customer_id
        ) c
        INNER JOIN orders o ON c.customer_id = o.customer_id
        WHERE o.status = 'completed'
    ) cohort_data
    GROUP BY cohort_month, period_number
) cohort_table
ORDER BY cohort_month, period_number;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Sistema de E-commerce:**

```sql
-- 1. View de inventário crítico
CREATE VIEW critical_inventory AS
SELECT 
    p.id,
    p.sku,
    p.name,
    p.stock_quantity,
    p.min_stock_level,
    p.stock_quantity - p.min_stock_level AS stock_difference,
    c.name AS category,
    s.name AS supplier,
    s.email AS supplier_email,
    
    CASE 
        WHEN p.stock_quantity = 0 THEN 'CRITICAL - Out of Stock'
        WHEN p.stock_quantity < p.min_stock_level THEN 'WARNING - Low Stock'
        WHEN p.stock_quantity <= p.min_stock_level * 1.2 THEN 'WATCH - Getting Low'
        ELSE 'OK'
    END AS stock_status,
    
    p.price * (p.min_stock_level - p.stock_quantity) AS reorder_value_needed
    
FROM products p
LEFT JOIN categories c ON p.category_id = c.id
LEFT JOIN suppliers s ON p.supplier_id = s.id
WHERE p.status = 'active'
AND (p.stock_quantity <= p.min_stock_level * 1.2 OR p.stock_quantity = 0)
ORDER BY 
    CASE 
        WHEN p.stock_quantity = 0 THEN 1
        WHEN p.stock_quantity < p.min_stock_level THEN 2
        ELSE 3
    END,
    p.stock_quantity ASC;

-- 2. View de performance de vendedores (assumindo tabela salespeople)
CREATE VIEW salesperson_performance AS
SELECT 
    sp.id,
    sp.name,
    sp.region,
    
    -- Volume metrics
    COUNT(DISTINCT o.id) AS total_orders,
    COUNT(DISTINCT o.customer_id) AS unique_customers,
    SUM(oi.quantity) AS total_items_sold,
    
    -- Revenue metrics
    SUM(o.total_amount) AS total_revenue,
    AVG(o.total_amount) AS avg_order_value,
    
    -- Performance metrics
    SUM(o.total_amount) / COUNT(DISTINCT o.id) AS revenue_per_order,
    COUNT(DISTINCT o.customer_id) / COUNT(DISTINCT o.id) AS customer_diversity,
    
    -- Time-based metrics
    MIN(o.order_date) AS first_sale_date,
    MAX(o.order_date) AS last_sale_date,
    DATEDIFF(MAX(o.order_date), MIN(o.order_date)) + 1 AS active_days,
    
    -- Rankings
    RANK() OVER (ORDER BY SUM(o.total_amount) DESC) AS revenue_rank,
    RANK() OVER (ORDER BY COUNT(DISTINCT o.id) DESC) AS volume_rank

FROM salespeople sp
LEFT JOIN orders o ON sp.id = o.salesperson_id
LEFT JOIN order_items oi ON o.id = oi.order_id
WHERE o.status = 'completed'
GROUP BY sp.id, sp.name, sp.region;

-- 3. View de análise de abandono de carrinho
CREATE VIEW cart_abandonment_analysis AS
SELECT 
    DATE(sc.created_at) AS date,
    COUNT(DISTINCT sc.id) AS total_carts,
    COUNT(DISTINCT CASE WHEN o.id IS NOT NULL THEN sc.id END) AS converted_carts,
    COUNT(DISTINCT sc.id) - COUNT(DISTINCT CASE WHEN o.id IS NOT NULL THEN sc.id END) AS abandoned_carts,
    
    ROUND(
        (COUNT(DISTINCT sc.id) - COUNT(DISTINCT CASE WHEN o.id IS NOT NULL THEN sc.id END)) * 100.0 / 
        COUNT(DISTINCT sc.id), 2
    ) AS abandonment_rate,
    
    SUM(CASE WHEN o.id IS NULL THEN sc.total_value ELSE 0 END) AS lost_revenue,
    AVG(CASE WHEN o.id IS NULL THEN sc.total_value END) AS avg_abandoned_value

FROM shopping_carts sc
LEFT JOIN orders o ON sc.customer_id = o.customer_id 
    AND DATE(sc.created_at) = DATE(o.order_date)
WHERE sc.status = 'active'
GROUP BY DATE(sc.created_at)
ORDER BY date DESC;
```

### **⚡ Performance e Otimização:**

#### **Views e Performance:**

```sql
❌ View lenta - JOINs desnecessários, sem índices  
CREATE VIEW slow_customer_orders AS  
SELECT  
c._, -- Selecionar tudo é ineficiente  
o._,  
p._,  
cat._  
FROM customers c  
LEFT JOIN orders o ON c.id = o.customer_id  
LEFT JOIN order_items oi ON o.id = oi.order_id  
LEFT JOIN products p ON oi.product_id = p.id  
LEFT JOIN categories cat ON p.category_id = cat.id; -- Múltiplos JOINs desnecessários

-- ✅ View otimizada - Apenas campos necessários, JOINs eficientes  
CREATE VIEW optimized_customer_orders AS  
SELECT  
c.id AS customer_id,  
c.first_name,  
c.last_name,  
o.id AS order_id,  
o.order_date,  
o.total_amount,  
o.status  
FROM customers c  
INNER JOIN orders o ON c.id = o.customer_id -- INNER JOIN se sempre há relação  
WHERE c.status = 'active' -- Filtrar na view  
AND o.order_date >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR); -- Limitar dados

-- Criar índices para suportar a view  
CREATE INDEX idx_customers_status ON customers(status);  
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);
```


#### **Views Materializadas (Conceito):**

```sql
-- MySQL não tem views materializadas nativas, mas pode simular:

-- 1. Criar tabela para cache
CREATE TABLE materialized_monthly_sales AS
SELECT 
    YEAR(order_date) AS year,
    MONTH(order_date) AS month,
    COUNT(*) AS order_count,
    SUM(total_amount) AS revenue
FROM orders 
WHERE status = 'completed'
GROUP BY YEAR(order_date), MONTH(order_date);

-- 2. Criar evento para atualizar automaticamente
CREATE EVENT refresh_monthly_sales
ON SCHEDULE EVERY 1 DAY
STARTS TIMESTAMP(CURRENT_DATE + INTERVAL 1 DAY, '01:00:00')
DO
BEGIN
    TRUNCATE TABLE materialized_monthly_sales;
    INSERT INTO materialized_monthly_sales
    SELECT 
        YEAR(order_date) AS year,
        MONTH(order_date) AS month,
        COUNT(*) AS order_count,
        SUM(total_amount) AS revenue
    FROM orders 
    WHERE status = 'completed'
    GROUP BY YEAR(order_date), MONTH(order_date);
END;

-- 3. Usar tabela materializada em vez de view complexa
SELECT * FROM materialized_monthly_sales 
WHERE year = 2024 
ORDER BY month;
```

### **🔧 Gerenciar Views:**

#### **Alterar Views:**

```sql
-- Alterar definição da view
CREATE OR REPLACE VIEW customer_stats AS
SELECT 
    c.id,
    c.first_name,
    c.last_name,
    c.email,
    COUNT(o.id) AS total_orders,
    COALESCE(SUM(o.total_amount), 0) AS total_spent,
    MAX(o.order_date) AS last_order_date,
    
    -- Nova coluna adicionada
    AVG(DATEDIFF(o.shipped_date, o.order_date)) AS avg_processing_days,
    
    -- Lógica atualizada
    CASE 
        WHEN COUNT(o.id) = 0 THEN 'New'
        WHEN COUNT(o.id) <= 2 THEN 'Occasional'
        WHEN COUNT(o.id) <= 5 THEN 'Regular'
        WHEN COUNT(o.id) <= 15 THEN 'Frequent'
        ELSE 'VIP'
    END AS customer_tier

FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id AND o.status = 'completed'
GROUP BY c.id, c.first_name, c.last_name, c.email;
```

#### **Ver Informações das Views:**

```sql
-- MySQL - Listar todas as views
SELECT 
    TABLE_NAME as view_name,
    VIEW_DEFINITION,
    CHECK_OPTION,
    IS_UPDATABLE
FROM INFORMATION_SCHEMA.VIEWS 
WHERE TABLE_SCHEMA = 'your_database'
ORDER BY TABLE_NAME;

-- Ver definição completa de uma view
SHOW CREATE VIEW customer_stats;

-- Ver colunas de uma view
DESCRIBE customer_stats;
```

#### **Remover Views:**

```sql
-- Remover view específica
DROP VIEW customer_stats;

-- Remover se existir (não dá erro se não existir)
DROP VIEW IF EXISTS customer_stats;

-- Remover múltiplas views
DROP VIEW view1, view2, view3;
```

### **🛡️ Segurança e Permissões:**

#### **Controlo de Acesso com Views:**

sqlresponse-action-icon

```sql
-- Criar utilizador só com acesso a views específicas
CREATE USER 'marketing_user'@'%' IDENTIFIED BY 'secure_password';

-- Dar acesso apenas às views de marketing
GRANT SELECT ON your_database.marketing_customer_view TO 'marketing_user'@'%';
GRANT SELECT ON your_database.product_catalog TO 'marketing_user'@'%';
GRANT SELECT ON your_database.monthly_sales_report TO 'marketing_user'@'%';

-- Negar acesso direto às tabelas
-- (por default não tem acesso, mas ser explícito é melhor)
REVOKE ALL ON your_database.customers FROM 'marketing_user'@'%';
REVOKE ALL ON your_database.orders FROM 'marketing_user'@'%';

-- View com row-level security
CREATE VIEW user_own_orders AS
SELECT 
    o.id,
    o.order_number,
    o.order_date,
    o.status,
    o.total_amount
FROM orders o
WHERE o.customer_id = (
    SELECT id FROM customers 
    WHERE email = USER()  -- Ou outro método de identificação
);
```

#### **Views com Definer Rights:**

sqlresponse-action-icon

```sql
-- View que executa com privilégios do criador
CREATE DEFINER = 'admin'@'localhost' VIEW sensitive_customer_data AS
SELECT 
    id,
    CONCAT(first_name, ' ', last_name) AS full_name,
    LEFT(email, 3) AS email_hint,  -- Dados parcialmente mascarados
    total_spent,
    last_order_date
FROM customers
WHERE status = 'active';

-- Utilizadores podem aceder à view mas não à tabela diretamente
GRANT SELECT ON your_database.sensitive_customer_data TO 'report_user'@'%';
```

### **📊 Views para Business Intelligence:**

#### **Views para Analytics:**

sqlresponse-action-icon

```sql
-- View para análise de cohort de receita
CREATE VIEW revenue_cohort_analysis AS
SELECT 
    cohort.cohort_month,
    analysis.period_offset,
    analysis.customers,
    analysis.revenue,
    analysis.revenue_per_customer,
    ROUND(analysis.revenue * 100.0 / cohort.total_customers, 2) AS revenue_retention_rate
FROM (
    -- Cohort base
    SELECT 
        DATE_FORMAT(MIN(order_date), '%Y-%m') AS cohort_month,
        customer_id,
        COUNT(DISTINCT customer_id) OVER (PARTITION BY DATE_FORMAT(MIN(order_date), '%Y-%m')) AS total_customers
    FROM orders
    WHERE status = 'completed'
    GROUP BY customer_id
) cohort
INNER JOIN (
    -- Revenue analysis per period
    SELECT 
        c.customer_id,
        DATE_FORMAT(MIN(c.order_date), '%Y-%m') AS cohort_month,
        PERIOD_DIFF(DATE_FORMAT(o.order_date, '%Y%m'), DATE_FORMAT(MIN(c.order_date), '%Y%m')) AS period_offset,
        COUNT(DISTINCT o.customer_id) AS customers,
        SUM(o.total_amount) AS revenue,
        AVG(o.total_amount) AS revenue_per_customer
    FROM (
        SELECT customer_id, MIN(order_date) AS order_date
        FROM orders WHERE status = 'completed'
        GROUP BY customer_id
    ) c
    INNER JOIN orders o ON c.customer_id = o.customer_id
    WHERE o.status = 'completed'
    GROUP BY c.customer_id, DATE_FORMAT(MIN(c.order_date), '%Y-%m'), 
             PERIOD_DIFF(DATE_FORMAT(o.order_date, '%Y%m'), DATE_FORMAT(MIN(c.order_date), '%Y%m'))
) analysis ON cohort.customer_id = analysis.customer_id 
              AND cohort.cohort_month = analysis.cohort_month;

-- View para RFM Analysis (Recency, Frequency, Monetary)
CREATE VIEW customer_rfm_analysis AS
SELECT 
    customer_id,
    customer_name,
    
    -- Recency (days since last order)
    recency_days,
    CASE 
        WHEN recency_days <= 30 THEN 5
        WHEN recency_days <= 60 THEN 4
        WHEN recency_days <= 90 THEN 3
        WHEN recency_days <= 180 THEN 2
        ELSE 1
    END AS recency_score,
    
    -- Frequency (number of orders)
    frequency_orders,
    CASE 
        WHEN frequency_orders >= 20 THEN 5
        WHEN frequency_orders >= 10 THEN 4
        WHEN frequency_orders >= 5 THEN 3
        WHEN frequency_orders >= 2 THEN 2
        ELSE 1
    END AS frequency_score,
    
    -- Monetary (total spent)
    monetary_value,
    CASE 
        WHEN monetary_value >= 5000 THEN 5
        WHEN monetary_value >= 2000 THEN 4
        WHEN monetary_value >= 1000 THEN 3
        WHEN monetary_value >= 500 THEN 2
        ELSE 1
    END AS monetary_score,
    
    -- Combined RFM Score
    CONCAT(
        CASE 
            WHEN recency_days <= 30 THEN 5
            WHEN recency_days <= 60 THEN 4
            WHEN recency_days <= 90 THEN 3
            WHEN recency_days <= 180 THEN 2
            ELSE 1
        END,
        CASE 
            WHEN frequency_orders >= 20 THEN 5
            WHEN frequency_orders >= 10 THEN 4
            WHEN frequency_orders >= 5 THEN 3
            WHEN frequency_orders >= 2 THEN 2
            ELSE 1
        END,
        CASE 
            WHEN monetary_value >= 5000 THEN 5
            WHEN monetary_value >= 2000 THEN 4
            WHEN monetary_value >= 1000 THEN 3
            WHEN monetary_value >= 500 THEN 2
            ELSE 1
        END
    ) AS rfm_score

FROM (
    SELECT 
        c.id AS customer_id,
        CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
        DATEDIFF(CURDATE(), MAX(o.order_date)) AS recency_days,
        COUNT(o.id) AS frequency_orders,
        SUM(o.total_amount) AS monetary_value
    FROM customers c
    INNER JOIN orders o ON c.id = o.customer_id
    WHERE o.status = 'completed'
    GROUP BY c.id, c.first_name, c.last_name
) customer_metrics;
```

### **🚨 Troubleshooting de Views:**

#### **Problemas Comuns:**

sqlresponse-action-icon

```sql
-- 1. View com erro após mudança de tabela
-- Erro: "Table 'database.old_column' doesn't exist"

-- Solução: Recriar a view
CREATE OR REPLACE VIEW problematic_view AS
SELECT 
    id,
    name,
    new_column_name  -- Atualizar para novo nome
FROM updated_table;

-- 2. View muito lenta
-- Problema: JOINs sem índices, muitos dados

-- Análise:
EXPLAIN SELECT * FROM slow_view WHERE condition;

-- Solução: Adicionar índices apropriados
CREATE INDEX idx_table1_join_column ON table1(join_column);
CREATE INDEX idx_table2_join_column ON table2(join_column);

-- 3. View não atualizável quando deveria ser
-- Verificar se atende aos critérios:
SELECT 
    TABLE_NAME,
    IS_UPDATABLE,
    VIEW_DEFINITION
FROM INFORMATION_SCHEMA.VIEWS 
WHERE TABLE_NAME = 'your_view_name';

-- Critérios para view atualizável:
-- - Uma tabela apenas
-- - Sem DISTINCT, GROUP BY, HAVING
-- - Sem funções de agregação
-- - Sem subqueries na SELECT
-- - Sem UNION
```

### **📋 Best Practices para Views:**

#### **✅ Boas Práticas:**

sqlresponse-action-icon

```sql
-- 1. Nomes descritivos
CREATE VIEW active_premium_customers AS ...  -- ✅ Claro
CREATE VIEW v1 AS ...                        -- ❌ Não descritivo

-- 2. Documentar views complexas
CREATE VIEW customer_lifetime_value AS
-- View para calcular CLV baseado em:
-- - Receita total dos últimos 12 meses
-- - Frequência de compra
-- - Margem média de lucro (assumida 30%)
-- Atualizada: 2024-01-15
SELECT ...;

-- 3. Prefixos consistentes
CREATE VIEW vw_customer_summary AS ...      -- Prefixo para identificar views
CREATE VIEW rpt_monthly_sales AS ...        -- Prefixo para views de relatório

-- 4. Limitar dados quando possível
CREATE VIEW recent_orders AS
SELECT * FROM orders 
WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR);  -- Não toda a história

-- 5. Usar aliases claros
CREATE VIEW order_summary AS
SELECT 
    o.id AS order_id,
    c.id AS customer_id,
    c.first_name AS customer_first_name  -- Evitar ambiguidade
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```