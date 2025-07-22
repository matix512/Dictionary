### **🔤 Funções de String:**

#### **Concatenação:**

sqlresponse-action-icon

```sql
-- MySQL
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM students;
SELECT CONCAT('Sr. ', first_name, ' ', last_name) AS formal_name FROM students;

-- SQL Server
SELECT first_name + ' ' + last_name AS full_name FROM students;
SELECT 'Sr. ' + first_name + ' ' + last_name AS formal_name FROM students;

-- Concatenação com separador (MySQL)
SELECT CONCAT_WS(' ', 'Sr.', first_name, last_name) AS full_name FROM students;
-- CONCAT_WS ignora valores NULL automaticamente
```

#### **Comprimento e Posição:**

sqlresponse-action-icon

```sql
-- Comprimento da string
SELECT first_name, LENGTH(first_name) AS name_length FROM students;    -- MySQL
SELECT first_name, LEN(first_name) AS name_length FROM students;       -- SQL Server

-- Posição de substring
SELECT email, LOCATE('@', email) AS at_position FROM students;         -- MySQL
SELECT email, CHARINDEX('@', email) AS at_position FROM students;      -- SQL Server

-- Encontrar última ocorrência
SELECT email, LENGTH(email) - LENGTH(SUBSTRING_INDEX(email, '.', -1)) AS last_dot FROM students;  -- MySQL
```

#### **Extração de Substrings:**

sqlresponse-action-icon

```sql
-- SUBSTRING/SUBSTR
SELECT first_name, SUBSTRING(first_name, 1, 3) AS first_3_chars FROM students;  -- Universal
SELECT first_name, SUBSTR(first_name, 1, 3) AS first_3_chars FROM students;     -- MySQL

-- LEFT e RIGHT
SELECT email, LEFT(email, 3) AS first_3 FROM students;                 -- MySQL/SQL Server
SELECT email, RIGHT(email, 4) AS last_4 FROM students;                 -- MySQL/SQL Server

-- Extrair domínio do email
SELECT email, SUBSTRING(email, LOCATE('@', email) + 1) AS domain FROM students;  -- MySQL
SELECT email, SUBSTRING(email, CHARINDEX('@', email) + 1, 100) AS domain FROM students;  -- SQL Server
```

#### **Mudança de Caso:**

sqlresponse-action-icon

```sql
-- Maiúsculas e minúsculas
SELECT UPPER(first_name) AS upper_name FROM students;
SELECT LOWER(first_name) AS lower_name FROM students;

-- Primeira letra maiúscula (função não padrão, varia por SGBD)
-- MySQL (função personalizada necessária)
SELECT CONCAT(UPPER(LEFT(first_name, 1)), LOWER(SUBSTRING(first_name, 2))) AS proper_name FROM students;

-- SQL Server
SELECT CONCAT(UPPER(LEFT(first_name, 1)), LOWER(SUBSTRING(first_name, 2, 100))) AS proper_name FROM students;
```

#### **Limpeza e Formatação:**

sqlresponse-action-icon

```sql
-- Remover espaços
SELECT TRIM(first_name) AS trimmed FROM students;                       -- Ambos lados
SELECT LTRIM(first_name) AS left_trimmed FROM students;                 -- Lado esquerdo
SELECT RTRIM(first_name) AS right_trimmed FROM students;                -- Lado direito

-- Remover caracteres específicos
SELECT TRIM('.' FROM first_name) AS no_dots FROM students;              -- MySQL
SELECT REPLACE(first_name, '.', '') AS no_dots FROM students;          -- Universal

-- Substituir texto
SELECT REPLACE(email, '@email.com', '@newemail.com') AS new_email FROM students;

-- Repetir string
SELECT REPEAT('*', 5) AS stars FROM students;                          -- MySQL
SELECT REPLICATE('*', 5) AS stars FROM students;                       -- SQL Server
```

### **📅 Funções de Data:**

#### **Data e Hora Atual:**

sqlresponse-action-icon

```sql
-- Data e hora atual
SELECT NOW() AS current_datetime FROM students;                        -- MySQL
SELECT GETDATE() AS current_datetime FROM students;                    -- SQL Server
SELECT CURRENT_TIMESTAMP AS current_datetime FROM students;           -- Padrão SQL

-- Apenas data
SELECT CURDATE() AS current_date FROM students;                        -- MySQL
SELECT CAST(GETDATE() AS DATE) AS current_date FROM students;         -- SQL Server

-- Apenas hora
SELECT CURTIME() AS current_time FROM students;                        -- MySQL
SELECT CAST(GETDATE() AS TIME) AS current_time FROM students;         -- SQL Server
```

#### **Extrair Partes da Data:**

sqlresponse-action-icon

```sql
-- Extrair componentes
SELECT birth_date,
       YEAR(birth_date) AS birth_year,
       MONTH(birth_date) AS birth_month,
       DAY(birth_date) AS birth_day,
       DAYOFWEEK(birth_date) AS day_of_week,        -- MySQL (1=Domingo)
       DAYNAME(birth_date) AS day_name,             -- MySQL
       MONTHNAME(birth_date) AS month_name          -- MySQL
FROM students;

-- SQL Server equivalente
SELECT birth_date,
       YEAR(birth_date) AS birth_year,
       MONTH(birth_date) AS birth_month,
       DAY(birth_date) AS birth_day,
       DATEPART(WEEKDAY, birth_date) AS day_of_week,
       DATENAME(WEEKDAY, birth_date) AS day_name,
       DATENAME(MONTH, birth_date) AS month_name
FROM students;
```

#### **Cálculos com Datas:**

sqlresponse-action-icon

```sql
-- Diferença entre datas
SELECT birth_date,
       CURDATE() AS today,
       DATEDIFF(CURDATE(), birth_date) AS days_old,             -- MySQL
       YEAR(CURDATE()) - YEAR(birth_date) AS age_years          -- Aproximado
FROM students;

-- SQL Server
SELECT birth_date,
       GETDATE() AS today,
       DATEDIFF(DAY, birth_date, GETDATE()) AS days_old,
       DATEDIFF(YEAR, birth_date, GETDATE()) AS age_years
FROM students;

-- Idade exata
SELECT birth_date,
       YEAR(CURDATE()) - YEAR(birth_date) - 
       (DATE_FORMAT(CURDATE(), '%m%d') < DATE_FORMAT(birth_date, '%m%d')) AS exact_age
FROM students;
```

#### **Adicionar/Subtrair Tempo:**

sqlresponse-action-icon

```sql
-- MySQL
SELECT birth_date,
       DATE_ADD(birth_date, INTERVAL 18 YEAR) AS age_18,
       DATE_SUB(birth_date, INTERVAL 1 MONTH) AS month_before,
       DATE_ADD(birth_date, INTERVAL 30 DAY) AS month_after
FROM students;

-- SQL Server  
SELECT birth_date,
       DATEADD(YEAR, 18, birth_date) AS age_18,
       DATEADD(MONTH, -1, birth_date) AS month_before,
       DATEADD(DAY, 30, birth_date) AS month_after
FROM students;
```

### **📊 Formatação de Data:**

#### **Formatos Personalizados:**

sqlresponse-action-icon

```sql
-- MySQL
SELECT birth_date,
       DATE_FORMAT(birth_date, '%d/%m/%Y') AS portuguese_format,
       DATE_FORMAT(birth_date, '%W, %M %d, %Y') AS full_format,
       DATE_FORMAT(birth_date, '%d-%b-%y') AS short_format
FROM students;

-- SQL Server
SELECT birth_date,
       FORMAT(birth_date, 'dd/MM/yyyy') AS portuguese_format,
       FORMAT(birth_date, 'dddd, MMMM dd, yyyy') AS full_format,
       FORMAT(birth_date, 'dd-MMM-yy') AS short_format
FROM students;
```

### **🔢 Funções Numéricas (Bonus):**

#### **Matemáticas Básicas:**

sqlresponse-action-icon

```sql
-- Arredondamento
SELECT 3.14159 AS pi,
       ROUND(3.14159, 2) AS rounded,
       CEIL(3.14159) AS ceiling,
       FLOOR(3.14159) AS floor,
       TRUNCATE(3.14159, 3) AS truncated;

-- Valor absoluto e sinal
SELECT ABS(-15) AS absolute,
       SIGN(-15) AS sign_negative,
       SIGN(15) AS sign_positive;

-- Potência e raiz
SELECT POWER(2, 3) AS power,
       SQRT(16) AS square_root;
```

### **🎯 Casos Práticos Combinados:**

#### **Exemplo 1 - Relatório de Estudantes:**

sqlresponse-action-icon

```sql
SELECT 
    CONCAT(UPPER(LEFT(first_name, 1)), LOWER(SUBSTRING(first_name, 2)), ' ', 
           UPPER(LEFT(last_name, 1)), LOWER(SUBSTRING(last_name, 2))) AS proper_name,
    UPPER(LEFT(email, LENGTH(email) - LENGTH(SUBSTRING_INDEX(email, '@', -1)) - 1)) AS email_user,
    SUBSTRING_INDEX(email, '@', -1) AS email_domain,
    birth_date,
    DATE_FORMAT(birth_date, '%W, %d de %M de %Y') AS birth_formatted,
    YEAR(CURDATE()) - YEAR(birth_date) AS age,
    CASE 
        WHEN YEAR(CURDATE()) - YEAR(birth_date) < 18 THEN 'Menor'
        WHEN YEAR(CURDATE()) - YEAR(birth_date) >= 65 THEN 'Senior'
        ELSE 'Adulto'
    END AS age_category
FROM students;
```

#### **Exemplo 2 - Análise de Emails:**

sqlresponse-action-icon

```sql
SELECT 
    COUNT(*) AS total_students,
    COUNT(DISTINCT SUBSTRING_INDEX(email, '@', -1)) AS unique_domains,
    AVG(LENGTH(email)) AS avg_email_length,
    MAX(LENGTH(CONCAT(first_name, last_name))) AS longest_name
FROM students
WHERE email IS NOT NULL;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Strings:**

sqlresponse-action-icon

```sql
-- 1. Nome completo em maiúsculas
SELECT UPPER(CONCAT(first_name, ' ', last_name)) AS full_name_upper FROM students;

-- 2. Primeiro nome com apenas 3 caracteres
SELECT LEFT(first_name, 3) AS short_name FROM students;

-- 3. Domínio do email
SELECT SUBSTRING_INDEX(email, '@', -1) AS domain FROM students WHERE email IS NOT NULL;

-- 4. Iniciais do nome
SELECT CONCAT(LEFT(first_name, 1), LEFT(last_name, 1)) AS initials FROM students;
```

#### **Exercício 2 - Datas:**

sqlresponse-action-icon

```sql
-- 1. Idade em anos completos
SELECT first_name, 
       YEAR(CURDATE()) - YEAR(birth_date) - 
       (DATE_FORMAT(CURDATE(), '%m%d') < DATE_FORMAT(birth_date, '%m%d')) AS exact_age
FROM students;

-- 2. Próximo aniversário
SELECT first_name, birth_date,
       DATE_ADD(CONCAT(YEAR(CURDATE()), '-', MONTH(birth_date), '-', DAY(birth_date)), INTERVAL 0 DAY) AS next_birthday
FROM students;

-- 3. Nascidos há quantos dias
SELECT first_name, DATEDIFF(CURDATE(), birth_date) AS days_since_birth FROM students;
```

### **💡 Dicas de Performance:**

#### **1. Evitar Funções em WHERE:**

sqlresponse-action-icon

```sql
-- ❌ Lento
SELECT * FROM students WHERE YEAR(birth_date) = 1995;

-- ✅ Rápido
SELECT * FROM students WHERE birth_date >= '1995-01-01' AND birth_date < '1996-01-01';
```

#### **2. Usar Colunas Calculadas:**

sqlresponse-action-icon

```sql
-- Para consultas frequentes, criar coluna calculada
ALTER TABLE students ADD COLUMN age_years INT AS (YEAR(CURDATE()) - YEAR(birth_date));
CREATE INDEX idx_age ON students(age_years);
```