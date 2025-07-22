### **🏗️ Data Definition Language (DDL):**

#### **O que é DDL?**

> DDL são comandos SQL usados para definir e modificar a estrutura da base de dados. Incluem CREATE, ALTER, DROP, TRUNCATE.

#### **Comandos Principais:**

textresponse-action-icon

```text
🆕 CREATE - Criar objetos (tabelas, índices, views)
🔧 ALTER - Modificar estrutura existente
❌ DROP - Apagar objetos permanentemente
🧹 TRUNCATE - Esvaziar tabela (mantém estrutura)
```

### **🆕 CREATE TABLE Básico:**

#### **Sintaxe Fundamental:**

```sql
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
    [table_constraints]
);
```

#### **Exemplo Simples:**

```sql
-- Tabela básica de produtos
CREATE TABLE products (
    id INT,
    name VARCHAR(100),
    price DECIMAL(10, 2),
    created_at DATETIME
);

-- Verificar estrutura criada
DESCRIBE products;  -- MySQL
-- ou
SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'products';
```

### **📊 Tipos de Dados:**

#### **Numéricos:**

```sql
CREATE TABLE numeric_examples (
    tiny_int TINYINT,           -- -128 to 127
    small_int SMALLINT,         -- -32,768 to 32,767
    medium_int MEDIUMINT,       -- MySQL específico
    regular_int INT,            -- -2,147,483,648 to 2,147,483,647
    big_int BIGINT,            -- Muito grande
    
    decimal_fixed DECIMAL(10,2), -- Precisão fixa: 999999.99
    numeric_fixed NUMERIC(10,2), -- Sinónimo de DECIMAL
    float_approx FLOAT,         -- Aproximado, 32-bit
    double_approx DOUBLE,       -- Aproximado, 64-bit
    
    -- Com unsigned (só positivos)
    unsigned_int INT UNSIGNED,  -- 0 to 4,294,967,295
    auto_id INT AUTO_INCREMENT  -- MySQL auto incremento
);
```

#### **Strings e Texto:**

```sql
CREATE TABLE string_examples (
    fixed_char CHAR(10),        -- Comprimento fixo, preenchido com espaços
    variable_varchar VARCHAR(255), -- Comprimento variável, até 255 chars
    tiny_text TINYTEXT,         -- Até 255 chars
    regular_text TEXT,          -- Até 65,535 chars
    medium_text MEDIUMTEXT,     -- Até 16,777,215 chars
    long_text LONGTEXT,         -- Até 4,294,967,295 chars
    
    -- Encoding específico
    utf8_text TEXT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
);
```

#### **Datas e Tempo:**

```sql
CREATE TABLE datetime_examples (
    just_date DATE,             -- '2024-01-15'
    just_time TIME,             -- '14:30:00'
    date_and_time DATETIME,     -- '2024-01-15 14:30:00'
    timestamp_auto TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    timestamp_update TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Ano apenas
    year_only YEAR,             -- '2024'
    
    -- Com timezone (PostgreSQL/SQL Server)
    with_timezone TIMESTAMPTZ   -- PostgreSQL
);
```

#### **Outros Tipos:**

```sql
CREATE TABLE other_types (
    true_false BOOLEAN,         -- TRUE/FALSE
    binary_data BLOB,           -- Dados binários
    json_data JSON,             -- MySQL 5.7+, PostgreSQL
    enum_status ENUM('active', 'inactive', 'pending'), -- MySQL
    
    -- Geométricos (MySQL)
    coordinates POINT,
    area_polygon POLYGON
);
```

### **🔑 Constraints (Restrições):**

#### **Primary Key:**

sqlresponse-action-icon

```sql
-- Chave primária simples
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- Chave primária nomeada
CREATE TABLE students (
    id INT,
    name VARCHAR(100),
    CONSTRAINT pk_students_id PRIMARY KEY (id)
);

-- Chave primária composta
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);

-- Auto increment
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,  -- MySQL
    name VARCHAR(100)
);
```

#### **Foreign Key:**

sqlresponse-action-icon

```sql
-- Criação com foreign key
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Foreign key nomeada com ações
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    CONSTRAINT fk_orders_customer 
        FOREIGN KEY (customer_id) 
        REFERENCES customers(id)
        ON DELETE CASCADE           -- Apaga orders se customer for apagado
        ON UPDATE CASCADE          -- Atualiza se customer.id mudar
);

-- Outras ações de referência
-- ON DELETE RESTRICT   -- Não permite apagar se há referências
-- ON DELETE SET NULL   -- Define como NULL
-- ON DELETE SET DEFAULT -- Define valor padrão
```

#### **Unique, Not NULL, Check:**

sqlresponse-action-icon

```sql
CREATE TABLE comprehensive_table (
    id INT AUTO_INCREMENT PRIMARY KEY,
    
    -- NOT NULL
    name VARCHAR(100) NOT NULL,
    
    -- UNIQUE
    email VARCHAR(200) UNIQUE,
    
    -- UNIQUE nomeado
    username VARCHAR(50),
    CONSTRAINT uk_username UNIQUE (username),
    
    -- CHECK constraint
    age INT CHECK (age >= 0 AND age <= 150),
    
    -- DEFAULT values
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    -- Múltiplas constraints
    salary DECIMAL(10,2) NOT NULL DEFAULT 0.00 CHECK (salary >= 0)
);
```

### **🎯 Exemplos Práticos Completos:**

#### **Sistema de E-commerce:**

sqlresponse-action-icon

```sql
-- Tabela de categorias
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    parent_id INT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Self-reference para categorias hierárquicas
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE SET NULL
);

-- Tabela de produtos
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    category_id INT NOT NULL,
    
    -- Preços
    cost_price DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    selling_price DECIMAL(10,2) NOT NULL,
    
    -- Inventário
    stock_quantity INT NOT NULL DEFAULT 0,
    min_stock_level INT DEFAULT 10,
    
    -- Status
    status ENUM('active', 'inactive', 'discontinued') DEFAULT 'active',
    is_featured BOOLEAN DEFAULT FALSE,
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Constraints
    FOREIGN KEY (category_id) REFERENCES categories(id),
    CHECK (selling_price > 0),
    CHECK (stock_quantity >= 0),
    CHECK (cost_price >= 0),
    
    -- Índices
    INDEX idx_products_category (category_id),
    INDEX idx_products_status (status),
    INDEX idx_products_featured (is_featured)
);

-- Tabela de clientes
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(200) NOT NULL UNIQUE,
    phone VARCHAR(20),
    
    -- Endereço
    street_address TEXT,
    city VARCHAR(100),
    postal_code VARCHAR(20),
    country VARCHAR(100) DEFAULT 'Portugal',
    
    -- Account info
    date_of_birth DATE,
    account_status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Full-text search no nome
    FULLTEXT(first_name, last_name),
    
    -- Índices
    INDEX idx_customers_email (email),
    INDEX idx_customers_status (account_status),
    INDEX idx_customers_country (country)
);
```

#### **Sistema Escolar:**

sqlresponse-action-icon

```sql
-- Tabela de cursos
CREATE TABLE courses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    code VARCHAR(10) NOT NULL UNIQUE,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    credits INT NOT NULL DEFAULT 3,
    max_students INT DEFAULT 30,
    
    -- Datas
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    CHECK (credits > 0),
    CHECK (max_students > 0),
    CHECK (end_date > start_date)
);

-- Tabela de estudantes
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_number VARCHAR(20) NOT NULL UNIQUE,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(200) NOT NULL UNIQUE,
    date_of_birth DATE NOT NULL,
    
    -- Academic info
    enrollment_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    graduation_date DATE NULL,
    gpa DECIMAL(3,2) DEFAULT 0.00,
    
    -- Status
    status ENUM('enrolled', 'graduated', 'suspended', 'dropped') DEFAULT 'enrolled',
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    CHECK (gpa >= 0.00 AND gpa <= 4.00),
    CHECK (graduation_date IS NULL OR graduation_date > enrollment_date)
);

-- Tabela de inscrições (Many-to-Many)
CREATE TABLE enrollments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrollment_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    final_grade DECIMAL(5,2) NULL,
    status ENUM('enrolled', 'completed', 'dropped', 'failed') DEFAULT 'enrolled',
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Foreign keys
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE,
    
    -- Constraints
    UNIQUE KEY unique_student_course (student_id, course_id),
    CHECK (final_grade IS NULL OR (final_grade >= 0 AND final_grade <= 100)),
    
    -- Índices
    INDEX idx_enrollments_student (student_id),
    INDEX idx_enrollments_course (course_id),
    INDEX idx_enrollments_status (status)
);
```

### **📋 CREATE TABLE Avançado:**

#### **Tabelas Temporárias:**

sqlresponse-action-icon

```sql
-- Tabela temporária (sessão)
CREATE TEMPORARY TABLE temp_calculations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    value DECIMAL(10,2),
    calculation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Usar a tabela temporária
INSERT INTO temp_calculations (value) VALUES (100.50), (200.75);
SELECT * FROM temp_calculations;
-- Tabela é automaticamente apagada no fim da sessão
```

#### **CREATE TABLE AS (CTAS):**

sqlresponse-action-icon

```sql
-- Criar tabela a partir de query
CREATE TABLE high_value_sales AS
SELECT 
    salesperson,
    product,
    quantity,
    price,
    quantity * price AS total_value,
    sale_date
FROM sales
WHERE quantity * price > 1000;

-- Com estrutura mas sem dados
CREATE TABLE empty_sales_copy AS
SELECT * FROM sales WHERE 1=0;  -- Condição sempre falsa

-- MySQL específico - LIKE
CREATE TABLE sales_backup LIKE sales;  -- Só estrutura
```

#### **Particionamento (MySQL/PostgreSQL):**

sqlresponse-action-icon

```sql
-- Particionamento por range de data
CREATE TABLE sales_partitioned (
    id INT AUTO_INCREMENT,
    product VARCHAR(100),
    sale_date DATE NOT NULL,
    amount DECIMAL(10,2),
    PRIMARY KEY (id, sale_date)  -- Chave deve incluir coluna de partição
)
PARTITION BY RANGE (YEAR(sale_date)) (
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - Básico:**

sqlresponse-action-icon

```sql
-- 1. Criar tabela de funcionários
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    phone VARCHAR(20),
    hire_date DATE NOT NULL,
    salary DECIMAL(10,2) CHECK (salary > 0),
    department VARCHAR(100) DEFAULT 'General'
);

-- 2. Criar tabela de departamentos
CREATE TABLE departments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    budget DECIMAL(12,2) DEFAULT 0,
    manager_id INT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 3. Adicionar foreign key aos funcionários
ALTER TABLE employees 
ADD COLUMN department_id INT,
ADD FOREIGN KEY (department_id) REFERENCES departments(id);
```

#### **Exercício 2 - Intermediário:**

sqlresponse-action-icon

```sql
-- 1. Sistema de biblioteca
CREATE TABLE authors (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    birth_date DATE,
    nationality VARCHAR(100),
    biography TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    isbn VARCHAR(20) UNIQUE NOT NULL,
    title VARCHAR(300) NOT NULL,
    author_id INT NOT NULL,
    publication_year YEAR,
    pages INT CHECK (pages > 0),
    genre ENUM('Fiction', 'Non-Fiction', 'Science', 'History', 'Biography') NOT NULL,
    available_copies INT DEFAULT 1,
    total_copies INT DEFAULT 1,
    
    FOREIGN KEY (author_id) REFERENCES authors(id),
    CHECK (available_copies <= total_copies),
    CHECK (available_copies >= 0)
);

CREATE TABLE members (
    id INT AUTO_INCREMENT PRIMARY KEY,
    member_number VARCHAR(20) UNIQUE NOT NULL,
    name VARCHAR(200) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    join_date DATE DEFAULT (CURRENT_DATE),
    status ENUM('active', 'suspended', 'expired') DEFAULT 'active',
    max_books_allowed INT DEFAULT 5
);

CREATE TABLE loans (
    id INT AUTO_INCREMENT PRIMARY KEY,
    member_id INT NOT NULL,
    book_id INT NOT NULL,
    loan_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    due_date DATE NOT NULL,
    return_date DATE NULL,
    fine_amount DECIMAL(6,2) DEFAULT 0.00,
    
    FOREIGN KEY (member_id) REFERENCES members(id),
    FOREIGN KEY (book_id) REFERENCES books(id),
    CHECK (due_date > loan_date),
    CHECK (return_date IS NULL OR return_date >= loan_date),
    CHECK (fine_amount >= 0)
);
```

### **💡 Boas Práticas:**

#### **1. Convenções de Nomeação:**

sqlresponse-action-icon

```sql
-- ✅ Boas práticas
CREATE TABLE customer_orders (           -- snake_case
    id INT AUTO_INCREMENT PRIMARY KEY,   -- id sempre como PK
    customer_id INT NOT NULL,            -- _id para foreign keys
    order_date DATE NOT NULL,            -- nomes descritivos
    total_amount DECIMAL(10,2),          -- tipo apropriado
    
    -- Constraints nomeadas
    CONSTRAINT fk_orders_customer FOREIGN KEY (customer_id) REFERENCES customers(id),
    CONSTRAINT ck_orders_amount CHECK (total_amount >= 0),
    
    -- Índices nomeados
    INDEX idx_orders_customer (customer_id),
    INDEX idx_orders_date (order_date)
);
```

#### **2. Performance e Manutenibilidade:**

sqlresponse-action-icon

```sql
-- Incluir timestamps para auditoria
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,

-- Usar ENUM para valores limitados  
status ENUM('active', 'inactive', 'pending') NOT NULL DEFAULT 'active',

-- Índices importantes  
INDEX idx_table_status (status),  
INDEX idx_table_created (created_at),

-- Comentários para documentação  
COMMENT = 'Tabela de pedidos de clientes - criada em 2024-01-15'  
);

textresponse-action-icon

````text

#### **3. Segurança e Integridade:**
```sql
-- Sempre usar NOT NULL onde apropriado
email VARCHAR(200) NOT NULL,

-- Validações com CHECK
CHECK (email LIKE '%@%'),
CHECK (age BETWEEN 0 AND 150),

-- Default values seguros
created_by INT NOT NULL DEFAULT 1,  -- Usuário do sistema
is_deleted BOOLEAN DEFAULT FALSE    -- Soft delete
````

### **🚨 Erros Comuns:**

#### **1. Tipos de Dados Inadequados:**

sqlresponse-action-icon

```sql
-- ❌ Problemas comuns
CREATE TABLE bad_design (
    id VARCHAR(50),                    -- Deveria ser INT AUTO_INCREMENT
    price FLOAT,                       -- Deveria ser DECIMAL para dinheiro
    is_active TINYINT,                 -- Deveria ser BOOLEAN
    big_text VARCHAR(1000000),         -- Deveria ser TEXT
    date_field VARCHAR(20)             -- Deveria ser DATE ou DATETIME
);

-- ✅ Correções
CREATE TABLE good_design (
    id INT AUTO_INCREMENT PRIMARY KEY,
    price DECIMAL(10,2) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    description TEXT,
    created_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

#### **2. Constraints Esquecidas:**

sqlresponse-action-icon

```sql
-- ❌ Sem constraints adequadas
CREATE TABLE orders_bad (
    id INT,
    customer_id INT,
    total DECIMAL(10,2)
);

-- ✅ Com constraints apropriadas
CREATE TABLE orders_good (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    total DECIMAL(10,2) NOT NULL CHECK (total >= 0),
    
    FOREIGN KEY (customer_id) REFERENCES customers(id),
    INDEX idx_orders_customer (customer_id)
);
```



