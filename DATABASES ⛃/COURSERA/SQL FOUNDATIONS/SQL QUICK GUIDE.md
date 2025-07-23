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
### Obter várias colunas
```sql
SELECT FirstName, LastName 
FROM Customer;
```
### Ordenar por coluna (ascendente)
```sql
SELECT FirstName, LastName 
FROM Customer
ORDER BY LastName ASC;
```
### Ordenar por coluna (descendente)
```sql
SELECT FirstName, LastName 
FROM Customer
ORDER BY State DESC;
```

### Listagem única (valores distintos)
```sql
SELECT DISTINCT State 
FROM Customer;
```

### Limitar resultados
```sql
SELECT TOP 5 FirstName, LastName 
FROM Customer;
```

### Saltar linhas (OFFSET / FETCH)
```sql
SELECT CustomerId, LastName 
FROM Customer
ORDER BY CustomerId
OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;
```

## 🎯 FILTERING DATA

### Operadores comuns

- `=` : Igual
    
- `!=` ou `<>` : Diferente
    
- `<` : Menor que
    
- `<=` : Menor ou igual
    
- `>` : Maior que
    
- `>=` : Maior ou igual
    
- `AND` : Ambas as condições verdadeiras
    
- `OR` : Pelo menos uma condição verdadeira
    
- `NOT` : Inverte a condição
    
- `BETWEEN` : Dentro de um intervalo
    
- `LIKE %` : Qualquer número de caracteres
    
- `LIKE _` : Um único caractere

### 🔢 Filtrar dados numéricos

```sql
SELECT CustomerId, Total 
FROM Invoice
WHERE Total >= 0.99;
```

```sql
SELECT CustomerId, Total 
FROM Invoice
WHERE Total BETWEEN 0.50 AND 9.99;
```


### 🔢 Filtrar dados de texto

```sql
SELECT * 
FROM Customer
WHERE Country = 'Germany';
```

```sql
SELECT * 
FROM Customer
WHERE Country = 'Germany' OR Country = 'France';
```

```sql
SELECT * 
FROM Customer
WHERE NOT Country = 'USA';
```

```sql
SELECT FirstName, LastName 
FROM Customer
WHERE LastName LIKE 'A%';
```

```sql
SELECT FirstName, LastName 
FROM Customer
WHERE LastName LIKE '_e%';
```

```sql
SELECT FirstName, LastName 
FROM Customer
WHERE LastName NOT LIKE '%A';
```



### 🧮 Filtros em múltiplas colunas

```sql
SELECT CustomerId, Total 
FROM Invoice
WHERE Total > 10 AND BillingCity = 'Paris';
```

### 🕳️ Filtrar dados nulos

```sql
SELECT CustomerId 
FROM Invoice
WHERE Total IS NOT NULL;
```

## 🔧 TRANSFORMING DATA

### Funções de Texto

```sql
SELECT UPPER(FirstName) 
FROM Customer;
```
