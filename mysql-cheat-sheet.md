# MySQL Cheat Sheet

This document is a simple reference for MySQL statements and functions.
It assumes a fundamental understanding of MySQL databases, tables, columns, and their data types.

## How to login into MySQL from terminal

Once logged into your terminal, the following command can be entered to login to MySQL:

`mysql -u username -p`

Replace 'username' with your own MySQL username, and enter your password as prompted. 

## How to SHOW USERS

After logging into MySQL, we'd execute the following command to show existing databases:

`SHOW DATABASES;`

NOTE: don't forget the semi-colon at the end of your MySQL commands!

The above command will displays a list of databases. To access the user list, we will first need to select the appropriate database:

`USE mysql;`

We're now using the right database. Now we can use this next command to view the list of tables within:

`SHOW TABLES;`

In this list there should be a table called 'user'. To view the list of users within it, we will run this command:

`SELECT User FROM user`

What this will show is a list if all data in the User column (by way of the SELECT statement) which is part of the user table (by way of the FROM condition) in the MySQL database. We could also add 'Password' to the query in order to see each User's password credentials at the same time. 

## How to CREATE USERS

Considering the previous step, this is how we'd go about adding a new User with a Password to the user table in the MySQL database. Assuming we're already using the correct database:

`CREATE USER 'new_user'@'local_host' IDENTIFIED BY 'password';`

This creates a new user called 'new_user' on the 'local_host' server (aka the local machine). The 'password' can be entered as chosen during the execution of this command.

## How to GRANT PRIVILEGES, Show granted privileges and remove.

#### Granting Privileges
In order to grant specific privileges (as shown by 1 and 2 in this example) to a specific user for a specific database and table (named as such for the following examples) on our server, we'd do this:

`GRANT privilege1, privilege2 ON database.table TO 'username'@'local_host';`

If we alternatively wanted to grant full access in the way of global privileges to an admin user for example, we'd do this instead:

`GRANT ALL PRIVILEGES ON *.* TO 'username'@'local_host' WITH GRANT OPTION;`

It's good practice to run a `FLUSH PRIVILEGES;` command after creating a new user or altering privileges. This will ensure the newly modified privileges are reloaded and are in effect.  

#### Showing Granted Privileges

This command is simple and pretty self-explanatory:

`SHOW GRANTS FOR 'username'@'local_host';`

#### Revoking Privileges

The same way we granted specific privileges to users above, we can revoke them:

`REVOKE privilege1, privilege2 ON database.table FROM 'username'@'local_host';`

If we otherwise wanted to revoke all privileges from a user:

`REVOKE ALL, GRANT OPTION FROM 'username'@'local_host';`

Good idea to `FLUSH PRIVILEGES;` here again!


## How to SHOW, DELETE & CREATE DATABASES & SELECT DATABASES

To show all databases:

`SHOW DATABASES;`

To create a new database:

`CREATE DATABASE databasename;`

To delete an existing database:

`DROP DATABASE newdatabasename;`

To access/select and use a database:

`USE databasename`

## How to CREATE a TABLE with Columns and their data types

Assuming we're using a database to begin with, this is how we'd creat a new table inside of it:

```
CREATE TABLE tablename (
    columnName1 datatype,
    columnName2 datatype,
    columnName3 datatype
);
```

Important to note the different types of data that we can designate to columns when creating them. As identified by 'datatype' in the example above, we'd otherwise use the appropriate shorthand for these datatypes. For example, `INT` would be used for numerical data, `VARCHAR(15)` would be used for a string wherein the number in parantheses determines the max length of said string, `DATE` would naturally be stored as a date in YYYY-MM-DD format. There are many other possible data types but these are simple and easily understandable examples.

## How to SHOW, DELETE & DROP Tables

Again assuming we've already accessed a database, we can show the tables within like this:

`SHOW TABLES;`

To delete the data inside a specific table, but not the table itself:

`TRUNCATE TABLE tablename;`

To delete a table in its entirety:

`DROP TABLE tablename;`

## How to Insert ROWS & RECORDS (single and multiple)

Once a table and its columns have been created, here's how we'd select a table and add information into rows:

```
INSERT INTO tablename (column1, column2) 
VALUES (value1, value2);
```

To add data to multiple rows at once, we'd simply introduce a comma in between the sets of data being entered into each row as seen here:

```
INSERT INTO tablename (column1, column2) 
VALUES (value1, value2), (value3, value4), ...;
```

The order in which the values are entered corresponds to the order of the columns. Strings should be entered in quotes, integers without. 

## How to SELECT with the WHERE Clause

The 'WHERE' clause is used to narrow down query paramaters by introducing a condition for the query to search for and match.

`SELECT * FROM tablename WHERE Name = 'John';`

In the example here, the condition asks for the results of our query to sift through a column titled 'Name' and return every item of data that matches the term 'John'.

There are several operators we can add to our WHERE queries in order to achieve more precise or specific results. Here's a list of some of them with examples:

```
OPERATOR        DESCRIPTION	         EXAMPLE
   =	        Equal:	                 WHERE id = 2
   >	        Greater than:	         WHERE age > 30
   <	        Less than:	         WHERE age < 18
   >=	        Greater than or equal:	 WHERE rating >= 4
   <=	        Less than or equal: 	 WHERE price <= 100
   LIKE	        Pattern matching:	 WHERE name LIKE 'Dav'
   IN	        Check whether a specified value matches any value in a list or subquery:   WHERE country IN ('USA', 'UK')
   BETWEEN	Check whether a specified value is within a range of values:	WHERE rating BETWEEN 3 AND 5
```

## How to SELECT with the WHERE Clause using a range

We can take build off of this and add a range parameter to our query for more specificity:

`SELECT * FROM tablename WHERE Age BETWEEN 20 AND 30;`

In this example our query will return all results from the Age column where the age falls in between the set range of 20 and 30.

## How to DELETE an individual ROW

In order to delete a single row from a table we would set a condition:

`DELETE FROM tablename WHERE Name = 'John';`

NOTE: If there are multiple rows that match the specified query, they will all be deleted. In this case, any row that has the name 'John' in the name column will be affected, so using the appropriate condition for deletion is important. 

## How to UPDATE a ROW

In order to update the contents of a row, we'd pair off the columns with the new data values we are looking to update:

`UPDATE tablename SET column1 = value1, column2 = value2 WHERE condition;
`

Note that we can specify or query by adding a condition, as seen in the previous examples above (WHERE Name = 'John').

## How to add a new column and modify it

To add a new column to an existing table:

`ALTER TABLE tablename ADD newColumnName datatype;`

And to modify it afterwards:

`ALTER TABLE tablename MODIFY newColumnName new_datatype;`

## How to Order by and use Distinct

We can use queries to show results in order. In the following example, we are pulling the data from the 'Population' column in the 'City' table, and showing it in ascending order (ASC = ascending, DESC = descending):

`SELECT Population FROM City ORDER BY Population ASC;`

If a table holds duplicate values in its column data, using the DISTINCT statement will allow us to isolate and display exclusively the single/distinct values. For example, here's how we'd select and show all the different customers in a table of orders:

`SELECT DISTINCT Customers FROM Orders;`

This table could have 25 rows of orders, but only 10 different customers. This query will show us those 10 distinct customers. 


## How to Concatenate Columns

To concatenate columns, we can use the CONCAT() function as seen in the following example:

`SELECT CONCAT(column1, column2) FROM tablename;`

This will join the data from both columns into a single string. It's also possible to add a space between the data in our concatenation by adding quote marks, as seen here:

`SELECT CONCAT(column1, ' ', column2) AS combined_column FROM tablename;`

A more practical example would be to think of concatenating first name and last name data from two columns, resulting in a 'combined_column' of a 'full_name'.

## How to Select Distinct Rows

As seen previously, the DISTINCT keyword returns all unique/individual data items from rows in a table. Here's the basic syntax:

`SELECT DISTINCT columnName FROM tablename;`

A practical example would be to think of a list of a table containing a list of teachers at a college as well as their respective faculties. We could SELECT DISTINCT the faculties as our 'columnName' and the query would return a list of those unique faculties.

## How to use LIKE to Search

Using the LIKE operator in MySQL queries allows us a great deal of precision in narrowing down search results. Here's an example of the basic syntax:

`SELECT columnName FROM tablename WHERE columnName LIKE pattern;`

Here's a list of several example 'wildcards' we can use with this syntax:

```

LIKE OPERATOR SYNTAX	     ->               DESCRIPTION
WHERE CustomerName LIKE 'a%' -> Finds any values that start with "a"
WHERE CustomerName LIKE '%a' -> Finds any values that end with "a"
WHERE CustomerName LIKE '%or%' -> Finds any values that have "or" in any position
WHERE CustomerName LIKE '_r%' -> Finds any values that have "r" in the second position
WHERE CustomerName LIKE 'a__%' -> Finds any values that start with "a" and are at least 3 characters in length
WHERE CustomerName LIKE 'a%o' -> Finds any values that start with "a" and ends with "o"

```

In the examples above, the % symbol will represent zero or more characters. The _ symbol will represent a single character. 

## How to Select using IN

The IN operator allows us to specify more than one value in a WHERE clause, which is useful when we want to select rows in a table and match multiple values from a column:

`SELECT columnName FROM tablename WHERE columnName IN (value1, value2, value3);`

To expand on an earlier example, say we wanted to return a list of teachers from the Law, Medicine, and WebDev faculties at a college. We could insert those values in the IN operator parentheses and we'd get a list of all teachers from multiple faculties. It would look something like this:

`SELECT * FROM teachers WHERE faculty IN ('Law','Medicine','WebDev');`

We can also use integers/numbers as values.  

## How to Create & Remove Index

#### Creating a new index

An index in a MySQL database functions the same way in index in a book does. As tables and databases get larger, using indexes helps maintain speedy data retrieval as it allows a user to query data without having to scan an entire table. Here's an example of how to create an index:

`CREATE INDEX indexName ON tablename (column1, column2, column3);`

The columns in parentheses are the ones to be indexed; a composite index is one made up of multiple columns. Here's a practical example showing how we'd create a new index for a list of employees' last names, using just a single column:

`CREATE INDEX idx_lastName ON employees (lastName);`

#### Removing an index

In order to remove an existing index, we'd simply use the DROP keyword to do so:

`DROP INDEX idx_lastName ON employees;`

## Create Two Tables demonstrating a one-to-many relationship that shows a PK & FK, and how to use Inner Join as well as how to JOIN Multiple Tables

#### One-to-Many Table Relationship

A simple example demonstrating a one-to-many relationship between tables would be to think of the following: a first table representing a list of customers, and a second table representing a list of orders made by those customers. Each customer would be assigned a unique ID, but each individual customer might be responsible for multiple orders, therefore establishing a one-to-many relationship from the customers table to the orders table. In other words, several orders could be linked to just one costumer. Here's how we might set those tables up:

```
CREATE TABLE customers (
    customer_id INT AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100),
    PRIMARY KEY (customer_id)
);

CREATE TABLE orders (
    order_id INT AUTO_INCREMENT,
    order_date DATE,
    amount DECIMAL(10, 2),
    customer_id INT,
    product_id INT,
    PRIMARY KEY (order_id),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

```

Note: the product_id value will become apparent later in this section when we introduce a third table to join.

#### Using INNER JOIN

Now let's say we want to combine data from both the Customers and Orders tables in order to view a detailed list of orders alongside customer specifics. We would use an INNER JOIN to put this data together:

```
SELECT customers.name, orders.order_date, orders.amount
FROM orders
INNER JOIN customers ON orders.customer_id = customers.customer_id;
```

Note that we specify table columns by including the table names in our syntax, and use a period to specify the column we're looking to use in our JOIN (customers.name = customers table, name column). The query above joins our two tables based on the customer_id (which is a Primary Key in the customers table, and a Foreign Key in the orders table) and will show the customer name along with the details of any associated orders. 

#### Joining Multiple Tables

Suppose we were to add a third table; in the case of this example, a Products table:

```
CREATE TABLE products (
    product_id INT AUTO_INCREMENT,
    name VARCHAR(100),
    price DECIMAL(10, 2),
    PRIMARY KEY (product_id)
);
```

Here's how we'd go about using INNER JOIN to pull and combine information from a third table in order to get a fully detailed dataset containing detailed information on a customer and any products they have ordered:

```
SELECT customers.name AS CustomerName, 
       orders.order_date AS OrderDate, 
       products.name AS ProductName, 
       orders.amount AS OrderAmount
FROM orders
INNER JOIN customers ON orders.customer_id = customers.customer_id
INNER JOIN products ON orders.product_id = products.product_id;

```

