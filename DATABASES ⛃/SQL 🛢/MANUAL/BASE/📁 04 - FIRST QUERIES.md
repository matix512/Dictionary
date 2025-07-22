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
```sql
SELECT 
    first_name AS "Nome Próprio",
    last_name AS "Apelido",
    email AS "E-mail"
FROM students;
```

#### **Aliases para Tabelas:**


```sql
-- Útil para JOINs posteriores
SELECT s.first_name, s.last_name
FROM students AS s;

-- Ou sem AS
SELECT s.first_name, s.last_name
FROM students s;
```

### **🎨 Expressões e Cálculos:**

#### **Concatenação de Strings:**

sqlresponse-action-icon

```sql
-- MySQL
SELECT 
    CONCAT(first_name, ' ', last_name) AS full_name
FROM students;

-- SQL Server
SELECT 
    first_name + ' ' + last_name AS full_name
FROM students;

-- Com texto fixo
SELECT 
    CONCAT('Sr./Sra. ', first_name, ' ', last_name) AS formal_name
FROM students;
```

#### **Cálculos com Números:**

sqlresponse-action-icon

```sql
-- Assumindo que temos uma tabela products
SELECT 
    name,
    price,
    price * 1.23 AS price_with_tax,
    price * 0.9 AS discounted_price
FROM products;
```

### **📅 Trabalhar com Datas:**

#### **Extrair Partes de Datas:**

sqlresponse-action-icon

```sql
-- Idade atual
SELECT 
    first_name,
    birth_date,
    YEAR(CURDATE()) - YEAR(birth_date) AS age
FROM students;

-- Extrair partes da data
SELECT 
    first_name,
    birth_date,
    YEAR(birth_date) AS birth_year,
    MONTH(birth_date) AS birth_month,
    DAY(birth_date) AS birth_day
FROM students;
```

### **🔢 DISTINCT - Valores Únicos:**

#### **Eliminar Duplicados:**

sqlresponse-action-icon

```sql
-- Anos de nascimento únicos
SELECT DISTINCT YEAR(birth_date) AS birth_year
FROM students;

-- Combinações únicas
SELECT DISTINCT first_name, YEAR(birth_date)
FROM students;
```

### **📊 Comentários no SQL:**

#### **Tipos de Comentários:**

sqlresponse-action-icon

```sql
-- Comentário de linha (duas hífenes e espaço)
SELECT first_name -- Este é o nome próprio
FROM students;

/* 
Comentário de bloco
Pode ser multilinha
*/
SELECT 
    first_name, 
    last_name
FROM students;

# MySQL também aceita # para comentário linha
SELECT first_name # Nome do estudante
FROM students;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Básico:**

sqlresponse-action-icon

```sql
-- 1. Mostrar apenas nomes e emails
-- Resposta:
SELECT first_name, email FROM students;

-- 2. Mostrar nome completo como uma coluna
-- Resposta:
SELECT CONCAT(first_name, ' ', last_name) AS nome_completo
FROM students;

-- 3. Mostrar idade de cada estudante
-- Resposta:
SELECT 
    first_name,
    YEAR(CURDATE()) - YEAR(birth_date) AS idade
FROM students;
```

### **🚨 Erros Comuns:**

#### **1. Esquecer FROM:**

sqlresponse-action-icon

```sql
-- ❌ Erro
SELECT first_name;

-- ✅ Correto
SELECT first_name FROM students;
```

#### **2. Nomes de Colunas Errados:**

sqlresponse-action-icon

```sql
-- ❌ Erro (coluna não existe)
SELECT firstname FROM students;

-- ✅ Correto
SELECT first_name FROM students;
```

#### **3. Case Sensitivity:**

sqlresponse-action-icon

```sql
-- MySQL não é case-sensitive por padrão, mas é boa prática
-- ✅ Consistente
SELECT First_Name FROM Students;  -- Funciona
SELECT first_name FROM students;  -- Melhor prática
```

### **💡 Dicas de Produtividade:**

#### **1. Formatação Legível:**

sqlresponse-action-icon

```sql
-- ❌ Difícil de ler
SELECT first_name,last_name,email FROM students;

-- ✅ Fácil de ler
SELECT 
    first_name,
    last_name,
    email
FROM students;
```

#### **2. Sempre Testar Pequeno:**

sqlresponse-action-icon

```sql
-- Primeiro ver estrutura
DESCRIBE students;

-- Depois ver alguns dados
SELECT * FROM students LIMIT 5;

-- Depois fazer query específica
SELECT first_name, last_name FROM students;
```

