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
- iterables
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

