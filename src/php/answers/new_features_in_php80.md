# PHP 8.0 new features

[Documentation: PHP 8.0 release](https://www.php.net/releases/8.0/en.php)

## Main new Features

1. Named arguments
- specifying only required parameters
- arguments are order-independent and self-documented

```php
htmlspecialchars($string, double_encode: false);
```

2. Attributes

3. Constructor property promotion

4. Union types

5. Match expression

6. Nullsafe operator ?->

7. Saner string to number comparison

8. Consistent type errors for internal functions

9. JIT compilator

## New  classes and functions

- WeakMap class - allows objects to be garbage collected to prevent leaks;
- Stringable interface (allows to force implement __toString);
- PhpToken class - replaces token_get_all() and implements new functionality;
- updated DOM API;
- string_contains(), str_starts_with(), str_ends_with() - more convenient that strpos and strstr; 
- fdiv() - floating-point division;
- get_debug_type() - replaces checks is_object($bar) ? get_class($bar) : gettype($bar);
- get_resource_id() - replaces (int)$resource;

## Type system and error handling improvements
1. Stricter type checks for arithmetic/bitwise operators: throw TypeError when arithmetic/bitwise operators
are used on non-scalar types
2. Abstract trait method validation: 
3. Correct signatures of magic methods:
4. Reclassified engine warnings:
5. Fatal error for incompatible method signatures
6. The @ operator doesn't suppress fatal errors anymore
7. Inheritance with private methods
8. Mixed type
9. Static return type
10. Types of internal functions
11. Opaque objects instead of resources for extensions
* CURL
* GD
* Sockets
* OpenSSL
* XMLWriter
* XML 

## Other syntax tweaks and improvements

1. Trailing comma in parameter lists and closure use lists
2. Non-capturing catches
3. Variable syntax tweaks
4. Treat namespaced names as single token
5. Throw becomes an expression
6. Allow::class on objects 