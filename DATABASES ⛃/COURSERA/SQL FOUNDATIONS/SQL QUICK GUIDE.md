# 📊 SQL – Guia de Consulta com Base no Chinook

_Exemplos baseados nas tabelas **Customer** e **Invoice** do conjunto de dados Chinook._

---

## 🧾 Tabelas e Colunas Utilizadas

### 🧮 Tabela: `Invoice`
| Coluna        | Descrição         |
|---------------|-------------------|
| InvoiceId     | ID da fatura      |
| CustomerId    | ID do cliente     |
| InvoiceDate   | Data da fatura    |
| BillingState  | Estado de faturação |
| Total         | Total da fatura   |

### 👤 Tabela: `Customer`
| Coluna     | Descrição       |
|------------|-----------------|
| CustomerId | ID do cliente   |
| FirstName  | Nome próprio    |
| LastName   | Apelido         |
| City       | Cidade          |
| State      | Estado          |
| Country    | País            |

---

## 🔍 QUERYING TABLES

### Obter todas as colunas
```sql
SELECT * 
FROM Customer;
```



```
Copy
Edit
SELECT Country 
FROM Customer;
Obter várias colunas
sql
Copy
Edit
SELECT FirstName, LastName 
FROM Customer;
Ordenar por coluna (ascendente)
sql
Copy
Edit
SELECT FirstName, LastName 
FROM Customer
ORDER BY LastName ASC;
Ordenar por coluna (descendente)
sql
Copy
Edit
SELECT FirstName, LastName 
FROM Customer
ORDER BY State DESC;
Listagem única (valores distintos)
sql
Copy
Edit
SELECT DISTINCT State 
FROM Customer;
Limitar resultados
sql
Copy
Edit
SELECT TOP 5 FirstName, LastName 
FROM Customer;
Saltar linhas (OFFSET / FETCH)
sql
Copy
Edit
SELECT CustomerId, LastName 
FROM Customer
ORDER BY CustomerId
OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
🎯 FILTERING DATA
Operadores comuns
= : Igual

!= ou <> : Diferente

< : Menor que

<= : Menor ou igual

> : Maior que

>= : Maior ou igual

AND : Ambas as condições verdadeiras

OR : Pelo menos uma condição verdadeira

NOT : Inverte a condição

BETWEEN : Dentro de um intervalo

LIKE % : Qualquer número de caracteres

LIKE _ : Um único caractere

🔢 Filtrar dados numéricos
sql
Copy
Edit
SELECT CustomerId, Total 
FROM Invoice
WHERE Total >= 0.99;
sql
Copy
Edit
SELECT CustomerId, Total 
FROM Invoice
WHERE Total BETWEEN 0.50 AND 9.99;
🔤 Filtrar dados de texto
sql
Copy
Edit
SELECT * 
FROM Customer
WHERE Country = 'Germany';
sql
Copy
Edit
SELECT * 
FROM Customer
WHERE Country = 'Germany' OR Country = 'France';
sql
Copy
Edit
SELECT * 
FROM Customer
WHERE NOT Country = 'USA';
sql
Copy
Edit
SELECT FirstName, LastName 
FROM Customer
WHERE LastName LIKE 'A%';
sql
Copy
Edit
SELECT FirstName, LastName 
FROM Customer
WHERE LastName LIKE '_e%';
sql
Copy
Edit
SELECT FirstName, LastName 
FROM Customer
WHERE LastName NOT LIKE '%A';
🧮 Filtros em múltiplas colunas
sql
Copy
Edit
SELECT CustomerId, Total 
FROM Invoice
WHERE Total > 10 AND BillingCity = 'Paris';
🕳️ Filtrar dados nulos
sql
Copy
Edit
SELECT CustomerId 
FROM Invoice
WHERE Total IS NOT NULL;
🔧 TRANSFORMING DATA
Funções de Texto
sql
Copy
Edit
SELECT UPPER(FirstName) 
FROM Customer;
sql
Copy
Edit
SELECT CONCAT(FirstName, ' ', LastName) 
FROM Customer;
sql
Copy
Edit
SELECT SUBSTRING(LastName, 1, 4) 
FROM Customer;
Funções Matemáticas
sql
Copy
Edit
SELECT ROUND(Total, 2) 
FROM Invoice;
sql
Copy
Edit
SELECT SUM(Total) 
FROM Invoice
WHERE CustomerId = 5;
sql
Copy
Edit
SELECT AVG(Total) 
FROM Invoice;
Funções de Data e Hora
sql
Copy
Edit
SELECT FORMAT(InvoiceDate, 'MM-dd-yyyy') 
FROM Invoice;
sql
Copy
Edit
SELECT FORMAT(GETDATE(), 'MM-dd-yyyy');
sql
Copy
Edit
SELECT DATEDIFF(day, '2022-01-01', '2023-01-01');
Funções de Agregação
sql
Copy
Edit
SELECT COUNT(InvoiceId) 
FROM Invoice
WHERE Total > 10;
sql
Copy
Edit
SELECT MIN(Total) 
FROM Invoice;
sql
Copy
Edit
SELECT MAX(Total) 
FROM Invoice;
🔗 JOINING TABLES
🔸 INNER JOIN
Retorna apenas linhas com correspondência em ambas as tabelas.

sql
Copy
Edit
SELECT 
    Customer.CustomerId,
    Customer.LastName,
    Invoice.InvoiceId
FROM Customer
JOIN Invoice 
    ON Customer.State = Invoice.BillingState;
🔸 LEFT JOIN
Retorna todos os registos da tabela da esquerda, e os correspondentes da direita (se existirem).

sql
Copy
Edit
SELECT 
    Customer.CustomerId,
    Customer.LastName,
    Invoice.InvoiceId
FROM Customer
LEFT JOIN Invoice 
    ON Customer.State = Invoice.BillingState;
🔸 FULL OUTER JOIN
Retorna todos os registos de ambas as tabelas, com NULL onde não houver correspondência.

sql
Copy
Edit
SELECT 
    Customer.CustomerId,
    Customer.LastName,
    Invoice.InvoiceId
FROM Customer
FULL OUTER JOIN Invoice 
    ON Customer.State = Invoice.BillingState;
🏷️ USING ALIASES
🔹 Alias para colunas
sql
Copy
Edit
SELECT CONCAT(FirstName, ' ', LastName) AS FullName 
FROM Customer;
🔹 Alias para tabelas
sql
Copy
Edit
SELECT 
    c.CustomerId, 
    c.FirstName, 
    c.LastName, 
    i.Total AS InvoiceTotal
FROM Customer c
JOIN Invoice i 
    ON c.CustomerId = i.CustomerId;