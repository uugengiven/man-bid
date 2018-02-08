## Introduction to Relational Databases ##

*SQL* or Simple Query Language is a common language that is used to perform actions on a database server. Microsoft has a SQL based database server that they call MSSQL. This is often shortened to SQL because Microsoft likes confusing names for things. In this course I will use SQL to mean the language and MSSQL to mean the server software by Microsoft.

MSSQL is a server that hosts relational databases. A relational database is normally a set of data broken into tables. These tables often have relationships between them. An example would be a database that holds a holiday card list for a company.

This list would need to contain things such as the name of the person receiving the card, the address the card is going to, perhaps a specific salutation. There are several ways that this data could be stored; something as easy as a text file, or something closer to a database like an Excel file.

This data may look something like this:

| Name | Salutation | Address |
| ---- | ---------- | ------- |
| John Lange | Mr. | 123 South Street |
| James Smith | Dr. | 432 North Drive |

This is a very simple table and could be stored, as is, in MSSQL. A user could then use SQL to query the database and get this data back from the server.

`SELECT * FROM [People]`

This would return all of the columns of the table and all of the rows in the table. SQL then provides a way of reducing the number of columns retrieved (maybe we only need names, no address or salutation) or reducing the number of rows returned (maybe we only need a list of all Doctors using the salutation).

`SELECT Name FROM [People]` will return a list of all of the names in the table.

`SELECT * FROM [People] WHERE Salutation='Dr.'` will return all of the information on every row where the salutation is Dr.

> Provide a connection to a database with a simple Contoso style database and have the students try different select statements from a single table. Have them combine choosing specific columns and rows.

While this initial table is pretty useful, we may have a list that includes international people. Or possibly multiple people within the same address, and we want to send each of them a card. But, we don't want to copy that same information over and over again. So what we may do is something like this:

### Address Table ###
| ID | Street1 | Street2 | Zip | State | Country |
| -- | ------- | ------- | --- | ----- | ------- |
| 1 | 123 South Street | | 90210 | CA | USA |
| 2 | 432 North Drive | Box 4 | ADU 232 | Glenburry | England |

### People Table ###
| Name | Salutation | AddressID |
| ---- | ---------- | --------- |
| John Lange | Mr. | 1 |
| Jean Lange | Mrs. | 1 |
| James Smith | Dr. | 2 |

This has separated the sets of data, so there is less potential duplication of data. This causes other potential issues (what is John moves but Jean doesn't?) but the benefits often outweigh those costs.

For us to select a list from these two tables requires us to join the two tables together. As you can see in the two tables, the Address table has an ID field and the People table has a matching AddressID field. We can use SQL to retrieve data from one table and attach the extra information from another table.

`SELECT * FROM [People] JOIN [Address] on [People].AddressID = [Address].ID`

This will result in each row from the People table to be retreived, but at the end of each row, the matching row from the Address table will be added on. Our output from the MSSQL server will appear like it has duplicate information, since two of the people have the same address, but within the database itself, it still only stores that address once.

Also, notice that this SELECT statement still has an area for which columns to select. You can also add a WHERE statement to reduce the number of rows returned. A JOIN statement is a modifier to a SELECT statement and does not prevent any other modifiers (WHERE, GROUP, UNION, ORDER BY) from being used.

Reading through two tables and outputting a lot of extra data does add some extra processing to the SQL request, but SQL servers have been very optomized over the years to be fast on these exact kind of requests.

> Have students connect to a Contoso style database with linked tables. Have them practice doing SELECT statements both with and without JOIN options. Have them try to do a SELECT and JOIN two other tables instead of just one extra.

