# Authentication
Authentication - verification process of a user's identity.

## Registration flow (Web flow)
1. User opens registration page in a browser.
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
//TODO