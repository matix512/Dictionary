### **🔗 Conceito de JOINs:**

#### **O que são JOINs?**

> JOINs combinam linhas de duas ou mais tabelas baseado numa relação entre elas. É a operação mais poderosa do SQL para trabalhar com dados relacionais.

#### **Dataset de Exemplo:**

```sql
-- Criar tabelas para exemplos
CREATE TABLE countries (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    continent VARCHAR(50)
);

CREATE TABLE students (
    id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    country_id INT,
    FOREIGN KEY (country_id) REFERENCES countries(id)
);

-- Dados de exemplo
INSERT INTO countries VALUES 
(1, 'Portugal', 'Europe'),
(2, 'Brazil', 'South America'),
(3, 'Spain', 'Europe'),
(4, 'France', 'Europe');

INSERT INTO students VALUES 
(1, 'João', 'Silva', 1),
(2, 'Maria', 'Santos', 1),
(3, 'Carlos', 'Oliveira', 2),
(4, 'Ana', 'Costa', NULL),
(5, 'Pierre', 'Dubois', 4);
```

### **🎯 INNER JOIN:**

#### **Conceito:**

> Retorna apenas registos que têm correspondência em ambas as tabelas.

#### **Sintaxe e Exemplos:**

```sql
-- Sintaxe básica
SELECT columns
FROM table1
INNER JOIN table2 ON table1.column = table2.column;

-- Estudantes com seus países
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
INNER JOIN countries c ON s.country_id = c.id;

-- Resultado:
-- +------------+-----------+-----------+
-- | first_name | last_name | country   |
-- +------------+-----------+-----------+
-- | João       | Silva     | Portugal  |
-- | Maria      | Santos    | Portugal  |
-- | Carlos     | Oliveira  | Brazil    |
-- | Pierre     | Dubois    | France    |
-- +------------+-----------+-----------+
-- Note: Ana Costa não aparece (country_id é NULL)
```

#### **INNER JOIN com Múltiplas Tabelas:**

```sql
-- Assumindo uma terceira tabela
CREATE TABLE enrollments (
    id INT PRIMARY KEY,
    student_id INT,
    course VARCHAR(50),
    grade DECIMAL(3,1),
    FOREIGN KEY (student_id) REFERENCES students(id)
);

-- JOIN triplo
SELECT 
    s.first_name,
    s.last_name,
    c.name AS country,
    e.course,
    e.grade
FROM students s
INNER JOIN countries c ON s.country_id = c.id
INNER JOIN enrollments e ON s.id = e.student_id;
```

### **⬅️ LEFT JOIN (LEFT OUTER JOIN):**

#### **Conceito:**

> Retorna TODOS os registos da tabela da esquerda, e os registos correspondentes da tabela da direita. Se não há correspondência, mostra NULL.

#### **Exemplos:**

```sql
-- Todos os estudantes, mesmo sem país
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
LEFT JOIN countries c ON s.country_id = c.id;

-- Resultado:
-- +------------+-----------+-----------+
-- | first_name | last_name | country   |
-- +------------+-----------+-----------+
-- | João       | Silva     | Portugal  |
-- | Maria      | Santos    | Portugal  |
-- | Carlos     | Oliveira  | Brazil    |
-- | Ana        | Costa     | NULL      |
-- | Pierre     | Dubois    | France    |
-- +------------+-----------+-----------+
-- Note: Ana Costa aparece com country NULL
```

#### **LEFT JOIN para Contar:**

```sql
-- Contar estudantes por país (incluindo países sem estudantes)
SELECT 
    c.name AS country,
    COUNT(s.id) AS student_count
FROM countries c
LEFT JOIN students s ON c.id = s.country_id
GROUP BY c.id, c.name
ORDER BY student_count DESC;

-- Resultado:
-- +-----------+---------------+
-- | country   | student_count |
-- +-----------+---------------+
-- | Portugal  | 2             |
-- | Brazil    | 1             |
-- | France    | 1             |
-- | Spain     | 0             |
-- +-----------+---------------+
```

### **➡️ RIGHT JOIN (RIGHT OUTER JOIN):**

#### **Conceito:**

> Retorna TODOS os registos da tabela da direita, e os registos correspondentes da tabela da esquerda. Menos usado que LEFT JOIN.

#### **Exemplos:**

```sql
-- Todos os países, mesmo sem estudantes
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
RIGHT JOIN countries c ON s.country_id = c.id;

-- Equivalente ao LEFT JOIN anterior, mas com tabelas trocadas
SELECT s.first_name, s.last_name, c.name AS country
FROM countries c
LEFT JOIN students s ON c.id = s.country_id;
```

### **🔄 FULL OUTER JOIN:**

#### **Conceito:**

> Retorna TODOS os registos quando há correspondência na tabela esquerda OU direita.

#### **Exemplos:**

```sql
-- SQL Server/PostgreSQL
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
FULL OUTER JOIN countries c ON s.country_id = c.id;

-- MySQL não tem FULL OUTER JOIN, simular com UNION
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
LEFT JOIN countries c ON s.country_id = c.id
UNION
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
RIGHT JOIN countries c ON s.country_id = c.id;
```

### **❌ CROSS JOIN:**

#### **Conceito:**

> Produto cartesiano - cada linha da primeira tabela é combinada com cada linha da segunda tabela.

#### **Exemplos:**

sqlresponse-action-icon

```sql
-- Todas as combinações possíveis (cuidado com tabelas grandes!)
SELECT s.first_name, c.name AS country
FROM students s
CROSS JOIN countries c;

-- Resultado: 5 estudantes × 4 países = 20 linhas
-- +------------+-----------+
-- | first_name | country   |
-- +------------+-----------+
-- | João       | Portugal  |
-- | João       | Brazil    |
-- | João       | Spain     |
-- | João       | France    |
-- | Maria      | Portugal  |
-- | ...        | ...       |
-- +------------+-----------+
```

### **🎯 Condições de JOIN:**

#### **JOIN com Múltiplas Condições:**

sqlresponse-action-icon

```sql
-- Múltiplas condições no ON
SELECT s.first_name, c.name
FROM students s
INNER JOIN countries c ON s.country_id = c.id 
                       AND c.continent = 'Europe';

-- Condição adicional no WHERE
SELECT s.first_name, c.name
FROM students s
INNER JOIN countries c ON s.country_id = c.id
WHERE c.continent = 'Europe';
```

#### **Diferença ON vs WHERE:**

sqlresponse-action-icon

```sql
-- Com LEFT JOIN, WHERE filtra DEPOIS do JOIN
SELECT s.first_name, c.name
FROM students s
LEFT JOIN countries c ON s.country_id = c.id
WHERE c.continent = 'Europe';  -- Remove Ana Costa do resultado

-- ON filtra DURANTE o JOIN
SELECT s.first_name, c.name
FROM students s
LEFT JOIN countries c ON s.country_id = c.id 
                      AND c.continent = 'Europe';  -- Ana Costa fica com country NULL
```

### **🏗️ Self JOIN:**

#### **Conceito:**

> Uma tabela faz JOIN consigo mesma. Útil para hierarquias.

#### **Exemplo - Estrutura de Funcionários:**

sqlresponse-action-icon

```sql
-- Tabela de funcionários
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    manager_id INT
);

INSERT INTO employees VALUES 
(1, 'CEO', NULL),
(2, 'Director A', 1),
(3, 'Director B', 1),
(4, 'Manager A1', 2),
(5, 'Employee A1a', 4);

-- Self JOIN para ver funcionário e seu chefe
SELECT 
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;

-- Resultado:
-- +-------------+-----------+
-- | employee    | manager   |
-- +-------------+-----------+
-- | CEO         | NULL      |
-- | Director A  | CEO       |
-- | Director B  | CEO       |
-- | Manager A1  | Director A|
-- | Employee A1a| Manager A1|
-- +-------------+-----------+
```

### **📊 JOINs com Agregações:**

#### **Estatísticas com GROUP BY:**

sqlresponse-action-icon

```sql
-- Número de estudantes por país e continente
SELECT 
    c.continent,
    c.name AS country,
    COUNT(s.id) AS student_count,
    GROUP_CONCAT(s.first_name) AS student_names  -- MySQL
FROM countries c
LEFT JOIN students s ON c.id = s.country_id
GROUP BY c.continent, c.id, c.name
ORDER BY c.continent, student_count DESC;
```

#### **Subquery vs JOIN Performance:**

sqlresponse-action-icon

```sql
-- ❌ Lento com subquery
SELECT first_name 
FROM students 
WHERE country_id IN (
    SELECT id FROM countries WHERE continent = 'Europe'
);

-- ✅ Rápido com JOIN
SELECT s.first_name 
FROM students s
INNER JOIN countries c ON s.country_id = c.id
WHERE c.continent = 'Europe';
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - JOINs Básicos:**

sqlresponse-action-icon

```sql
-- 1. Todos os estudantes europeus
SELECT s.first_name, s.last_name, c.name AS country
FROM students s
INNER JOIN countries c ON s.country_id = c.id
WHERE c.continent = 'Europe';

-- 2. Países sem estudantes
SELECT c.name AS country
FROM countries c
LEFT JOIN students s ON c.id = s.country_id
WHERE s.id IS NULL;

-- 3. Contar estudantes por continente
SELECT c.continent, COUNT(s.id) AS student_count
FROM countries c
LEFT JOIN students s ON c.id = s.country_id
GROUP BY c.continent;
```

#### **Exercício 2 - Avançado:**

sqlresponse-action-icon

```sql
-- 1. Estudantes do mesmo país
SELECT 
    s1.first_name + ' ' + s1.last_name AS student1,
    s2.first_name + ' ' + s2.last_name AS student2,
    c.name AS country
FROM students s1
INNER JOIN students s2 ON s1.country_id = s2.country_id AND s1.id < s2.id
INNER JOIN countries c ON s1.country_id = c.id;

-- 2. País com mais estudantes
SELECT c.name, COUNT(s.id) AS count
FROM countries c
LEFT JOIN students s ON c.id = s.country_id
GROUP BY c.id, c.name
ORDER BY count DESC
LIMIT 1;
```

### **⚡ Performance Tips:**

#### **1. Índices em Colunas de JOIN:**

sqlresponse-action-icon

```sql
-- Sempre indexar foreign keys
CREATE INDEX idx_students_country ON students(country_id);
CREATE INDEX idx_enrollments_student ON enrollments(student_id);
```

#### **2. Ordem das Tabelas:**

sqlresponse-action-icon

```sql
-- Geralmente, tabela menor primeiro
-- ✅ Menor → Maior
SELECT * 
FROM small_table s
INNER JOIN large_table l ON s.id = l.small_id;
```

#### **3. Usar EXPLAIN:**

sqlresponse-action-icon

```sql
-- Ver plano de execução
EXPLAIN 
SELECT s.first_name, c.name
FROM students s
INNER JOIN countries c ON s.country_id = c.id;
```