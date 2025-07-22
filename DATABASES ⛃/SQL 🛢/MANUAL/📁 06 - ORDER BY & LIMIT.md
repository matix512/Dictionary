### **🎯 ORDER BY - Ordenação:**

#### **Sintaxe Básica:**

sqlresponse-action-icon

```sql
SELECT columns
FROM table
WHERE condition
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...
[LIMIT number];
```

#### **Ordem Crescente (ASC - padrão):**

sqlresponse-action-icon

```sql
-- Por nome (A-Z)
SELECT * FROM students ORDER BY first_name;
SELECT * FROM students ORDER BY first_name ASC;  -- Mesmo resultado

-- Por data de nascimento (mais antigo primeiro)
SELECT * FROM students ORDER BY birth_date;

-- Por idade (mais novo primeiro)
SELECT first_name, birth_date, 
       YEAR(CURDATE()) - YEAR(birth_date) AS age
FROM students 
ORDER BY age;
```

#### **Ordem Decrescente (DESC):**

sqlresponse-action-icon

```sql
-- Por nome (Z-A)
SELECT * FROM students ORDER BY first_name DESC;

-- Por data de nascimento (mais recente primeiro)
SELECT * FROM students ORDER BY birth_date DESC;

-- Por idade (mais velho primeiro)
SELECT first_name, birth_date,
       YEAR(CURDATE()) - YEAR(birth_date) AS age
FROM students 
ORDER BY age DESC;
```

### **📋 Ordenação por Múltiplas Colunas:**

#### **Prioridade de Ordenação:**

sqlresponse-action-icon

```sql
-- Primeiro por último nome, depois por primeiro nome
SELECT * FROM students 
ORDER BY last_name ASC, first_name ASC;

-- Por ano de nascimento (desc), depois por nome (asc)
SELECT first_name, last_name, birth_date
FROM students 
ORDER BY YEAR(birth_date) DESC, first_name ASC;

-- Combinação de ordens
SELECT * FROM students 
ORDER BY last_name DESC, first_name ASC, birth_date DESC;
```

### **🔢 Ordenar por Posição:**

#### **Usar Número da Coluna:**

sqlresponse-action-icon

```sql
-- Ordenar pela 2ª coluna do SELECT
SELECT first_name, last_name, birth_date 
FROM students 
ORDER BY 2;  -- Ordena por last_name

-- Múltiplas posições
SELECT first_name, last_name, birth_date 
FROM students 
ORDER BY 3 DESC, 1 ASC;  -- birth_date DESC, first_name ASC
```

### **🎨 Ordenar por Expressões:**

#### **Ordenar por Cálculos:**

sqlresponse-action-icon

```sql
-- Por idade calculada
SELECT first_name, birth_date,
       YEAR(CURDATE()) - YEAR(birth_date) AS age
FROM students 
ORDER BY YEAR(CURDATE()) - YEAR(birth_date) DESC;

-- Por nome completo
SELECT first_name, last_name,
       CONCAT(first_name, ' ', last_name) AS full_name
FROM students 
ORDER BY CONCAT(first_name, ' ', last_name);

-- Por comprimento do nome
SELECT first_name, LENGTH(first_name) AS name_length
FROM students 
ORDER BY LENGTH(first_name) DESC;
```

### **🔍 LIMIT - Limitar Resultados:**

#### **LIMIT Básico:**

sqlresponse-action-icon

```sql
-- Primeiros 5 registos
SELECT * FROM students LIMIT 5;

-- Top 3 mais novos
SELECT first_name, birth_date 
FROM students 
ORDER BY birth_date DESC 
LIMIT 3;

-- Último estudante por ordem alfabética
SELECT * FROM students 
ORDER BY first_name DESC 
LIMIT 1;
```

#### **LIMIT com OFFSET (Paginação):**

sqlresponse-action-icon

```sql
-- MySQL/PostgreSQL
SELECT * FROM students 
ORDER BY first_name 
LIMIT 5 OFFSET 10;  -- Saltar 10, mostrar próximos 5

-- MySQL sintaxe alternativa
SELECT * FROM students 
ORDER BY first_name 
LIMIT 10, 5;  -- Saltar 10, mostrar 5

-- SQL Server (TOP com OFFSET)
SELECT * FROM students 
ORDER BY first_name 
OFFSET 10 ROWS 
FETCH NEXT 5 ROWS ONLY;
```

### **📊 Casos de Uso Práticos:**

#### **Top N Queries:**

sqlresponse-action-icon

```sql
-- 3 estudantes mais novos
SELECT first_name, last_name, birth_date 
FROM students 
ORDER BY birth_date DESC 
LIMIT 3;

-- 5 nomes mais compridos
SELECT first_name, LENGTH(first_name) AS name_length 
FROM students 
ORDER BY LENGTH(first_name) DESC 
LIMIT 5;

-- Primeiro e último por ordem alfabética
(SELECT 'Primeiro' AS position, first_name FROM students ORDER BY first_name LIMIT 1)
UNION ALL
(SELECT 'Último' AS position, first_name FROM students ORDER BY first_name DESC LIMIT 1);
```

#### **Paginação Simples:**

sqlresponse-action-icon

```sql
-- Página 1 (primeiros 10)
SELECT * FROM students ORDER BY first_name LIMIT 10 OFFSET 0;

-- Página 2 (próximos 10)
SELECT * FROM students ORDER BY first_name LIMIT 10 OFFSET 10;

-- Página 3 (próximos 10)  
SELECT * FROM students ORDER BY first_name LIMIT 10 OFFSET 20;

-- Fórmula: OFFSET = (página - 1) * registos_por_página
```

### **🎯 Ordenação com NULL:**

#### **Comportamento de NULL:**

sqlresponse-action-icon

```sql
-- MySQL: NULL vem primeiro em ASC
SELECT first_name, email 
FROM students 
ORDER BY email ASC;

-- Para forçar NULL no final
SELECT first_name, email 
FROM students 
ORDER BY email IS NULL, email ASC;

-- Para forçar NULL no início
SELECT first_name, email 
FROM students 
ORDER BY email IS NOT NULL, email ASC;
```

### **🔤 Ordenação de Strings:**

#### **Case Sensitivity:**

sqlresponse-action-icon

```sql
-- MySQL (geralmente não case-sensitive)
SELECT first_name FROM students ORDER BY first_name;

-- Para forçar case-sensitive
SELECT first_name FROM students ORDER BY first_name COLLATE utf8mb4_bin;

-- Ignorar acentos
SELECT first_name FROM students ORDER BY first_name COLLATE utf8mb4_unicode_ci;
```

#### **Ordenação Natural:**

sqlresponse-action-icon

```sql
-- Para strings com números (item1, item10, item2 -> item1, item2, item10)
-- MySQL 8.0+
SELECT name FROM products ORDER BY name+0, name;

-- Ou usando CAST
SELECT name FROM products ORDER BY CAST(REGEXP_SUBSTR(name, '[0-9]+') AS SIGNED), name;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Ordenação Básica:**

sqlresponse-action-icon

```sql
-- 1. Estudantes por ordem alfabética
SELECT * FROM students ORDER BY first_name;

-- 2. Estudantes do mais velho para o mais novo
SELECT first_name, birth_date FROM students ORDER BY birth_date;

-- 3. Últimos 3 estudantes criados
SELECT * FROM students ORDER BY created_at DESC LIMIT 3;

-- 4. Estudantes por sobrenome, depois por nome
SELECT * FROM students ORDER BY last_name, first_name;
```

#### **Exercício 2 - Avançado:**

sqlresponse-action-icon

```sql
-- 1. Top 2 nomes mais compridos
SELECT first_name, LENGTH(first_name) AS length 
FROM students 
ORDER BY LENGTH(first_name) DESC 
LIMIT 2;

-- 2. Paginação: página 2 com 2 registos por página
SELECT * FROM students 
ORDER BY first_name 
LIMIT 2 OFFSET 2;

-- 3. Estudantes nascidos em meses pares, ordenados por mês
SELECT first_name, birth_date, MONTH(birth_date) AS birth_month
FROM students 
WHERE MONTH(birth_date) % 2 = 0
ORDER BY MONTH(birth_date);
```

### **⚡ Performance Tips:**

#### **1. Índices para ORDER BY:**

sqlresponse-action-icon

```sql
-- Criar índice para ordenação frequente
CREATE INDEX idx_students_name ON students(first_name, last_name);
CREATE INDEX idx_students_birth_date ON students(birth_date);

-- Índice composto para múltiplas colunas
CREATE INDEX idx_students_name_date ON students(last_name, first_name, birth_date);
```

#### **2. LIMIT com ORDER BY:**

sqlresponse-action-icon

```sql
-- ✅ Eficiente - para quando só precisas de poucos registos
SELECT * FROM students ORDER BY birth_date DESC LIMIT 10;

-- ❌ Ineficiente - ordena tudo para depois limitar
SELECT * FROM (SELECT * FROM students ORDER BY birth_date DESC) subquery LIMIT 10;
```

#### **3. Evitar ORDER BY em Subqueries:**

sqlresponse-action-icon

```sql
-- ❌ ORDER BY desnecessário em subquery
SELECT * FROM (
    SELECT * FROM students ORDER BY first_name
) s WHERE s.id > 5;

-- ✅ ORDER BY só no nível principal
SELECT * FROM students 
WHERE id > 5 
ORDER BY first_name;
```