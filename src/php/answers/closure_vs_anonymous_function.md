# Closure vs anonymous function

Both of them use class [Closure](https://www.php.net/manual/ru/class.closure.php) under the hood.

In PHP, the main difference between a closure and an anonymous function is that closure takes arguments:

```php
<?php

$someVar = 12345;

//closure
$result1 = function() use ($someVar) {
    return $someVar;
};

//anonymous function
$result2 = function () {
    return '123';
};
```
