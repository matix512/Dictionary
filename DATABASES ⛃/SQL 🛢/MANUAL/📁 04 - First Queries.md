### **🎯 Sintaxe Básica do SELECT:**

#### **Estrutura Fundamental:**

sqlresponse-action-icon

```sql
SELECT [DISTINCT] column1, column2, ...
FROM table_name
[WHERE condition]
[ORDER BY column ASC|DESC]
[LIMIT number];
```

#### **Primeira Query:**

sqlresponse-action-icon

```sql
-- Ver todos os dados
SELECT * FROM students;

-- Resultado esperado:
-- +----+------------+-----------+------------------+------------+---------------------+
-- | id | first_name | last_name | email            | birth_date | created_at          |
-- +----+------------+-----------+------------------+------------+---------------------+
-- |  1 | João       | Silva     | joao@email.com   | 1995-03-15 | 2024-01-15 10:30:00 |
-- |  2 | Maria      | Santos    | maria@email.com  | 1992-07-22 | 2024-01-15 10:30:01 |
-- +----+------------+-----------+------------------+------------+---------------------+
```

### **📋 Selecionar Colunas Específicas:**

#### **Colunas Individuais:**

sqlresponse-action-icon

```sql
-- Uma coluna
SELECT first_name FROM students;

-- Múltiplas colunas
SELECT first_name, last_name FROM students;

-- Todas as colunas (evitar em produção)
SELECT * FROM students;
```

#### **Ordem das Colunas:**

sqlresponse-action-icon

```sql
-- A ordem no SELECT define a ordem no resultado
SELECT last_name, first_name, email FROM students;
-- last_name aparece primeiro
```


### **🏷️ Aliases (Apelidos):**

#### **Aliases para Colunas:**

sqlresponse-action-icon

```sql
-- Usando AS (recomendado)
SELECT 
    first_name AS nome,
    last_name AS sobrenome,
    email AS correio_eletronico
FROM students;

-- AS é opcional
SELECT 
    first_name nome,
    last_name sobrenome
FROM students;

-- com espaços(usar aspas)
```