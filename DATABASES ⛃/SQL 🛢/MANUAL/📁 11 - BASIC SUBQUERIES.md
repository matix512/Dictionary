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

sqlresponse-action-icon

```sql
-- Preço médio na base de dados
SELECT 
    product
```