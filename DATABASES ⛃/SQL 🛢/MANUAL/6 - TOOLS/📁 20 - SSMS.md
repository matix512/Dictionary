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



#### **Debugging de Stored Procedures:**

```sql
-- Criar procedure para debug
CREATE PROCEDURE GetCustomerOrderSummary
    @CustomerID INT,
    @StartDate DATE = NULL,
    @EndDate DATE = NULL
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Debug: Print parameters
    PRINT 'CustomerID: ' + CAST(@CustomerID AS VARCHAR(10));
    PRINT 'StartDate: ' + ISNULL(CAST(@StartDate AS VARCHAR(10)), 'NULL');
    PRINT 'EndDate: ' + ISNULL(CAST(@EndDate AS VARCHAR(10)), 'NULL');
    
    -- Set defaults
    IF @StartDate IS NULL SET @StartDate = DATEADD(YEAR, -1, GETDATE());
    IF @EndDate IS NULL SET @EndDate = GETDATE();
    
    -- Main query
    SELECT 
        c.FirstName + ' ' + c.LastName AS CustomerName,
        COUNT(o.OrderID) AS TotalOrders,
        SUM(o.TotalAmount) AS TotalSpent,
        AVG(o.TotalAmount) AS AvgOrderValue,
        MIN(o.OrderDate) AS FirstOrder,
        MAX(o.OrderDate) AS LastOrder
    FROM Customers c
    LEFT JOIN Orders o ON c.CustomerID = o.CustomerID 
        AND o.OrderDate BETWEEN @StartDate AND @EndDate
    WHERE c.CustomerID = @CustomerID
    GROUP BY c.CustomerID, c.FirstName, c.LastName;
END;

-- Debug usando breakpoints e step-through
-- 1. Right-click na procedure → "Debug Procedure..."
-- 2. Enter parameters
-- 3. Use F10 (Step Over), F11 (Step Into)
-- 4. Watch variables no Debug window
````

### **📊 Ferramentas de Performance:**

#### **Activity Monitor:**

```text
-- Aceder: Right-click server instance → "Activity Monitor"
-- Ou: Ctrl+Alt+A

Secções do Activity Monitor:
📈 Overview: CPU, I/O, batch requests
🔄 Processes: Sessions ativas, blocking
💾 Resource Waits: Waits que impactam performance
📁 Data File I/O: I/O por ficheiro de dados
💽 Recent Expensive Queries: Queries mais custosas
```

#### **Execution Plans:**

```sql
-- Incluir execution plan
SET SHOWPLAN_XML ON;
-- ou usar Ctrl+M no SSMS

-- Exemplo para análise
SELECT 
    p.ProductName,
    c.CategoryName,
    AVG(od.UnitPrice * od.Quantity) AS AvgOrderValue
FROM Products p
INNER JOIN Categories c ON p.CategoryID = c.CategoryID
INNER JOIN OrderDetails od ON p.ProductID = od.ProductID
INNER JOIN Orders o ON od.OrderID = o.OrderID
WHERE o.OrderDate >= DATEADD(MONTH, -6, GETDATE())
GROUP BY p.ProductID, p.ProductName, c.CategoryName
ORDER BY AvgOrderValue DESC;

-- Analisar no execution plan:
-- 🔍 Table Scan vs Index Seek
-- 📊 Cost percentages
-- ⚠️ Yellow warnings
-- 🔄 Join types
-- 📈 Estimated vs Actual rows
```

#### **Performance Tuning Advisor:**

sqlresponse-action-icon

```sql
-- Database Engine Tuning Advisor (DTA)
-- 1. Tools → "Database Engine Tuning Advisor"
-- 2. Select workload (file or table)
-- 3. Select databases to tune
-- 4. Choose tuning options
-- 5. Start Analysis
-- 6. Review recommendations

-- Query Store (SQL Server 2016+)
ALTER DATABASE ECommerceDB SET QUERY_STORE = ON;

-- Ver top queries no Query Store
SELECT TOP 10
    q.query_id,
    qt.query_sql_text,
    rs.avg_duration/1000.0 AS avg_duration_ms,
    rs.avg_cpu_time/1000.0 AS avg_cpu_ms,
    rs.count_executions
FROM sys.query_store_query q
JOIN sys.query_store_query_text qt ON q.query_text_id = qt.query_text_id
JOIN sys.query_store_runtime_stats rs ON q.query_id = rs.query_id
ORDER BY rs.avg_duration DESC;
```

### **🔐 Gestão de Segurança:**

#### **Logins e Users:**

sqlresponse-action-icon

```sql
-- Criar SQL Server Login
CREATE LOGIN JohnDoe WITH PASSWORD = 'StrongPassword123!';

-- Criar Database User
USE ECommerceDB;
CREATE USER JohnDoe FOR LOGIN JohnDoe;

-- Via Interface SSMS:
-- Security → Logins → Right-click → "New Login..."
-- General: Login name, Authentication type
-- Server Roles: sysadmin, dbcreator, etc.
-- User Mapping: Map to databases
-- Securables: Specific permissions

-- Database Level User
-- ECommerceDB → Security → Users → Right-click → "New User..."
```

#### **Roles e Permissions:**

sqlresponse-action-icon

```sql
-- Database Roles predefinidas
-- db_owner: Full control of database
-- db_datareader: Can read all data
-- db_datawriter: Can write all data
-- db_ddladmin: Can run DDL commands

-- Criar Custom Role
CREATE ROLE sales_team;

-- Adicionar user ao role
ALTER ROLE sales_team ADD MEMBER JohnDoe;

-- Dar permissões ao role
GRANT SELECT, INSERT, UPDATE ON Customers TO sales_team;
GRANT SELECT ON Orders TO sales_team;
GRANT EXECUTE ON GetCustomerOrderSummary TO sales_team;

-- Row Level Security (RLS)
CREATE SCHEMA Security;

CREATE FUNCTION Security.fn_SecurityPredicate(@SalespersonID INT)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS fn_SecurityPredicate_result
WHERE @SalespersonID = USER_ID() OR IS_ROLEMEMBER('sales_manager') = 1;

CREATE SECURITY POLICY CustomerSecurityPolicy
ADD FILTER PREDICATE Security.fn_SecurityPredicate(SalespersonID) ON Customers,
ADD BLOCK PREDICATE Security.fn_SecurityPredicate(SalespersonID) ON Customers
WITH (STATE = ON);
```

### **💾 Backup e Restore:**

#### **Backup Strategies:**

sqlresponse-action-icon

```sql
-- Full Backup
BACKUP DATABASE ECommerceDB 
TO DISK = 'C:\Backups\ECommerceDB_Full.bak'
WITH FORMAT, INIT, 
NAME = 'ECommerceDB Full Backup',
COMPRESSION;

-- Differential Backup
BACKUP DATABASE ECommerceDB 
TO DISK = 'C:\Backups\ECommerceDB_Diff.bak'
WITH DIFFERENTIAL, FORMAT, INIT,
NAME = 'ECommerceDB Differential Backup',
COMPRESSION;

-- Transaction Log Backup
BACKUP LOG ECommerceDB 
TO DISK = 'C:\Backups\ECommerceDB_Log.trn'
WITH FORMAT, INIT,
NAME = 'ECommerceDB Log Backup',
COMPRESSION;

-- Via SSMS:
-- Right-click Database → "Tasks" → "Back Up..."
-- Backup type: Full, Differential, Transaction Log
-- Destination: Add backup file
-- Options: Compression, verification
```

#### **Restore Operations:**

sqlresponse-action-icon

```sql
-- Restore Full Backup
RESTORE DATABASE ECommerceDB 
FROM DISK = 'C:\Backups\ECommerceDB_Full.bak'
WITH REPLACE, NORECOVERY;

-- Restore Differential
RESTORE DATABASE ECommerceDB 
FROM DISK = 'C:\Backups\ECommerceDB_Diff.bak'
WITH NORECOVERY;

-- Restore Log
RESTORE LOG ECommerceDB 
FROM DISK = 'C:\Backups\ECommerceDB_Log.trn'
WITH RECOVERY;

-- Point-in-time restore
RESTORE LOG ECommerceDB 
FROM DISK = 'C:\Backups\ECommerceDB_Log.trn'
WITH STOPAT = '2024-01-15 14:30:00',
RECOVERY;

-- Via SSMS:
-- Right-click Database → "Tasks" → "Restore" → "Database..."
-- Source: Device, select backup files
-- Timeline: Latest backup or specific point in time
```

### **📈 Maintenance Plans:**

#### **Criar Maintenance Plan:**

textresponse-action-icon

```text
Management → Maintenance Plans → Right-click → "New Maintenance Plan..."

Tarefas Comuns:
🔧 Update Statistics: Manter estatísticas atualizadas
🗑️ Cleanup History: Limpar histórico antigo
💾 Backup Database: Backups automatizados
🔍 Check Database Integrity: DBCC CHECKDB
📊 Rebuild Index: Reorganizar índices fragmentados
📁 Cleanup Maintenance: Remover ficheiros antigos
```

#### **Maintenance Plan via T-SQL:**

sqlresponse-action-icon

```sql
-- Job para maintenance
USE msdb;
GO

-- Criar job
EXEC dbo.sp_add_job
    @job_name = N'ECommerceDB Maintenance';

-- Adicionar step para update statistics
EXEC dbo.sp_add_jobstep
    @job_name = N'ECommerceDB Maintenance',
    @step_name = N'Update Statistics',
    @command = N'USE ECommerceDB; EXEC sp_updatestats;',
    @database_name = N'ECommerceDB';

-- Adicionar step para reindex
EXEC dbo.sp_add_jobstep
    @job_name = N'ECommerceDB Maintenance',
    @step_name = N'Reindex Tables',
    @command = N'
        USE ECommerceDB;
        DECLARE @TableName NVARCHAR(255);
        DECLARE table_cursor CURSOR FOR
        SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES 
        WHERE TABLE_TYPE = ''BASE TABLE'';
        
        OPEN table_cursor;
        FETCH NEXT FROM table_cursor INTO @TableName;
        
        WHILE @@FETCH_STATUS = 0
        BEGIN
            EXEC(''ALTER INDEX ALL ON '' + @TableName + '' REBUILD'');
            FETCH NEXT FROM table_cursor INTO @TableName;
        END;
        
        CLOSE table_cursor;
        DEALLOCATE table_cursor;
    ',
    @database_name = N'ECommerceDB';

-- Schedule (executar todas as noites às 2:00 AM)
EXEC dbo.sp_add_schedule
    @schedule_name = N'Nightly Maintenance',
    @freq_type = 4, -- Daily
    @active_start_time = 020000; -- 2:00 AM

-- Attach schedule to job
EXEC dbo.sp_attach_schedule
    @job_name = N'ECommerceDB Maintenance',
    @schedule_name = N'Nightly Maintenance';

-- Add job to server
EXEC dbo.sp_add_jobserver
    @job_name = N'ECommerceDB Maintenance';
```

### **📊 Import/Export Data:**

#### **SQL Server Import/Export Wizard:**

textresponse-action-icon

```text
Right-click Database → "Tasks" → "Import Data..." ou "Export Data..."

Fontes suportadas:
📄 Flat files (CSV, TXT)
📊 Excel files
🗃️ Access databases
🔗 OLE DB sources
📡 ODBC sources
🌐 .NET Framework Data Providers

Processo:
1. Choose Data Source
2. Choose Destination  
3. Copy data or write query
4. Select tables/views
5. Review mapping
6. Save and run package
```

#### **BULK INSERT para Ficheiros:**

sqlresponse-action-icon

```sql
-- Preparar tabela de staging
CREATE TABLE CustomerImport (
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    Email NVARCHAR(200),
    Phone NVARCHAR(20),
    City NVARCHAR(100)
);

-- Bulk insert de CSV
BULK INSERT CustomerImport
FROM 'C:\Data\customers.csv'
WITH (
    FORMAT = 'CSV',
    FIRSTROW = 2,           -- Skip header
    FIELDTERMINATOR = ',',  -- CSV delimiter
    ROWTERMINATOR = '\n',   -- Line ending
    TABLOCK                 -- Performance improvement
);

-- Verificar dados importados
SELECT TOP 10 * FROM CustomerImport;

-- Transferir para tabela principal após validação
INSERT INTO Customers (FirstName, LastName, Email, Phone, City)
SELECT FirstName, LastName, Email, Phone, City
FROM CustomerImport
WHERE Email IS NOT NULL 
AND Email LIKE '%@%'
AND FirstName IS NOT NULL;
```

### **🔧 Ferramentas Adicionais:**

#### **SQL Server Profiler:**

textresponse-action-icon

```text
Tools → "SQL Server Profiler"

Eventos para Monitorizar:
🔍 SQL:BatchCompleted - Queries completed
🔄 RPC:Completed - Stored procedure calls
⚠️ Exception - Errors and warnings
🔒 Security Audit - Login/logout events
📊 Performance - Deadlocks, blocking

Template comum: Standard (default)
Filtros: Application, Database, Duration > X
```

#### **Database Diagram:**

textresponse-action-icon

```text
-- Criar Database Diagram
Database → Database Diagrams → Right-click → "New Database Diagram"

Funcionalidades:
📋 Add tables visually
🔗 Show relationships
🔧 Modify table structure
📊 Generate change script
💾 Save as image

-- Enable diagramming (se necessário)
EXEC sp_changedbowner 'sa';
```

### **🎯 Exercícios Práticos SSMS:**

#### **Exercício 1 - Setup Completo:**

sqlresponse-action-icon

```sql
-- 1. Criar nova database via wizard
-- 2. Criar estas tabelas usando Table Designer:

-- Categories table
CategoryID (int, identity, PK)
CategoryName (nvarchar(50), not null)
Description (nvarchar(255))
IsActive (bit, default 1)

-- Products table  
ProductID (int, identity, PK)
ProductName (nvarchar(100), not null)
CategoryID (int, FK to Categories)
UnitPrice (decimal(10,2), not null, default 0)
UnitsInStock (int, not null, default 0)
Discontinued (bit, default 0)

-- 3. Criar relacionamento via Database Diagram
-- 4. Popular com dados via Import Wizard
-- 5. Criar índices via Index wizard
```