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
CREATE VIEW monthly
```
```