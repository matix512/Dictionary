
### **📐 Princípios do Modelo Relacional:**

#### **Criado por Edgar F. Codd (1970):**

> O modelo relacional organiza dados em relações (tabelas), onde cada relação é um conjunto de tuplas (linhas) com atributos (colunas) bem definidos.

#### **Regras Fundamentais:**

```text
1. 📋 Informação está em tabelas
2. 🔑 Cada linha é única (chave primária)
3. 🎯 Cada célula contém um valor atómico
4. 🔗 Relacionamentos através de chaves
5. 📏 Ordem de linhas/colunas irrelevante
6. ✅ Integridade referencial mantida
```

### **🗝️ Tipos de Chaves:**

#### **Chave Primária (Primary Key):**

```sql
-- Identifica unicamente cada linha
CREATE TABLE users (
    id INT PRIMARY KEY,        -- Chave simples
    email VARCHAR(100) UNIQUE,
    name VARCHAR(50)
);

-- Chave composta
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)  -- Chave composta
);
```

#### **Chave Estrangeira (Foreign Key):**

```sql
-- Liga duas tabelas
CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

#### **Chave Candidata e Alternativa:**

```sql
-- Múltiplas chaves possíveis
CREATE TABLE users (
    id INT PRIMARY KEY,           -- Chave primária escolhida
    email VARCHAR(100) UNIQUE,    -- Chave alternativa
    ssn VARCHAR(20) UNIQUE       -- Outra chave candidata
);
```

### **🔗 Tipos de Relacionamentos:**

#### **1:1 (Um para Um):**

```text
👤 USER ←→ 📄 PROFILE
Cada user tem um profile único

users                profiles
┌────┬──────┐       ┌────┬─────────┬─────────┐
│ id │ name │  ←──  │ id │ user_id │ bio     │
├────┼──────┤       ├────┼─────────┼─────────┤
│ 1  │ João │       │ 1  │ 1       │ Bio...  │
│ 2  │ Ana  │       │ 2  │ 2       │ Bio...  │
└────┴──────┘       └────┴─────────┴─────────┘
```

#### **1:N (Um para Muitos):**

```text
🏢 COMPANY ←──→ 👥 EMPLOYEES
Uma empresa tem muitos funcionários

companies            employees
┌────┬───────┐      ┌────┬──────┬────────────┐
│ id │ name  │  ←── │ id │ name │ company_id │
├────┼───────┤      ├────┼──────┼────────────┤
│ 1  │ ACME  │      │ 1  │ João │ 1          │
│ 2  │ XYZ   │      │ 2  │ Ana  │ 1          │
└────┴───────┘      │ 3  │ Luis │ 2          │
                    └────┴──────┴────────────┘
```

#### **M:N (Muitos para Muitos):**


```text
📚 STUDENTS ←──→ 📖 COURSES
Estudantes fazem vários cursos

students             enrollments         courses
┌────┬──────┐      ┌───────────┬─────────┐   ┌────┬──────────┐
│ id │ name │  ←─  │ student_id│course_id│ ─→│ id │ name     │
├────┼──────┤      ├───────────┼─────────┤   ├────┼──────────┤
│ 1  │ Ana  │      │ 1         │ 1       │   │ 1  │ Math     │
│ 2  │ João │      │ 1         │ 2       │   │ 2  │ Physics  │
└────┴──────┘      │ 2         │ 1       │   │ 3  │ Chemistry│
                   └───────────┴─────────┘   └────┴──────────┘
```

### **📊 Normalização:**

#### **Primeira Forma Normal (1NF):**


```sql
-- ❌ Violação 1NF
CREATE TABLE students_bad (
    id INT,
    name VARCHAR(50),
    courses VARCHAR(200)  -- "Math, Physics, Chemistry"
);

-- ✅ 1NF Corrigida
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE student_courses (
    student_id INT,
    course VARCHAR(50),
    FOREIGN KEY (student_id) REFERENCES students(id)
);
```

#### **Segunda Forma Normal (2NF):**


```sql
-- ❌ Violação 2NF
CREATE TABLE order_items_bad (
    order_id INT,
    product_id INT,
    product_name VARCHAR(50),    -- Depende só de product_id
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);

-- ✅ 2NF Corrigida
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

#### **Terceira Forma Normal (3NF):**


```sql
-- ❌ Violação 3NF
CREATE TABLE employees_bad (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    department_name VARCHAR(50),  -- Depende de department_id
    department_location VARCHAR(50)
);

-- ✅ 3NF Corrigida
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    location VARCHAR(50)
);
```

### **🎯 Integridade Referencial:**

#### **Regras de Integridade:**

```sql
-- Cascata: apagar pai apaga filhos
ALTER TABLE orders 
ADD CONSTRAINT fk_customer 
FOREIGN KEY (customer_id) REFERENCES customers(id)
ON DELETE CASCADE;

-- Restrict: não permite apagar pai com filhos
ON DELETE RESTRICT;

-- Set NULL: põe NULL nos filhos
ON DELETE SET NULL;

-- Set Default: põe valor padrão
ON DELETE SET DEFAULT;
```

### **🔍 Álgebra Relacional:**

#### **Operações Básicas:**

```text
σ (Seleção)     → WHERE
π (Projeção)    → SELECT colunas específicas
× (Produto)     → CROSS JOIN
⋈ (Junção)      → JOIN
∪ (União)       → UNION
∩ (Interseção)  → INTERSECT
- (Diferença)   → EXCEPT/MINUS
```