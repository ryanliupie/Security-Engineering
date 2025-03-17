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

In this case, we are asking if there are any users with an empty string, hence ' '. The important manipulation part is the `OR 1=1`. This is saying "Is there a user with an empty string or does 1 equal the value of 1?. That is True, 1 is equal to 1, so I will let you access". Now the `--' AND password = ''` is commented out because of the `--` which is saying "We don't need the password anymore". 

Mind you, this is not neccesarily granting you access to the website via UI such as logging into your banking app and seeing the purchases you have made. It is granting access to the backend database that powers the application. 