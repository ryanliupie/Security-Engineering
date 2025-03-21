## What is Secure Code? 🌐

 Secure coding is the practice of developing software with a strong emphasis on security. By writing secure code, developers can minimize vulnerabilities, reducing the risk of attackers exploiting a system or service. 
 
 Imagine you and your family are going on vacation, but securing your home is not a priority. You may leave doors unlocked, windows open, or even the garage ajar—creating multiple entry points for intruders to steal valuable items. Similarly, insecure code leaves applications exposed to cyber threats.


 This is why writing secure code is essential, especially when handling confidential and sensitive information. Implementing security best practices from the start helps protect systems, data, and users from potential breaches.
<hr>

## Common Vulnerabilites in Code

<b>SQL Injection (SQLi):</b> A vulnerability that allows an attacker to manipulate SQL queries executed by a relational database. SQL databases store data in structured tables, making them easy to query using Structured Query Language (SQL). If an application fails to properly handle user input, attackers can inject malicious SQL code to gain unauthorized access, modify data, or even delete entire databases.

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
In this, the `'"` opens the string. We add an `input` such as a name and the end the string with `"'"` as a single quote. If an attacker were to try and inject, it would return as a single string. This means SQL is trying to process it but there is no username such as that. It would reject that input which prevents the injection. Remember the attacker is trying to break out of string. The catch would be: 

```sql
' OR '1'='1
``` 
The attacker would end the string with a `'`. so it would be `'ryan' OR '1'='1 ` which allows the attacker to break out of the string and gain access. A way to prevent an SQLi would be using <b>parameterized queries</b> also known as prepared statements: 

```java
String query = "SELECT account_balance FROM user_data WHERE user_name = ? "; // placeholder
PreparedStatement pstmt = connection.prepareStatement( query ); // object that helps Java talk to database safely
pstmt.setString(1, input); // secret!! *
ResultSet results = pstmt.executeQuery( ); // runs query and gives results to database 
```
The `?` is placeholder, it holds the users input but does not run the query to the database. 

In `pstmt.setString(1, input);`, `1` means the first `?` as you can have `2`, `3` etc.. and the `input` means the value the user typed in association with the number. 

Java says to the database "I'm going to give you a value, it is not SQL code, just <b>plain text</b>". The database would say "Alright, let me plug it in. Hmm, there is nothing related to that, sorry man". Let's rollback and see what happens if an attacker performs a SQLi: 

```sql
' OR '1'='1 -- attackers input

WHERE name = ''' OR ''1''=''1' -- database response
```

Since the database treats it as plain text, the result is a string with a very weird name that is not in the database. This rejects the input which protects the database from injection. 






