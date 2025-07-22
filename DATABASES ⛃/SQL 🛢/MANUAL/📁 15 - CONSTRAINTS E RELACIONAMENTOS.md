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
```