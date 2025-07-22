### **🔐 O que são Constraints?**

#### **Definição:**

> Constraints são regras aplicadas às colunas de uma tabela para garantir a integridade e consistência dos dados. Elas impedem a inserção de dados inválidos.

#### **Tipos de Constraints:**

textresponse-action-icon

```text
🔑 PRIMARY KEY - Identifica unicamente cada linha
🔗 FOREIGN KEY - Mantém integridade referencial  
✨ UNIQUE - Garante valores únicos
❌ NOT NULL - Impede valores nulos
✅ CHECK - Valida condições específicas
🎯 DEFAULT - Define valores padrão
```

### **🔑 PRIMARY KEY:**

#### **Conceito e Características:**

sqlresponse-action-icon

```sql
-- Características da Primary Key:
-- 1. Valores únicos (não pode repetir)
-- 2. Não pode ser NULL
-- 3. Só uma PK por tabela (mas pode ser composta)
-- 4. Automaticamente cria índice único
-- 5. Referenciada por Foreign Keys

-- Primary Key simples
CREATE TABLE customers (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- Primary Key nomeada
CREATE TABLE customers (
    id INT,
    name VARCHAR(100),
    CONSTRAINT pk_customers PRIMARY KEY (id)
);

-- Primary Key composta
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

#### **AUTO_INCREMENT com Primary Key:**

sqlresponse-action-icon

```sql
-- MySQL
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);

-- SQL Server
CREATE TABLE customers (
    id INT IDENTITY(1,1) PRIMARY KEY,
    name VARCHAR(100)
);

-- PostgreSQL
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- Controlar AUTO_INCREMENT
ALTER TABLE customers AUTO_INCREMENT = 1000;  -- Próximo id será 1000
```

#### **Primary Keys Naturais vs Substitutas:**

sqlresponse-action-icon

```sql
-- Primary Key Natural (dados do negócio)
CREATE TABLE countries (
    iso_code CHAR(2) PRIMARY KEY,  -- 'PT', 'ES', 'FR'
    name VARCHAR(100) NOT NULL
);

-- Primary Key Substituta (artificial)
CREATE TABLE countries (
    id INT AUTO_INCREMENT PRIMARY KEY,  -- Melhor para performance
    iso_code CHAR(2) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL
);

-- Quando usar cada uma:
-- Natural: Poucos registos, código não muda nunca
-- Substituta: Muitos registos, melhor performance, mais flexível
```

### **🔗 FOREIGN KEY:**

#### **Conceito e Integridade Referencial:**

sqlresponse-action-icon

```sql
-- Foreign Key garante que valor existe na tabela pai
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE,
    
    -- Foreign Key constraint
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Constraint nomeada (melhor prática)
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE,
    
    CONSTRAINT fk_orders_customer 
        FOREIGN KEY (customer_id) 
        REFERENCES customers(id)
);
```

#### **Ações Referenciais:**

sqlresponse-action-icon

```sql
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    
    CONSTRAINT fk_orders_customer 
        FOREIGN KEY (customer_id) 
        REFERENCES customers(id)
        ON DELETE CASCADE      -- Se customer for apagado, apaga orders também
        ON UPDATE CASCADE      -- Se customer.id mudar, atualiza customer_id
);

-- Opções de ação:
-- CASCADE    - Propaga a mudança
-- RESTRICT   - Impede a operação se há dependentes  
-- SET NULL   - Define como NULL
-- SET DEFAULT - Define valor padrão
-- NO ACTION  - Igual a RESTRICT (padrão)
```

#### **Exemplos de Ações Referenciais:**

sqlresponse-action-icon

```sql
-- Exemplo 1: CASCADE - Propagação automática
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    category_id INT,
    
    FOREIGN KEY (category_id) REFERENCES categories(id)
    ON DELETE CASCADE ON UPDATE CASCADE
);

-- Se apagar categoria, apaga todos produtos dessa categoria
DELETE FROM categories WHERE id = 1;

-- Exemplo 2: SET NULL - Preservar dados
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    
    FOREIGN KEY (department_id) REFERENCES departments(id)
    ON DELETE SET NULL  -- Se departamento for apagado, employee fica sem departamento
);

-- Exemplo 3: RESTRICT - Proteger dados críticos
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    
    FOREIGN KEY (customer_id) REFERENCES customers(id)
    ON DELETE RESTRICT  -- Não permite apagar customer se tem orders
);
```

#### **Foreign Keys Compostas:**

sqlresponse-action-icon

```sql
-- Quando PK da tabela pai é composta
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    line_number INT,
    quantity INT,
    
    PRIMARY KEY (order_id, product_id),
    
    FOREIGN KEY (order_id, product_id) 
    REFERENCES order_products(order_id, product_id)
);
```

### **✨ UNIQUE:**

#### **Garantir Valores Únicos:**

sqlresponse-action-icon

```sql
-- UNIQUE em coluna individual
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(200) UNIQUE NOT NULL,
    name VARCHAR(100)
);

-- UNIQUE nomeada
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(200) NOT NULL,
    name VARCHAR(100),
    
    CONSTRAINT uk_customers_email UNIQUE (email)
);

-- UNIQUE composta
CREATE TABLE user_roles (
    user_id INT,
    role_id INT,
    granted_date DATE,
    
    UNIQUE (user_id, role_id)  -- Combinação deve ser única
);

-- Múltiplas constraints UNIQUE
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employee_number VARCHAR(20) UNIQUE,
    email VARCHAR(200) UNIQUE,
    social_security VARCHAR(20) UNIQUE,
    name VARCHAR(100)
);
```

#### **UNIQUE vs PRIMARY KEY:**

sqlresponse-action-icon

```sql
-- Diferenças:
-- PRIMARY KEY: 1 por tabela, NOT NULL obrigatório, cria cluster index
-- UNIQUE: várias por tabela, permite NULL, cria non-cluster index

CREATE TABLE example (
    id INT PRIMARY KEY,           -- Só uma PK
    email VARCHAR(200) UNIQUE,    -- Múltiplas UNIQUE OK
    username VARCHAR(50) UNIQUE,  -- Múltiplas UNIQUE OK
    phone VARCHAR(20) UNIQUE      -- Múltiplas UNIQUE OK
);
```

### **❌ NOT NULL:**

#### **Impedir Valores Nulos:**

sqlresponse-action-icon

```sql
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,    -- Obrigatório
    last_name VARCHAR(50) NOT NULL,     -- Obrigatório
    email VARCHAR(200) NOT NULL,        -- Obrigatório
    phone VARCHAR(20),                  -- Opcional
    birth_date DATE                     -- Opcional
);

-- NOT NULL com DEFAULT
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    status VARCHAR(20) NOT NULL DEFAULT 'active',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### **✅ CHECK Constraints:**

#### **Validações Personalizadas:**

sqlresponse-action-icon

```sql
-- CHECK simples
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) CHECK (price > 0),
    stock INT CHECK (stock >= 0)
);

-- CHECK nomeada
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    salary DECIMAL(10,2),
    
    CONSTRAINT ck_employees_age CHECK (age >= 16 AND age <= 70),
    CONSTRAINT ck_employees_salary CHECK (salary > 0)
);

-- CHECK com múltiplas colunas
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    start_date DATE,
    end_date DATE,
    
    CONSTRAINT ck_orders_dates CHECK (end_date >= start_date)
);

-- CHECK com expressões complexas
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(200),
    phone VARCHAR(20),
    
    CONSTRAINT ck_customers_contact 
    CHECK (email IS NOT NULL OR phone IS NOT NULL),  -- Pelo menos um
    
    CONSTRAINT ck_customers_email 
    CHECK (email LIKE '%@%.%')  -- Formato básico email
);
```

#### **CHECK com ENUM Alternativo:**

sqlresponse-action-icon

```sql
-- Em vez de ENUM, usar CHECK
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    
    CONSTRAINT ck_orders_status 
    CHECK (status IN ('pending', 'confirmed', 'shipped', 'delivered', 'cancelled'))
);

-- Vantagem: Mais fácil adicionar novos valores
ALTER TABLE orders 
DROP CONSTRAINT ck_orders_status;

ALTER TABLE orders 
ADD CONSTRAINT ck_orders_status 
CHECK (status IN ('pending', 'confirmed', 'processing', 'shipped', 'delivered', 'cancelled'));
```

### **🎯 DEFAULT Values:**

#### **Valores Padrão:**

sqlresponse-action-icon

```sql
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    status VARCHAR(20) DEFAULT 'active',
    country VARCHAR(50) DEFAULT 'Portugal',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    is_newsletter BOOLEAN DEFAULT TRUE,
    credit_limit DECIMAL(10,2) DEFAULT 1000.00
);

-- DEFAULT com expressões
CREATE TABLE logs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT,
    log_date DATE DEFAULT (CURRENT_DATE),
    log_time TIME DEFAULT (CURRENT_TIME),
    severity_level INT DEFAULT 1
);
```

### **🏗️ Relacionamentos Complexos:**

#### **Relacionamento 1:1:**

sqlresponse-action-icon

```sql
-- Cada user tem um profile único
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL
);

CREATE TABLE user_profiles (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNIQUE NOT NULL,  -- UNIQUE garante 1:1
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    bio TEXT,
    
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### **Relacionamento 1:N:**

sqlresponse-action-icon

```sql
-- Um customer tem muitas orders
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,  -- Muitas orders para um customer
    order_date DATE,
    
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

#### **Relacionamento M:N com Tabela Associativa:**

sqlresponse-action-icon

```sql
-- Students podem ter muitos courses, courses podem ter muitos students
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE courses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    credits INT
);

-- Tabela associativa
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    enrollment_date DATE DEFAULT (CURRENT_DATE),
    grade DECIMAL(4,2),
    
    PRIMARY KEY (student_id, course_id),  -- Chave composta
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE,
    
    -- Constraints adicionais
    CHECK (grade IS NULL OR (grade >= 0 AND grade <= 20))
);
```

#### **Self-Referencing (Hierárquicas):**

sqlresponse-action-icon

```sql
-- Employees podem ter managers (que também são employees)
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    manager_id INT,  -- Reference to another employee
    department VARCHAR(50),
    
    FOREIGN KEY (manager_id) REFERENCES employees(id) ON DELETE SET NULL
);

-- Categorias podem ter categorias pai
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    parent_id INT,
    level INT DEFAULT 0,
    
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE CASCADE,
    CHECK (level >= 0 AND level <= 5)  -- Limitar profundidade
);
```

### **🎯 Exercícios Práticos:**

#### **Exercício 1 - E-commerce Completo:**

sqlresponse-action-icon

```sql
-- Criar sistema completo com todas as constraints

-- 1. Categorias hierárquicas
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    parent_id INT,
    is_active BOOLEAN DEFAULT TRUE,
    
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE SET NULL
);

-- 2. Fornecedores
CREATE TABLE suppliers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    email VARCHAR(200) UNIQUE,
    phone VARCHAR(20),
    country VARCHAR(100) DEFAULT 'Portugal',
    
    CONSTRAINT ck_suppliers_contact CHECK (email IS NOT NULL OR phone IS NOT NULL)
);

-- 3. Produtos
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    sku VARCHAR(50) UNIQUE NOT NULL,
    name VARCHAR(200) NOT NULL,
    category_id INT NOT NULL,
    supplier_id INT,
    
    -- Pricing
    cost_price DECIMAL(10,2) NOT NULL DEFAULT 0,
    selling_price DECIMAL(10,2) NOT NULL,
    
    -- Inventory
    stock_quantity INT NOT NULL DEFAULT 0,
    min_stock_level INT DEFAULT 10,
    
    -- Status
    status ENUM('active', 'inactive', 'discontinued') DEFAULT 'active',
    
    -- Audit
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Constraints
    FOREIGN KEY (category_id) REFERENCES categories(id),
    FOREIGN KEY (supplier_id) REFERENCES suppliers(id) ON DELETE SET NULL,
    
    CONSTRAINT ck_products_prices CHECK (selling_price > cost_price),
    CONSTRAINT ck_products_stock CHECK (stock_quantity >= 0),
    CONSTRAINT ck_products_min_stock CHECK (min_stock_level >= 0),
    
    -- Indexes
    INDEX idx_products_category (category_id),
    INDEX idx_products_supplier (supplier_id),
    INDEX idx_products_status (status)
);

-- 4. Clientes
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_number VARCHAR(20) UNIQUE NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    phone VARCHAR(20),
    birth_date DATE,
    
    -- Address
    street_address TEXT,
    city VARCHAR(100),
    postal_code VARCHAR(20),
    country VARCHAR(100) DEFAULT 'Portugal',
    
    -- Account
    status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
    credit_limit DECIMAL(10,2) DEFAULT 1000.00,
    
    -- Stats
    total_orders INT DEFAULT 0,
    total_spent DECIMAL(12,2) DEFAULT 0,
    loyalty_points INT DEFAULT 0,
    
    -- Audit
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Constraints
    CONSTRAINT ck_customers_age CHECK (birth_date IS NULL OR birth_date < CURDATE()),
    CONSTRAINT ck_customers_credit CHECK (credit_limit >= 0),
    CONSTRAINT ck_customers_stats CHECK (total_orders >= 0 AND total_spent >= 0 AND loyalty_points >= 0),
    
    -- Index
    INDEX idx_customers_email (email),
    INDEX idx_customers_status (status)
);

-- 5. Pedidos
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_number VARCHAR(50) UNIQUE NOT NULL,
    customer_id INT NOT NULL,
    
    
    
-- Dates  
order_date DATE NOT NULL DEFAULT (CURRENT_DATE),  
required_date DATE,  
shipped_date DATE,

-- Status
status ENUM('pending', 'confirmed', 'processing', 'shipped', 'delivered', 'cancelled') DEFAULT 'pending',

-- Financial
subtotal DECIMAL(10,2) NOT NULL DEFAULT 0,
tax_amount DECIMAL(10,2) NOT NULL DEFAULT 0,
shipping_cost DECIMAL(10,2) NOT NULL DEFAULT 0,
total_amount DECIMAL(10,2) NOT NULL DEFAULT 0,

-- Shipping
shipping_address TEXT,
tracking_number VARCHAR(100),

-- Notes
customer_notes TEXT,
internal_notes TEXT,

-- Audit
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,

-- Constraints
FOREIGN KEY (customer_id) REFERENCES customers(id),

CONSTRAINT ck_orders_dates CHECK (
    (required_date IS NULL OR required_date >= order_date) AND
    (shipped_date IS NULL OR shipped_date >= order_date)
),
CONSTRAINT ck_orders_amounts CHECK (
    subtotal >= 0 AND tax_amount >= 0 AND 
    shipping_cost >= 0 AND total_amount >= 0
),
CONSTRAINT ck_orders_total CHECK (
    total_amount = subtotal + tax_amount + shipping_cost
),

-- Indexes
INDEX idx_orders_customer (customer_id),
INDEX idx_orders_status (status),
INDEX idx_orders_date (order_date)
);

-- 6. Itens do pedido  
CREATE TABLE order_items (  
id INT AUTO_INCREMENT PRIMARY KEY,  
order_id INT NOT NULL,  
product_id INT NOT NULL,


quantity INT NOT NULL,
unit_price DECIMAL(10,2) NOT NULL,
discount_percent DECIMAL(5,2) DEFAULT 0,
line_total DECIMAL(10,2) NOT NULL,

-- Constraints
FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
FOREIGN KEY (product_id) REFERENCES products(id),

CONSTRAINT ck_order_items_quantity CHECK (quantity > 0),
CONSTRAINT ck_order_items_price CHECK (unit_price > 0),
CONSTRAINT ck_order_items_discount CHECK (discount_percent >= 0 AND discount_percent <= 100),
CONSTRAINT ck_order_items_total CHECK (
    line_total = quantity * unit_price * (1 - discount_percent/100)
),

-- Unique constraint
UNIQUE KEY uk_order_product (order_id, product_id),

-- Indexes
INDEX idx_order_items_order (order_id),
INDEX idx_order_items_product (product_id)
);

```


#### **Exercício 2 - Sistema Escolar Avançado:**

```sql
-- Sistema escolar com constraints complexas

-- 1. Professores
CREATE TABLE teachers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employee_number VARCHAR(20) UNIQUE NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    phone VARCHAR(20),
    hire_date DATE NOT NULL,
    salary DECIMAL(10,2) NOT NULL,
    department VARCHAR(100),
    
    CONSTRAINT ck_teachers_hire_date CHECK (hire_date <= CURDATE()),
    CONSTRAINT ck_teachers_salary CHECK (salary > 0),
    
    INDEX idx_teachers_department (department)
);

-- 2. Cursos
CREATE TABLE courses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    course_code VARCHAR(10) UNIQUE NOT NULL,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    credits INT NOT NULL,
    max_students INT DEFAULT 30,
    teacher_id INT,
    
    -- Prerequisites (self-reference)
    prerequisite_course_id INT,
    
    -- Semester info
    semester ENUM('Fall', 'Spring', 'Summer') NOT NULL,
    year_offered YEAR NOT NULL,
    
    FOREIGN KEY (teacher_id) REFERENCES teachers(id) ON DELETE SET NULL,
    FOREIGN KEY (prerequisite_course_id) REFERENCES courses(id) ON DELETE SET NULL,
    
    CONSTRAINT ck_courses_credits CHECK (credits BETWEEN 1 AND 10),
    CONSTRAINT ck_courses_max_students CHECK (max_students BETWEEN 5 AND 200),
    
    INDEX idx_courses_teacher (teacher_id),
    INDEX idx_courses_semester_year (semester, year_offered)
);

-- 3. Estudantes
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_number VARCHAR(20) UNIQUE NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    birth_date DATE NOT NULL,
    enrollment_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    
    -- Academic info
    major VARCHAR(100),
    gpa DECIMAL(3,2) DEFAULT 0.00,
    credits_completed INT DEFAULT 0,
    
    -- Status
    status ENUM('enrolled', 'suspended', 'graduated', 'dropped') DEFAULT 'enrolled',
    graduation_date DATE,
    
    CONSTRAINT ck_students_birth_date CHECK (
        birth_date < DATE_SUB(CURDATE(), INTERVAL 16 YEAR)
    ),
    CONSTRAINT ck_students_gpa CHECK (gpa BETWEEN 0.00 AND 4.00),
    CONSTRAINT ck_students_credits CHECK (credits_completed >= 0),
    CONSTRAINT ck_students_graduation CHECK (
        (status = 'graduated' AND graduation_date IS NOT NULL) OR
        (status != 'graduated' AND graduation_date IS NULL)
    ),
    
    INDEX idx_students_status (status),
    INDEX idx_students_major (major)
);

-- 4. Inscrições (M:N relationship)
CREATE TABLE enrollments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    
    enrollment_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    final_grade DECIMAL(5,2),
    letter_grade ENUM('A+', 'A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D', 'F'),
    status ENUM('enrolled', 'completed', 'dropped', 'failed') DEFAULT 'enrolled',
    
    -- Attendance tracking
    classes_attended INT DEFAULT 0,
    total_classes INT DEFAULT 0,
    
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE,
    
    CONSTRAINT ck_enrollments_grade CHECK (
        final_grade IS NULL OR (final_grade BETWEEN 0 AND 100)
    ),
    CONSTRAINT ck_enrollments_attendance CHECK (
        classes_attended <= total_classes AND
        classes_attended >= 0 AND total_classes >= 0
    ),
    
    -- Business rule: Student can't enroll in same course twice unless failed
    UNIQUE KEY uk_student_course_active (student_id, course_id, status),
    
    INDEX idx_enrollments_student (student_id),
    INDEX idx_enrollments_course (course_id),
    INDEX idx_enrollments_status (status)
);

-- 5. Trigger para atualizar GPA automaticamente
DELIMITER //
CREATE TRIGGER update_student_gpa
AFTER UPDATE ON enrollments
FOR EACH ROW
BEGIN
    IF NEW.status = 'completed' AND NEW.final_grade IS NOT NULL THEN
        UPDATE students 
        SET gpa = (
            SELECT AVG(
                CASE 
                    WHEN final_grade >= 97 THEN 4.0
                    WHEN final_grade >= 93 THEN 4.0
                    WHEN final_grade >= 90 THEN 3.7
                    WHEN final_grade >= 87 THEN 3.3
                    WHEN final_grade >= 83 THEN 3.0
                    WHEN final_grade >= 80 THEN 2.7
                    WHEN final_grade >= 77 THEN 2.3
                    WHEN final_grade >= 73 THEN 2.0
                    WHEN final_grade >= 70 THEN 1.7
                    WHEN final_grade >= 67 THEN 1.3
                    WHEN final_grade >= 65 THEN 1.0
                    ELSE 0.0
                END
            )
            FROM enrollments 
            WHERE student_id = NEW.student_id 
            AND status = 'completed' 
            AND final_grade IS NOT NULL
        ),
        credits_completed = (
            SELECT SUM(c.credits)
            FROM enrollments e
            JOIN courses c ON e.course_id = c.id
            WHERE e.student_id = NEW.student_id 
            AND e.status = 'completed'
        )
        WHERE id = NEW.student_id;
    END IF;
END //
DELIMITER ;
````

### **🔍 Verificar e Gerenciar Constraints:**

#### **Visualizar Constraints Existentes:**

sqlresponse-action-icon

```sql
-- MySQL - Ver todas as constraints
SELECT 
    TABLE_NAME,
    COLUMN_NAME,
    CONSTRAINT_NAME,
    CONSTRAINT_TYPE
FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS tc
JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE kcu 
    ON tc.CONSTRAINT_NAME = kcu.CONSTRAINT_NAME
WHERE tc.TABLE_SCHEMA = 'your_database_name'
ORDER BY TABLE_NAME, CONSTRAINT_TYPE;

-- Ver Foreign Keys específicas
SELECT 
    kcu.TABLE_NAME,
    kcu.COLUMN_NAME,
    kcu.CONSTRAINT_NAME,
    kcu.REFERENCED_TABLE_NAME,
    kcu.REFERENCED_COLUMN_NAME,
    rc.UPDATE_RULE,
    rc.DELETE_RULE
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE kcu
JOIN INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS rc
    ON kcu.CONSTRAINT_NAME = rc.CONSTRAINT_NAME
WHERE kcu.TABLE_SCHEMA = 'your_database_name'
ORDER BY kcu.TABLE_NAME;

-- Ver Check Constraints (MySQL 8.0+)
SELECT 
    CONSTRAINT_SCHEMA,
    TABLE_NAME,
    CONSTRAINT_NAME,
    CHECK_CLAUSE
FROM INFORMATION_SCHEMA.CHECK_CONSTRAINTS
WHERE CONSTRAINT_SCHEMA = 'your_database_name';
```

#### **Gerenciar Constraints Existentes:**

```sql
-- Adicionar constraint a tabela existente
ALTER TABLE products 
ADD CONSTRAINT ck_products_price CHECK (price > 0);

-- Remover constraint
ALTER TABLE products 
DROP CONSTRAINT ck_products_price;

-- Modificar Foreign Key
ALTER TABLE orders 
DROP FOREIGN KEY fk_orders_customer;

ALTER TABLE orders 
ADD CONSTRAINT fk_orders_customer 
FOREIGN KEY (customer_id) REFERENCES customers(id)
ON DELETE RESTRICT ON UPDATE CASCADE;

-- Desabilitar temporariamente checks (MySQL)
SET FOREIGN_KEY_CHECKS = 0;
-- ... operações que podem violar FKs
SET FOREIGN_KEY_CHECKS = 1;

-- Verificar integridade depois
CHECK TABLE orders;
```

### **⚡ Performance e Constraints:**

#### **Impacto na Performance:**

```sql
-- Constraints que criam índices automaticamente:
-- PRIMARY KEY - Cria índice clustered
-- UNIQUE - Cria índice único
-- FOREIGN KEY - Cria índice na coluna referenciada

-- Ver índices criados automaticamente
SHOW INDEXES FROM orders;

-- Índices adicionais podem ser necessários
CREATE INDEX idx_orders_status_date ON orders(status, order_date);

-- Para consultas como:
SELECT * FROM orders 
WHERE status = 'pending' 
AND order_date >= '2024-01-01';
```

### **🚨 Troubleshooting Constraints:**

#### **Erros Comuns e Soluções:**

```sql
-- Erro: Cannot add foreign key constraint
-- Causa: Dados órfãos na tabela filha

-- 1. Identificar dados problemáticos
SELECT DISTINCT o.customer_id 
FROM orders o 
LEFT JOIN customers c ON o.customer_id = c.id 
WHERE c.id IS NULL;

-- 2. Corrigir dados ou remover órfãos
DELETE FROM orders WHERE customer_id NOT IN (SELECT id FROM customers);

-- 3. Adicionar constraint
ALTER TABLE orders 
ADD FOREIGN KEY (customer_id) REFERENCES customers(id);

-- Erro: Check constraint violation
-- Causa: Dados existentes violam nova regra

-- 1. Identificar dados problemáticos
SELECT * FROM products WHERE price <= 0;

-- 2. Corrigir dados
UPDATE products SET price = 1.00 WHERE price <= 0;

-- 3. Adicionar constraint
ALTER TABLE products 
ADD CONSTRAINT ck_products_price CHECK (price > 0);
```