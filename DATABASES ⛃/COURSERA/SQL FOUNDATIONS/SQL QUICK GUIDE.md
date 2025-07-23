# SAMPLE DATA

_The data used in examples throughout this guide are taken from the Chinook database and limited to the following tables and columns._

Table 1: Invoice Table

|   |   |   |   |   |
|---|---|---|---|---|
|COLUMN 1|COLUMN 2|COLUMN 3|COLUMN 4|COLUMN 5|
|InvoiceId|CustomerId|InvoiceDate|BillingState|Total|

Table 2: Customer Table

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|COLUMN 1|COLUMN 2|COLUMN 3|COLUMN 4|COLUMN 5|COLUMN 6|
|CustomerID|FirstName|LastName|City|State|Country|

# QUERYING TABLES

  

_Get all the columns from a table._


**SELECT** *

**FROM** Custome r;

_Get one column from a table._

**SELECT** Country  
**FROM** Custome r;

_Get two or more columns from a table._

**SELECT** FirstName, LastName  
**FROM** Custome r;

_Get two columns from a table displayed in ascending order.  
_**SELECT** FirstName, LastName  
**FROM** Customer

**ORDER BY** LastName **ASC** ;

_Get two columns from a table displayed in descending order._

**SELECT** FirstName, LastName  
**FROM** Customer

**ORDER BY** State **DESC** ;

_Get a unique list from a table._

**SELECT DISTINCT** State

**FROM** Custome r;

Get the first 5 rows from a table

**SELECT TOP 5**  FirstName, LastName

**FROM** Customer ;

_Get an offset number of rows from a table._

**SELECT** CustomerId, LastName  
**FROM** Customer

**ORDER BY** CustomerId

**OFFSET** 10 ROWS **FETCH** NEXT 5 ROWS ONLY ;

  

# FILTERING DATA

  

●      **=**: Equal to

●      **!=** **or <>**: Not equal to

●      **<**: Less than

●      **<=**: Less than or equal to

●      **>**: Greater than

●      **>=**: Greater than or equal to

●      **AND**: All conditions must be true.

●      **OR**: At least one condition must be true.

●      **NOT**: The condition must be false.

●      **BETWEEN**: Finds data within a range

●      **LIKE %**: Any number of characters

●      **LIKE _**: A single character

## Filtering on Numeric Columns

_Get information where criteria is more than or equal to other records._  
**SELECT** CustomerId, Total  
**FROM** Invoice  
**WHERE** Total **>=** 0.99

_Get all information that meets two criteria exclusively.  
_**SELECT** CustomerId, Total  
**FROM** Invoice  
**WHERE** Total **BETWEEN** .50 **AND** 9.99;

## Filtering on Text Columns

_Get all information that meets one criteria.  
_**SELECT** *  
**FROM** Customer  
**WHERE** Country **=** ‘Germany’ ;

_Get all information that meets two criteria inclusively.  
_**SELECT** *  
**FROM** Customer  
**WHERE** Country **=** ‘Germany’ **OR** Country **=** ‘France’ ;

_Get all information excluding one criteria.  
_**SELECT** *  
**FROM** Customer  
**WHERE NOT** Country **=** ‘USA’ ;

_Get information where criteria starts with something.  
_**SELECT** FirstName, LastName  
**FROM** Customer  
**WHERE** LastName **LIKE** ‘A**%**’ ;

_Get information where criteria skips one (or more) characters.  
_**SELECT** FirstName, LastName  
**FROM** Customer  
**WHERE** LastName **LIKE** ‘**_**e**%**’ ;

_Get information where criteria does not end with something.  
_**SELECT** FirstName, LastName  
**FROM** Customer  
**WHERE** LastName **NOT** **LIKE** ‘**%**A’ ;

## Filtering on Multiple Columns

_Get all information that meets criteria from two columns.  
_**SELECT** CustomerId, Total  
**FROM** Invoice  
**WHERE** Total > 10 **AND** BillingCity = ‘Paris’;

## Filtering on Missing Data

_Get all information that meets criteria from two columns.  
_**SELECT** CustomerId  
**FROM** Invoice  
**WHERE** Total **IS NOT NULL** ;

  

  

# TRANSFORMING DATA

  

●      **UPPER/LOWER**: Converts text case

●      **CONCAT**: Joins text together

●      **SUBSTRING**: Extracts parts of text

●      **ROUND**: Rounds to a specified number of decimal places

●      **SUM**: Adds values for total calculation

●      **AVG**: Calculates mean average of data

●      **COUNT**: Adds the number of rows

●      **MIN/MAX**: Identifies smallest/largest number.

●      **FORMAT**: Displays components as specified

●      **GETDATE**: Retrieves current date/time

## String Data

_Convert text data to upper case letters._  
**SELECT UPPER**(FirstName)  
**FROM** Customer ;

_Join text to create composite information.  
_**SELECT CONCAT**(FirstName, ‘ ‘ , LastName)  
**FROM** Customer ;

_Extract parts of a text string (e.g. 1st through 4th characters)._  
**SELECT SUBSTRING**(LastName, 1, 4)  
**FROM** Customer ;

## Mathematical Functions

_Round to the nearest two decimal places.  
_**SELECT ROUND**(Total, 2)  
**FROM** Invoice ;

_Add values in a set of numbers.  
_**SELECT SUM**(Total)  
**FROM** Invoice  
**WHERE** CustomerId = 5 ;

_Calculate the mean average of a set of numbers.  
_**SELECT AVG**(Total)  
**FROM** Invoice ;

## Date and Time Functions

_Display date in mm-dd-yyyy format.  
_**SELECT FORMAT**(InvoiceDate, ‘MM-dd-yyyy’)  
**FROM** Invoice ;

_Get the current date displayed in mm-dd-yyyy format.  
_**SELECT FORMAT (GETDATE()**, ‘MM-dd-yyyy’) ;

_Calculate the difference between two dates.  
_**SELECT DATEDIFF**(day, ‘01-01-2022’, ‘01-01-2023’) ;

## Aggregation Functions

_Add up the number of records returned.  
_**SELECT COUNT**(InvoiceId)  
**FROM** Invoice  
**WHERE** Total > 10

_Identify the smallest value in a dataset.  
_**SELECT MIN**(Total)  
**FROM** Invoice ;

_Identify the largest value in a dataset.  
_**SELECT MAX**(Total)  
**FROM** Invoice ;

  

# JOINING TABLES![](file:///C:/Users/Diogo/AppData/Local/Packages/oice_16_974fa576_32c1d314_bf8/AC/Temp/msohtmlclip1/01/clip_image002.gif)

_INNER JOIN: Return rows with a match in both tables  
(no null values)  
_**SELECT  
**            Customer.CustomerId,  
            Customer.LastName,  
            Invoice.InvoiceId  
**FROM** Customer  
**JOIN** Invoice **ON** Customer.State = Invoice.BillingState ;![](file:///C:/Users/Diogo/AppData/Local/Packages/oice_16_974fa576_32c1d314_bf8/AC/Temp/msohtmlclip1/01/clip_image003.gif)

_LEFT/RIGHT JOIN: Return all rows in the left or right table, and only matching rows in the other table  
(null values may be present from the table with all rows)  
_**SELECT  
**            Customer.CustomerId,  
            Customer.LastName,  
            Invoice.InvoiceId  
**FROM** Customer  
**LEFT JOIN** Invoice **ON** Customer.State = Invoice.BillingState ;![](file:///C:/Users/Diogo/AppData/Local/Packages/oice_16_974fa576_32c1d314_bf8/AC/Temp/msohtmlclip1/01/clip_image004.gif)

_FULL OUTER JOIN: Return all rows in both tables  
(null values may be present from either table)  
_**SELECT  
**            Customer.CustomerId,  
            Customer.LastName,  
            Invoice.InvoiceId  
**FROM** Customer  
**FULL OUTER JOIN** Invoice **ON** Customer.State = Invoice.BillingState ;![](file:///C:/Users/Diogo/AppData/Local/Packages/oice_16_974fa576_32c1d314_bf8/AC/Temp/msohtmlclip1/01/clip_image005.gif)

# USING ALIASES

_COLUMN NAMES: Clarify a new column in the results which is created by concatenating two separate columns._

**SELECT CONCAT**(FirstName, ‘ ‘ , LastName) **AS** FullName  
**FROM** Customer ;

_TABLE NAMES: Simplify the readability of a complex query by assigning aliases to tables._

**SELECT** c.CustomerId, c.FirstName, c.LastName, i.Total **AS** InvoiceTotal  
**FROM** Customer c

**JOIN** Invoice i **ON** c.CustomerId = i.CustomerId ;