<h3><u>SQL Injection (SQLi):</u></h3>

A vulnerability that allows an attacker to manipulate SQL queries executed by a relational database. SQL databases store data in structured tables, making them easy to query using Structured Query Language (SQL). If an application fails to properly handle user input, attackers can inject malicious SQL code to gain unauthorized access, modify data, or even delete entire databases.

A normal login query would look like this:  
```sql
SELECT * FROM users WHERE username = 'ryan' AND password = 'Britishqw43$';
``` 

Whenever we try to type our username and password into a website, this is backend query that is occuring. 

A way an attacker can exploit this is by running: 

```sql
SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = ''; 
```

In this case, we are asking if there are any users with an empty string, hence ' '. The important manipulation part is the `OR 1=1`. This is saying "Is there a user with an empty string or does 1 equal the value of 1?. Well, that math does make sense, so I will let you in". Now the `--' AND password = ''` is commented out because of the `--` which is saying "We don't need the password anymore". 

Mind you, this is not neccesarily granting you access to the website via UI such as logging into your banking app and seeing the purchases you have made. It is granting access to the backend database that powers the application. 

Lets take a look at how this would look in Java. Down below, there is some code from OWASP where JDBC (Java Database Connectivity) is communicating with a database. 

```java 
String query = "SELECT account_balance FROM user_data WHERE user_name = "
             + request.getParameter("customerName");
try {
    Statement statement = connection.createStatement( ... );
    ResultSet results = statement.executeQuery( query );
}
```
It is trying to get the account balance of a user based on their customer name input. However, the issues lies with `+ request.getParameter("customerName");`. If a user would  input their name it would return: 

```sql
SELECT * FROM users WHERE name = ryan
```

This is not good because SQL expects the input to return as a string in order to process it. A user can enter `' OR '1'='1` to gain access. You might say "oh just add a quote inside the query". This would work however there is a catch --> 

```sql
String query = "SELECT * FROM users WHERE name = '" + input + "'";

SELECT * FROM users WHERE name = 'ryan OR 1=1'
```
In this, the `'"` opens the string. We add an `input` such as a name and the end the string with `"'"` as a single quote. If an attacker were to try and inject, it would return as a single string. This means SQL is trying to process it but there is no username such as that. It would reject that input which prevents the injection. Keep in mind, the attacker wants to break out of the string when we take a look at the catch --> 

```sql
' OR '1'='1
``` 
The attacker would end the string with a `'`. so it would be `'ryan' OR '1'='1 ` which allows the attacker to end the string, then perform the injection. A way to prevent a SQLi would be using <b>parameterized queries</b> also known as prepared statements: 

```java
String query = "SELECT account_balance FROM user_data WHERE user_name = ? "; // placeholder
PreparedStatement pstmt = connection.prepareStatement( query ); // object that helps Java talk to database safely
pstmt.setString(1, input); // secret!! *
ResultSet results = pstmt.executeQuery( ); // runs query and gives results to database 
```
The `?` is placeholder; it holds the users input, but does not run the query to the database. 

In `pstmt.setString(1, input);`, `1` means the first `?` as you can have `2`, `3` etc.. and the `input` means the value the user typed in association with the number. 

Java says to the database "I'm going to give you a value, it is not SQL code, just <b>plain text</b>". The database would say "Alright, let me plug it in. Hmm, there is nothing related to that, sorry man". Let's rollback and see what happens if an attacker performs a SQLi: 

```sql
' OR '1'='1 -- attackers input

WHERE name = ''' OR ''1''=''1' -- database response
```

Since the database treats it as <b>plain text</b>, the result is a string with a weird name which is not in the database. This rejects the input which protects the database from injection.

---

The second way would be using <b>Stored Procedures</b>. In the database, we can input pre-written SQL code that defines a method without dynamic SQL, then user Java to call it. This helps us reuse the code across platforms, and improve performance by reducing parse and compile time. 

However, companies have to use this carefully as it can be risky. Let's take a look at some secure code: 

```sql
-- SQL 

CREATE PROCEDURE sp_getAccountBalance
    @CustomerName VARCHAR(100)
AS
BEGIN
    SELECT account_balance
    FROM user_data
    WHERE user_name = @CustomerName;
END;
```
This is a safe stored procedure as it is using the parameter "@CustomerName" and it does not use string concatenation or dynamic SQL. 

On the contrary, insecure code would look like this: 

```sql
CREATE PROCEDURE sp_getAccountBalance
    @CustomerName VARCHAR(100)
AS
BEGIN
    DECLARE @query NVARCHAR(MAX);
    SET @query = 'SELECT account_balance FROM user_data WHERE user_name = ''' + @CustomerName + '''';
    EXEC sp_executesql @query;
END;
```
This is not good as the input is merged as a string and it is using dynamic SQL, which means if an attacker enters `' OR '1'='1`, they could break it. 

Now to call the safe procedure, let's take a look at some Java from OWASP: 

```java
// Java

String custname = request.getParameter("customerName"); // gets input from user (input->ryan, custname=ryan)
try {
  CallableStatement cs = connection.prepareCall("{call sp_getAccountBalance(?)}"); //prepares call to prepared statement (? very important to store as plain text/placeholder)
  cs.setString(1, custname);
  ResultSet results = cs.executeQuery(); // runs stored procedure on database
} catch (SQLException se) { // logging and error handling
}
```
---

The third way would be an <b> Allow-list Input Validation.</b> As we know, the main main to protect against a SQLi is by using parameterized queries by passing data as plain text rather than SQL logic. However, we cannot bind table names, table columns, or sort order keywords (ACS or DESC) with `?`. In order to get those values, we would have to use SQL commands, but that database vulnerable to a SQLi. 

To get these values and protect against a SQLi, we can write in Java: 

```java
String tableName; // variable tableName
switch(PARAM): // smarter if-else, PARAM = user input
  case "users": tableName = "users"; //if PARAM = users, safely set tableName = users
                 break;
  case "admins": tableName = "admins"; //if PARAM = admins, safely set tableName = admins
                 break;
  ...
  default      : throw new InputValidationException("unexpected value provided" 
                                                  + " for table name"); 
//input not equal; throws exception
```
The switch statements checks "Does this input match one of my safe options"? If it matches (example-> `users`) `tableName` = `users` so that the user can safely perform a query. 

---

The last way, which is NOT recommended would be <b>Escaping All User Input Manually</b>. This method tries to stop a SQL by "escaping" which simply means adding special characters so that the input is safe. 

For instance, before we learnt that if a software developer were to use a `'` in between the `""` -> `"'"`, an attacker can end the string, or use other characters such as `--`, `#`, `/* */` to get out of the string = succesful SQLi 