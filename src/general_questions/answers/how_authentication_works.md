# Authentication
Authentication - verification process of a user's identity.

Authentication usually may be session/cookie-based (for Web) or token-based (for APIs).

## Registration flow (Web flow)
1. User opens registration page in a browser with a form.
2. User fills required fields in a form: usually identification field (username/email etc.) and password.
3. User clicks 'submit'.
4. Browser sends HTTP POST request to the server with plain form's data (and CSRF token, if implemented).
5. Server receives the request, checks CSRF token. If a token is valid, the next steps are done:

   1. Identification field is validated (for uniqueness and string-related rules).
   2. Password is validated (length, complexity).
   3. If validation errors have been found, 
   the server sends a response to the browser with HTTP 400 error and a list of validation errors. 
   User stays unauthorized.
   4. If validation was successful, 
   password is been hashed by one of suitable hashing algorithms
    ([crypt()](https://www.php.net/manual/ru/function.crypt.php) 
    or [password_hash()](https://www.php.net/manual/ru/function.password-hash.php) functions).
   5. Identification field and hashed password are saved in a database.
6. Server sends HTTP response with status 200 with successful registration message. Usually, it redirects to a login page.

## Login flow (Web flow)
1. User opens login page in a browser with form.
2. User fills identification field (username/email etc.) and plain password, clicks submit.
3. Browser sends HTTP POST request with form data to the server.
4. Server receives and parses HTTP request, gets plain identification field and password.
5. Server makes request to the database, checks if user with this identification field exists.
    1. If user doesn't exist, server sends HTTP 400 response with an error.
    2. If user exists, server hashes password and compares with user's password in the database.
    3. If password hashes don't match, server sends HTTP 400 response with an error.
6. After successful check server starts new Session. 
7. Server sends response to browser with a "Set-cookie" header with session id. Browser saves it.
On each request browser adds 'Cookie' header and server compares session id with cookie id.
There are two approaches for authentication lifetime:
    1. Session-based authentication: when cookie lifetime is not set or is relatively small (less than 24 minutes), session lives only while browser is opened.
    2. Cookie-based authentication: session id is stored in the database or key-value storage, 
    cookie lifetime is set and relatively long. It allows to remain user login in after browser is closed 
    and session is destroyed ("remember me" functionality). After opening the browser and returning to a page,
    browser sends cookie header with id, and compares it with session id in storage - 
    if they are the same, user remains authenticated until cookie expires/is erased or storage is cleaned out.
  
[Stackoverflow answer](https://stackoverflow.com/a/9325698/5371978)

##Logout flow (Web flow)
(TODO)

##Login flow (API flow)
(TODO)

##Logout flow (Web flow)
(TODO)