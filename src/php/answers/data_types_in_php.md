# Data types in PHP. Describe Resource and Callable types

[Documentation](https://www.php.net/manual/en/language.types.php)

__Scalar types__:
- int (integer)
- string
- bool (boolean)
- float

__Compound types__:
- array
- object
- iterables (pseudo-type)
- callback/callables

__Special types__:
- resource
- null

## Scalar types

```php
<?php

///int
$a = 1234; // decimal number
$a = 0123; // octal number (equivalent to 83 decimal)
$a = 0x1A; // hexadecimal number (equivalent to 26 decimal)
$a = 0b11111111; // binary number (equivalent to 255 decimal)
$a = 1_234_567; // decimal number (as of PHP 7.4.0)

///string
$a = 'this is a simple string';
$a = "this is a {$a} quoted string\n"; //can parse variables and escape sequences 
//HEREDOC syntax, expanding variables is possible
echo <<<EOT
bar
EOT;
//NOWDOC syntax, no escaping and variables parsing
echo <<<'EOD'
Example of string spanning multiple lines
using nowdoc syntax. Backslashes are always treated literally,
e.g. \\ and \'.
EOD;

///bool
$a = True; //is case-insensitive
$a = false;

///float
$a = 1.234; 
$b = 1.2e3; 
$c = 7E-10;
$d = 1_234.567; //for php 7.4.0+

```

__Notes__:

* Bool

	When converting to bool, the following values are considered false:
	
	- the boolean false itself
	- the integers 0 and -0 (zero)
	- the floats 0.0 and -0.0 (zero)
	- the empty string, and the string "0"
	- an array with zero elements
	- the special type NULL (including unset variables)
	- SimpleXML objects created from attributeless empty elements, i.e. elements which have neither children nor attributes.

## Compound types

```php
<?php
///arrays
$array = array( //for earlier PHP versions
    "foo" => "bar",
    "bar" => "foo",
);

// Using the short array syntax 
$array = [
    "foo" => "bar",
    "bar" => "foo",
];

///objects

//casting array to object
$obj = (object)array('1' => 'foo');
var_dump(isset($obj->{'1'})); // outputs 'bool(true)' as of PHP 7.2.0; 'bool(false)' previously
var_dump(key($obj)); // outputs 'string(1) "1"' as of PHP 7.2.0; 'int(1)' previously

//casting scalar to object
$obj = (object) 'ciao';
echo $obj->scalar;  // outputs 'ciao'

//or create class and instantiate it

///iterables
function foo(iterable $iterable) {
    foreach ($iterable as $value) {
        // ...
    } 
}

///callables

// An example callback function
function my_callback_function() {
    echo 'hello world!';
}

// An example callback method
class MyClass {
    static function myCallbackMethod() {
        echo 'Hello World!';
    }
}

// Type 1: Simple callback
call_user_func('my_callback_function');

// Type 2: Static class method call
call_user_func(array('MyClass', 'myCallbackMethod'));

// Type 3: Object method call
$obj = new MyClass();
call_user_func(array($obj, 'myCallbackMethod'));

// Type 4: Static class method call
call_user_func('MyClass::myCallbackMethod');

// Type 5: Relative static class method call
class A {
    public static function who() {
        echo "A\n";
    }
}

class B extends A {
    public static function who() {
        echo "B\n";
    }
}

call_user_func(array('B', 'parent::who')); // A

// Example with closure
$double = function($a) {
    return $a * 2;
};

// This is our range of numbers
$numbers = range(1, 5);

// Use the closure as a callback here to
// double the size of each element in our
// range
$new_numbers = array_map($double, $numbers);


```

__Notes__:

* Arrays
	The key can either be an **int** or a **string**. The value can be of any type.

	Additionally the following key casts will occur:

	- Strings containing valid decimal ints, 
	unless the number is preceded by a + sign, will be cast to the int type. E.g. the key "8" will actually be stored under 8. On the other hand "08" will not be cast, as it isn't a valid decimal integer.
	- Floats are also cast to ints, which means that the fractional part will be truncated. 
	E.g. the key 8.7 will actually be stored under 8.
	- Bools are cast to ints, too, i.e. the key true will
	 actually be stored under 1 and the key false under 0.
	- Null will be cast to the empty string, i.e. the key null will actually be stored under "".
	- Arrays and objects can not be used as keys. 
	Doing so will result in a warning: Illegal offset type.
	
* Objects
	- If an object is converted to an object, it is not modified. 
	- If a value of any other type is converted to an object, 
	a new instance of the stdClass built-in class is created. 
	If the value was null, the new instance will be empty. 
	An array converts to an object with properties named 
	by keys and corresponding values. 
	- Note that in this case before PHP 7.2.0 numeric keys have been inaccessible unless iterated.
	- For any other value, a member variable named scalar will contain the value.
	
* Iterables
	Iterable is a pseudo-type introduced in PHP 7.1. 
	It accepts any array or object implementing the Traversable interface. 
	Both of these types are iterable using foreach and can be used with yield from within a generator.
	Iterable can be:
	- as a parameter type
	- as a return type
	
* Callables
	Callbacks can be denoted by the callable type declaration.
    
    Some functions like call_user_func() or usort() 
    accept user-defined callback functions as a parameter. 
    Callback functions can not only be simple functions, 
    but also object methods, including static class methods.
    
    - Apart from common user-defined function, 
    anonymous functions and arrow functions can also be passed to a callback parameter.
    - any object implementing __invoke() can also be passed to a callback parameter.