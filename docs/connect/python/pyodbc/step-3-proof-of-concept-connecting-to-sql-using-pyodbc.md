---
title: "Step 3: Proof of concept connecting to SQL using pyodbc | Microsoft Docs"
ms.custom: ""
ms.date: "10/09/2019"
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ""
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4bfd6e52-817d-4f0a-a33d-11466e3f0484
author: MightyPen
ms.author: genemi
---
# Step 3: Proof of concept connecting to SQL using pyodbc

This example should be considered a proof of concept only.  The sample code is simplified for clarity, and does not necessarily represent best practices recommended by Microsoft.  

**Run sample script below**  Create a file called test.py, and add each code snippet as you go. 

```
> python test.py
```
  
## Step 1:  Connect  
  
```python

import pyodbc 
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'tcp:myserver.database.windows.net' 
database = 'mydb' 
username = 'myusername' 
password = 'mypassword' 
cnxn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()

```  
  
  
## Step 2:  Execute query  
  
The cursor.executefunction can be used to retrieve a result set from a query against SQL Database. This function essentially accepts any query and returns a result set which can be iterated over with the use of cursor.fetchone()
  
  
```python
#Sample select query
cursor.execute("SELECT @@version;") 
row = cursor.fetchone() 
while row: 
    print(row[0])
    row = cursor.fetchone()

```  
  
## Step 3:  Insert a row  
  
In this example you will see how to execute an [INSERT](../../../t-sql/statements/insert-transact-sql.md) statement safely, pass parameters which protect your application from [SQL injection](../../../relational-databases/tables/primary-and-foreign-key-constraints.md) value.    
  
  
```python

#Sample insert query
cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New 20', 'SQLEXPRESS New 20', 0, 0, CURRENT_TIMESTAMP )") 
cnxn.commit()
row = cursor.fetchone()

while row: 
    print 'Inserted Product key is ' + str(row[0]) 
    row = cursor.fetchone()
```  

## Azure Active Directory (AAD) and the connection string

pyODBC uses the Microsoft ODBC driver for SQL Server.
If your version of the ODBC driver is 17.1 or later, you can use the AAD interactive mode of the ODBC driver through pyODBC.
This AAD interactive option works if Python and pyODBC allow the ODBC driver to pop up the dialog.
This option is available only on the Windows operating system.

### Example connection string for AAD interactive authentication

Here is an example ODBC connection string that specifies AAD interactive authentication:

- `server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

For details on the AAD authentication options of the ODBC driver, see the following article:

- [Using Azure Active Directory with the ODBC Driver](../../odbc/using-azure-active-directory.md#new-andor-modified-dsn-and-connection-string-keywords)

## Next steps
  
For more information, see the [Python Developer Center](https://azure.microsoft.com/develop/python/).
