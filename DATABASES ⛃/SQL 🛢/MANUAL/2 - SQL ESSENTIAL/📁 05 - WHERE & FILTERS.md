### **🎯 Introdução ao WHERE:**

#### **Propósito:**

> A cláusula WHERE filtra linhas baseado em condições específicas. É executada ANTES do SELECT, ou seja, primeiro filtra, depois seleciona.

#### **Sintaxe:**

sqlresponse-action-icon

```sql
SELECT columns
FROM table
WHERE condition;
```

### **⚖️ Operadores de Comparação:**

#### **Operadores Básicos:**

```sql
-- Igualdade
SELECT * FROM students WHERE first_name = 'João';

-- Desigualdade
SELECT * FROM students WHERE first_name != 'João';
SELECT * FROM students WHERE first_name <> 'João';  -- Padrão SQL

-- Comparações numéricas
SELECT * FROM students WHERE YEAR(birth_date) > 1995;
SELECT * FROM students WHERE YEAR(birth_date) >= 1995;
SELECT * FROM students WHERE YEAR(birth_date) < 1990;
SELECT * FROM students WHERE YEAR(birth_date) <= 1990;
```

#### **Exemplos Práticos:**

```sql
-- Estudantes nascidos após 1995
SELECT first_name, last_name, birth_date
FROM students
WHERE YEAR(birth_date) > 1995;

-- Estudantes com email específico
SELECT * FROM students WHERE email = 'joao@email.com';

-- Estudantes que NÃO são João
SELECT * FROM students WHERE first_name != 'João';
```

### **🔗 Operadores Lógicos:**

#### **AND - Todas as condições devem ser verdadeiras:**

```sql
-- Nascidos nos anos 90 E chamados João
SELECT * FROM students 
WHERE YEAR(birth_date) BETWEEN 1990 AND 1999 
  AND first_name = 'João';

-- Múltiplas condições AND
SELECT * FROM students
WHERE first_name = 'Maria'
  AND YEAR(birth_date) > 1990
  AND email LIKE '%email.com';
```

#### **OR - Pelo menos uma condição deve ser verdadeira:**

```sql
-- João OU Maria
SELECT * FROM students
WHERE first_name = 'João' OR first_name = 'Maria';

-- Nascidos antes de 1990 OU depois de 2000
SELECT * FROM students
WHERE YEAR(birth_date) < 1990 OR YEAR(birth_date) > 2000;
```

#### **NOT - Inverte a condição:**

```sql
-- Todos EXCETO João
SELECT * FROM students WHERE NOT first_name = 'João';

-- NÃO nascidos nos anos 90
SELECT * FROM students WHERE NOT (YEAR(birth_date) BETWEEN 1990 AND 1999);
```

#### **Precedência e Parênteses:**

```sql
-- ❌ Ambíguo
SELECT * FROM students
WHERE first_name = 'João' OR first_name = 'Maria' AND YEAR(birth_date) > 1995;

-- ✅ Claro
SELECT * FROM students
WHERE (first_name = 'João' OR first_name = 'Maria') 
  AND YEAR(birth_date) > 1995;
```

### **📝 Operadores de Texto (LIKE):**

#### **Wildcards:**

```text
% - Zero ou mais caracteres
_ - Exatamente um caractere
```

#### **Padrões LIKE:**

```sql
-- Começar com 'J'
SELECT * FROM students WHERE first_name LIKE 'J%';

-- Terminar com 'a'
SELECT * FROM students WHERE first_name LIKE '%a';

-- Conter 'ar'
SELECT * FROM students WHERE first_name LIKE '%ar%';

-- Exatamente 4 caracteres
SELECT * FROM students WHERE first_name LIKE '____';

-- Começar com 'J' e ter 4 caracteres
SELECT * FROM students WHERE first_name LIKE 'J___';

-- Segunda letra é 'o'
SELECT * FROM students WHERE first_name LIKE '_o%';
```

#### **Case Sensitivity:**

```sql
-- MySQL (não case-sensitive por padrão)
SELECT * FROM students WHERE first_name LIKE 'joão%';  -- Encontra 'João'

-- Para forçar case-sensitive no MySQL
SELECT * FROM students WHERE first_name LIKE BINARY 'joão%';

-- SQL Server (case-sensitive depende da collation)
SELECT * FROM students WHERE first_name LIKE 'joão%';
```

### **📊 Operadores de Conjunto:**

#### **IN - Lista de valores:**

```sql
-- Múltiplos nomes
SELECT * FROM students 
WHERE first_name IN ('João', 'Maria', 'Pedro');

-- Múltiplos anos
SELECT * FROM students 
WHERE YEAR(birth_date) IN (1990, 1995, 2000);

-- Equivalente a múltiplos OR
-- first_name = 'João' OR first_name = 'Maria' OR first_name = 'Pedro'
```

## **NOT IN - Excluir valores:**

```sql
-- Todos exceto alguns nomes
SELECT * FROM students 
WHERE first_name NOT IN ('João', 'Maria');

-- ⚠️ Cuidado com NULL
-- Se qualquer valor na lista IN for NULL, NOT IN retorna nenhum resultado
SELECT * FROM students 
WHERE first_name NOT IN ('João', NULL);  -- Retorna 0 linhas!
```

#### **BETWEEN - Intervalo de valores:**

```sql
-- Entre duas datas
SELECT * FROM students 
WHERE birth_date BETWEEN '1990-01-01' AND '1999-12-31';

-- Entre números
SELECT * FROM products 
WHERE price BETWEEN 100 AND 500;

-- NOT BETWEEN
SELECT * FROM students 
WHERE birth_date NOT BETWEEN '1990-01-01' AND '1999-12-31';

-- Equivalente a:
-- birth_date >= '1990-01-01' AND birth_date <= '1999-12-31'
```

### **🔍 Trabalhar com NULL:**

#### **IS NULL / IS NOT NULL:**

```sql
-- Registos sem email
SELECT * FROM students WHERE email IS NULL;

-- Registos com email
SELECT * FROM students WHERE email IS NOT NULL;

-- ❌ NUNCA usar = ou != com NULL
SELECT * FROM students WHERE email = NULL;   -- Sempre retorna 0 linhas
SELECT * FROM students WHERE email != NULL;  -- Sempre retorna 0 linhas
```

#### **Lidar com NULL em Condições:**

```sql
-- Considerar NULL como valor específico
SELECT * FROM students 
WHERE email IS NULL OR email = '';

-- Função COALESCE para valores padrão
SELECT 
    first_name,
    COALESCE(email, 'Sem email') AS email_display
FROM students;
```

### **📅 Filtros com Datas:**

#### **Comparações de Data:**

```sql
-- Data específica
SELECT * FROM students WHERE birth_date = '1995-03-15';

-- Antes de uma data
SELECT * FROM students WHERE birth_date < '1995-01-01';

-- Último ano
SELECT * FROM students 
WHERE created_at >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR);

-- Hoje
SELECT * FROM students WHERE DATE(created_at) = CURDATE();

-- Este mês
SELECT * FROM students 
WHERE YEAR(created_at) = YEAR(CURDATE()) 
  AND MONTH(created_at) = MONTH(CURDATE());
```

#### **Funções de Data em WHERE:**

```sql
-- Nascidos em março
SELECT * FROM students WHERE MONTH(birth_date) = 3;

-- Nascidos num domingo
SELECT * FROM students WHERE DAYOFWEEK(birth_date) = 1;

-- Idade específica
SELECT * FROM students 
WHERE YEAR(CURDATE()) - YEAR(birth_date) = 25;
```

### **🔢 Expressões Matemáticas:**

#### **Cálculos no WHERE:**

```sql
-- Assumindo tabela orders com total
SELECT * FROM orders WHERE total * 1.23 > 1000;  -- Com IVA

-- Percentagem
SELECT * FROM students 
WHERE (YEAR(CURDATE()) - YEAR(birth_date)) / YEAR(CURDATE()) * 100 > 25;
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Filtros Básicos:**

```sql
-- 1. Estudantes chamados Maria
SELECT * FROM students WHERE first_name = 'Maria';

-- 2. Estudantes nascidos após 1995
SELECT * FROM students WHERE YEAR(birth_date) > 1995;

-- 3. Estudantes cujo nome começa com 'A'
SELECT * FROM students WHERE first_name LIKE 'A%';

-- 4. Estudantes nascidos nos anos 90
SELECT * FROM students WHERE YEAR(birth_date) BETWEEN 1990 AND 1999;

-- 5. Estudantes que não são João nem Maria
SELECT * FROM students WHERE first_name NOT IN ('João', 'Maria');
```

#### **Exercício 2 - Combinações:**

```sql
-- 1. João ou Maria nascidos após 1992
SELECT * FROM students 
WHERE (first_name = 'João' OR first_name = 'Maria') 
  AND YEAR(birth_date) > 1992;

-- 2. Nomes com 4 letras, nascidos em março
SELECT * FROM students 
WHERE first_name LIKE '____' 
  AND MONTH(birth_date) = 3;

-- 3. Email termina em '.com' e tem data de nascimento
SELECT * FROM students 
WHERE email LIKE '%.com' 
  AND birth_date IS NOT NULL;
```

### **🚨 Erros Comuns:**

#### **1. Confundir = com LIKE:**

sqlresponse-action-icon

```sql
-- ❌ Procura texto exato 'Maria%'
SELECT * FROM students WHERE first_name = 'Maria%';

-- ✅ Procura nomes que começam com 'Maria'
SELECT * FROM students WHERE first_name LIKE 'Maria%';
```

#### **2. Esquecer aspas em strings:**

sqlresponse-action-icon

```sql
-- ❌ MySQL interpreta como coluna
SELECT * FROM students WHERE first_name = Maria;

-- ✅ String literal
SELECT * FROM students WHERE first_name = 'Maria';
```

#### **3. Problemas com NULL:**

sqlresponse-action-icon

```sql
-- ❌ Nunca encontra NULL
SELECT * FROM students WHERE email = NULL;

-- ✅ Encontra NULL
SELECT * FROM students WHERE email IS NULL;
```

### **💡 Tips de Performance:**

#### **1. Usar Índices:**

sqlresponse-action-icon

```sql
-- Criar índice para colunas frequentemente filtradas
CREATE INDEX idx_students_first_name ON students(first_name);
CREATE INDEX idx_students_birth_date ON students(birth_date);
```

#### **2. Evitar Funções em WHERE:**

sqlresponse-action-icon

```sql
-- ❌ Lento (função em cada linha)
SELECT * FROM students WHERE YEAR(birth_date) = 1995;

-- ✅ Rápido (usa índice)
SELECT * FROM students 
WHERE birth_date >= '1995-01-01' 
  AND birth_date <= '1995-12-31';
```