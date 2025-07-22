### **🖥️ Introdução ao SSMS:**

#### **O que é o SQL Server Management Studio?**

> SSMS é a ferramenta oficial da Microsoft para administrar, configurar, gerir e desenvolver componentes do SQL Server. É o equivalente ao MySQL Workbench para SQL Server.

#### **Características Principais:**

```text
🎯 Interface Gráfica - Gestão visual de bases de dados
📊 Query Editor - Editor avançado com IntelliSense
🔧 Administração - Gestão de utilizadores, permissões, backups
📈 Performance - Ferramentas de monitorização e otimização
🔍 Debug - Debugging de stored procedures e funções
📋 Relatórios - Relatórios integrados de sistema
🔄 Importação/Exportação - Ferramentas de migração de dados
```

### **⚙️ Interface do SSMS:**

#### **Componentes Principais:**

```text
📁 Object Explorer:
   - Navegação hierárquica de objetos
   - Databases → Tables → Columns
   - Security → Logins → Roles
   - Management → Maintenance Plans

📝 Query Editor:
   - Editor SQL com syntax highlighting
   - IntelliSense para auto-complete
   - Execution plans
   - Results pane

📊 Solution Explorer:
   - Projetos de base de dados
   - Scripts organizados
   - Controlo de versões

🔧 Properties Window:
   - Propriedades de objetos selecionados
   - Configurações detalhadas
```

#### **Configuração Inicial:**

```sql
-- Configurações recomendadas
-- Tools → Options → Query Execution → SQL Server → General
-- ✅ SET NOCOUNT ON by default
-- ✅ SET QUOTED_IDENTIFIER ON
-- ✅ SET ANSI_NULLS ON

-- Query Results → SQL Server → Results to Grid
-- ✅ Include column headers when copying
-- ✅ Retain CR/LF on copy or save

-- Text Editor → Transact-SQL → IntelliSense
-- ✅ Enable IntelliSense
-- ✅ Underline errors
-- ✅ Enable Quick Info
```

### **🗃️ Gestão de Bases de Dados:**

#### **Criar Base de Dados:**

```sql
-- Via T-SQL
CREATE DATABASE ECommerceDB
ON (
    NAME = 'ECommerceDB_Data',
    FILENAME = 'C:\Data\ECommerceDB.mdf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 10MB
)
LOG ON (
    NAME = 'ECommerceDB_Log',
    FILENAME = 'C:\Data\ECommerceDB.ldf',
    SIZE = 10MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 5MB
);

-- Via Interface:
-- 1. Right-click "Databases" no Object Explorer
-- 2. "New Database..."
-- 3. Database name: ECommerceDB
-- 4. Options → Initial Size, Autogrowth
-- 5. OK
```

#### **Configurações de Base de Dados:**

```sql
-- Verificar configurações atuais
SELECT 
    name,
    database_id,
    collation_name,
    state_desc,
    recovery_model_desc,
    compatibility_level
FROM sys.databases
WHERE name = 'ECommerceDB';

-- Alterar modelo de recuperação
ALTER DATABASE ECommerceDB SET RECOVERY SIMPLE;

-- Alterar compatibility level
ALTER DATABASE ECommerceDB SET COMPATIBILITY_LEVEL = 150; -- SQL Server 2019

-- Configurar Auto Shrink (geralmente desativado)
ALTER DATABASE ECommerceDB SET AUTO_SHRINK OFF;

-- Configurar Auto Update Statistics
ALTER DATABASE ECommerceDB SET AUTO_UPDATE_STATISTICS ON;
```

### **📋 Gestão de Tabelas:**

#### **Criar Tabelas via Designer:**

```text
1. Expand Database → Tables
2. Right-click → "New Table..."
3. Table Designer opens:
   - Column Name
   - Data Type  
   - Allow Nulls checkbox
   - Default Value
4. Set Primary Key: Right-click column → "Set Primary Key"
5. Save: Ctrl+S, give table name
```

#### **Tipos de Dados SQL Server:**

```sql
-- Tipos Numéricos
BIT                    -- Boolean (0 ou 1)
TINYINT               -- 0 a 255
SMALLINT              -- -32,768 a 32,767
INT                   -- -2 bilhões a 2 bilhões
BIGINT                -- Números muito grandes
DECIMAL(p,s)          -- Precisão fixa
NUMERIC(p,s)          -- Sinónimo de DECIMAL
FLOAT                 -- Ponto flutuante
REAL                  -- Ponto flutuante menor

-- Tipos de String
CHAR(n)               -- Comprimento fixo
VARCHAR(n)            -- Comprimento variável
NCHAR(n)              -- Unicode fixo
NVARCHAR(n)           -- Unicode variável
TEXT                  -- Texto longo (deprecated)
NTEXT                 -- Unicode texto longo (deprecated)
VARCHAR(MAX)          -- Até 2GB
NVARCHAR(MAX)         -- Unicode até 2GB

-- Tipos de Data/Hora
DATE                  -- Apenas data
TIME                  -- Apenas hora
DATETIME              -- Data e hora (legacy)
DATETIME2             -- Data e hora melhorada
DATETIMEOFFSET        -- Com timezone
SMALLDATETIME         -- Precisão menor

-- Outros
UNIQUEIDENTIFIER      -- GUID
XML                   -- Dados XML
VARBINARY(MAX)        -- Dados binários
IMAGE                 -- Imagens (deprecated)
```

#### **Exemplo Completo de Tabela:**

```sql
-- Criar tabela complexa
CREATE TABLE Customers (
    CustomerID INT IDENTITY(1,1) PRIMARY KEY,
    CustomerNumber AS ('CUST' + RIGHT('000000' + CAST(CustomerID AS VARCHAR(6)), 6)) PERSISTED,
    FirstName NVARCHAR(50) NOT NULL,
    LastName NVARCHAR(50) NOT NULL,
    Email NVARCHAR(200) NOT NULL UNIQUE,
    Phone NVARCHAR(20),
    DateOfBirth DATE,
    
    -- Address fields
    StreetAddress NVARCHAR(255),
    City NVARCHAR(100),
    PostalCode NVARCHAR(20),
    Country NVARCHAR(100) DEFAULT 'Portugal',
    
    -- Account info
    AccountStatus NVARCHAR(20) DEFAULT 'Active' 
        CHECK (AccountStatus IN ('Active', 'Inactive', 'Suspended')),
    CreditLimit DECIMAL(10,2) DEFAULT 1000.00,
    
    -- Stats
    TotalOrders INT DEFAULT 0,
    TotalSpent DECIMAL(12,2) DEFAULT 0.00,
    LoyaltyPoints INT DEFAULT 0,
    
    -- Audit
    CreatedDate DATETIME2 DEFAULT GETDATE(),
    CreatedBy NVARCHAR(100) DEFAULT SYSTEM_USER,
    ModifiedDate DATETIME2 DEFAULT GETDATE(),
    ModifiedBy NVARCHAR(100) DEFAULT SYSTEM_USER,
    
    -- Soft delete
    IsDeleted BIT DEFAULT 0,
    DeletedDate DATETIME2 NULL,
    
    -- Indexes
    INDEX IX_Customers_Email (Email),
    INDEX IX_Customers_Status (AccountStatus, IsDeleted),
    INDEX IX_Customers_Country (Country)
);
```

### **🔍 Query Editor Avançado:**

#### **Funcionalidades do Editor:**

```sql
-- IntelliSense (Ctrl+Space para forçar)
SELECT c.FirstName,     -- IntelliSense sugere colunas
       c.LastName
       o.OrderDate -- Auto-complete de tabelas JOINed  
FROM Customers c  
INNER JOIN Orders o ON c.CustomerID = o.CustomerID;

-- Code Snippets (Ctrl+K, Ctrl+X)  
-- Snippets incluem: CREATE TABLE, INSERT, UPDATE, DELETE, etc.

-- Query Execution Options  
SET STATISTICS IO ON; -- Mostra I/O statistics  
SET STATISTICS TIME ON; -- Mostra tempo de execução  
SET SHOWPLAN_ALL ON; -- Mostra execution plan textual

-- Execution Plan (Ctrl+M para incluir actual plan)  
SELECT c.FirstName, c.LastName, COUNT(o.OrderID) as OrderCount  
FROM Customers c  
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID  
GROUP BY c.CustomerID, c.FirstName, c.LastName  
HAVING COUNT(o.OrderID) > 5;

-- Results Options  
-- Ctrl+Shift+R: Results to Text  
-- Ctrl+D: Results to Grid (default)  
-- Ctrl+Shift+F: Results to File
```

