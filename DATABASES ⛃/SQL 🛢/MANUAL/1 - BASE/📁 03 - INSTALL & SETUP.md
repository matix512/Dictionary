### **🛠️ Escolher o SGBD:**

#### **MySQL (Recomendado para iniciantes):**

```text
✅ Gratuito e open-source
✅ Fácil instalação
✅ Grande comunidade
✅ Documentação extensa
✅ MySQL Workbench visual
```

#### **SQL Server (Microsoft):**

```text
✅ SSMS muito poderoso
✅ Integração Windows
✅ Express gratuito
⚠️ Só Windows/Linux
```

#### **PostgreSQL (Avançado):**

```text
✅ Muito robusto
✅ Standards SQL
✅ Extensibilidade
⚠️ Curva aprendizagem
```

### **📥 Instalação MySQL:**

#### **Windows:**

textresponse-action-icon

```text
1. Download: https://dev.mysql.com/downloads/installer/
2. Escolher "mysql-installer-web-community"
3. Instalar:
   - MySQL Server
   - MySQL Workbench
   - MySQL Shell (opcional)
4. Configurar root password
5. Testar conexão
```

#### **macOS:**

bashresponse-action-icon

```bash
# Homebrew (recomendado)
brew install mysql
brew services start mysql
mysql_secure_installation

# Download direto
# https://dev.mysql.com/downloads/mysql/
```

#### **Linux (Ubuntu/Debian):**

bashresponse-action-icon

```bash
sudo apt update
sudo apt install mysql-server mysql-client
sudo mysql_secure_installation
sudo systemctl start mysql
sudo systemctl enable mysql
```

### **🔧 Configuração Inicial:**

#### **Primeira Conexão:**

sqlresponse-action-icon

```sql
-- Conectar como root
mysql -u root -p

-- Criar nova base de dados
CREATE DATABASE learning_db;

-- Usar a base de dados
USE learning_db;

-- Criar utilizador para prática
CREATE USER 'student'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON learning_db.* TO 'student'@'localhost';
FLUSH PRIVILEGES;
```

#### **Verificar Instalação:**

sqlresponse-action-icon

```sql
-- Ver versão
SELECT VERSION();

-- Ver bases de dados
SHOW DATABASES;

-- Ver utilizador atual
SELECT USER();

-- Ver configurações importantes
SHOW VARIABLES LIKE '%char%';
SHOW VARIABLES LIKE 'sql_mode';
```

### **💻 MySQL Workbench Setup:**

#### **Criar Conexão:**

textresponse-action-icon

```text
1. Abrir MySQL Workbench
2. "+" ao lado de "MySQL Connections"
3. Configurar:
   - Connection Name: "Local Learning"
   - Hostname: localhost
   - Port: 3306
   - Username: student
   - Password: (guardar no keychain)
4. Test Connection
5. OK
```

#### **Interface Essencial:**

textresponse-action-icon

```text
📁 Navigator: Schemas, Administration
📝 Query Tab: Escrever SQL
📊 Result Grid: Ver resultados
⚡ Action Output: Logs e mensagens
🔧 Server Status: Performance
```

### **📊 SQL Server Setup (Alternativo):**

#### **SQL Server Express:**

textresponse-action-icon

```text
1. Download: https://www.microsoft.com/sql-server/sql-server-downloads
2. Escolher "Express"
3. Basic installation
4. Download SSMS separadamente
5. Conectar com Windows Authentication
```

#### **SQL Server Management Studio:**

textresponse-action-icon

```text
1. Download: https://docs.microsoft.com/en-us/sql/ssms/download
2. Instalar SSMS
3. Conectar:
   - Server type: Database Engine
   - Server name: localhost\SQLEXPRESS
   - Authentication: Windows
```

### **📚 Dataset de Prática:**

#### **Criar Estrutura de Aprendizagem:**

sqlresponse-action-icon

```sql
-- Base de dados para prática
CREATE DATABASE practice_db;
USE practice_db;

-- Tabela simples para começar
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    birth_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Inserir dados de teste
INSERT INTO students (first_name, last_name, email, birth_date) VALUES
('João', 'Silva', 'joao@email.com', '1995-03-15'),
('Maria', 'Santos', 'maria@email.com', '1992-07-22'),
('Pedro', 'Oliveira', 'pedro@email.com', '1998-11-08'),
('Ana', 'Costa', 'ana@email.com', '1990-05-30'),
('Carlos', 'Ferreira', 'carlos@email.com', '1994-09-12');
```

### **⚙️ Configurações Recomendadas:**

#### **MySQL my.cnf/my.ini:**

iniresponse-action-icon

```ini
[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
sql_mode = STRICT_TRANS_TABLES,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO
```

#### **Verificar Configurações:**

sqlresponse-action-icon

```sql
-- Ver charset
SHOW VARIABLES LIKE 'character_set%';

-- Ver sql_mode
SELECT @@sql_mode;

-- Ver timezone
SELECT @@time_zone;
```

### **🚨 Troubleshooting Comum:**

#### **Problemas de Conexão:**

bashresponse-action-icon

```bash
# MySQL não inicia
sudo systemctl status mysql
sudo systemctl start mysql

# Esqueceu password root
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'newpassword';
FLUSH PRIVILEGES;

# Porta ocupada
netstat -tulpn | grep 3306
```

#### **Problemas de Charset:**

sqlresponse-action-icon

```sql
-- Se aparecem caracteres estranhos
ALTER DATABASE practice_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
ALTER TABLE students CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```