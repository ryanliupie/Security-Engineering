<h3><u>Cross Site Request Forgery (CSRF):</u></h3>

This is a type of attack that forces a user on a web application to do unwanted tasks while they are authenticated. With the help of social engineering, they can trick a user to execute actions on the web browser such transferring funds, chaining their email address, and much more, especially if the user is the admin, the threat actor can compromise the entire web application. 

When a user is logged into a web browser, a session is created and the server assigns it unique ID. This ID is stored on the user's device as a session. It helps hold or remember information about that user's sessions next time they visit. 

Since the user is already authenticated, the web browser cannot tell what request is malicious or not. Let's take a look at an example: 

1. You are logged into TD bank. 
2. You look in Gmail and see that you own "$600" to TD bank and you get curious and click the link.  
3. This link is malicious that is has a hidden code that actually transfers that "$600" to the malicious actor's bank account. 
4. This action was allowed since TD bank received this request with the valid login and session cookies. 

<hr>





