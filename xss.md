Friday, 2024-10-25

Author: Changwoo Yu

PART 1: COOKIES
===============

Cookies for cs338.jeffondich.com
--------------------------------

The domain has a cookie called "theme" whose value is "default"

Theme Cookie
------------

Upon changing the theme of FDF, the value of the "theme" field changes to reflect the selected theme color.

Burpsuite and Cookies
---------------------

The "Set-Cookie:" header appears in the HTTP Response header.
```
Set-Cookie: theme=default; Expires=Thu, 23 Jan 2025 16:11:46 GMT; Path=/ 
``` 
The field consists of the cookie name-value pair, expiration dates, and the path specifies when the browser should use the cookie. In this case, we have that Path=/, meaning the cookie will be applied to all of the pages that are contained at the root domain. 

The value of this field changes when the user changes the theme using the drop down menu.

Here, the "theme" is set equal to "default" because I had set the theme to "default."

The "Cookie:" header appears in the HTTP GET request header. 
```
Cookie: theme=red
```
The field consists of cookie(s) name-value pair, which was sent by the web server to the browser. This field only realizes after the web server sets the cookie in the HTTP response. 

Here, the "theme" is set equal to "red" because, I had previously set the theme to "red." 

Cookie Persistence
------------------

After quitting and reopening the browser, the previously selected them is still selected. This means that the theme cookie is a persistent cookie, not a session cookie. 


Cookie Communication
--------------------

### Browser to Web Server

The current theme between the browser and the FDF server is communicated through the HTTP GET request. The current theme, being stored somewhere in the client's local storage that the browser can access, it specified in the "Cookie:" header in the HTTP GET request. Then, the web server can know that what is the current theme. 

### Changing Theme

When you change the theme, the browser first sends a get request. For example, suppose that I have selected blue as my new theme, while my previous theme was red: 
```
GET /fdf/?theme=blue HTTP/1.1
Host: cs338.jeffondich.com
⋮
Cookie: theme=red
⋮
```

Then, the web server responds to the GET request: 
```
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu) 
⋮
Set-Cookie: theme=blue; Expires=Thu, 23 Jan 2025 17:48:44 GMT; Path=/
⋮
```
Notice that the HTTP request's "Set-Cookie" header not sets the theme to blue. 

Inspector and Cookies
---------------------

Using the browser's inspector tool, you can directly change the value of the cookie, and the website will reflect the changes.

Cookie Storage
--------------

The Chrome browser stores the cookies in sqlite3 database name "Cookies" located in
```
/Users/user/Library/Application Support/Google/Chrome/Default/Cookies
```  

PART 2: CROSS-SITE SCRIPTING (XSS)
==================================

Moriarty's attack
-----------------

Moriarty first posts his malicious code using the forum's post functionality. 

The malicious script remains on the forum, awaiting for innocent users to run the code. 

When the user clicks on the malicious post, the malicious script gets executed. 

Depending on the script, the text turns red, or a pop up appears on the user's browser.

Security Implications
---------------------

### Fork Bomb

It is possible to interrupt the client's browsing experience by writing a script that will continuously open unwanted websites

### Malicious Redirections

When the website does not prevent malicious redirections, a user can be directed to a clone of the website, fooling the user to possibly disclose their personal information. 

Preventions
-----------

The server should check if the post contains only the intended formats of data. 

The browser can prevent scripts from automatically executing upon loading the page. Although this would break other functionalities that depends on client-side scripting languages, it will ensure that no malicious codes get executed without user's consent. 
