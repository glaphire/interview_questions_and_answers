# PHP 8.0 new features

[Documentation: PHP 8.0 release](https://www.php.net/releases/8.0/en.php)

## Main new Features

1. *Named arguments*
- specifying only required parameters
- arguments are order-independent and self-documented

```php
htmlspecialchars($string, double_encode: false);
```

2. *Attributes*
```php
class PostsController
{
	#[Route("/api/posts/{id}", methods: ["GET"])]
	public function getId($id)
	{ 
		/**/
	}
}
```

3. *Constructor property promotion*
```php
class Point
{
	public function __construct(
		public float $x = 0.0,
		public float $y = 0.0,
		public float $z = 0.0,
	) {}
}
```
4. *Union types*
```php
class Number {
  public function __construct(
    private int|float $number
  ) {}
}
```

5. *Match expression* - partial replacement for switch() with strict comparisons
```php
echo match (8.0) {
  '8.0' => "Oh no!",
  8.0 => "This is what I expected",
};
//> This is what I expected
```

6. *Nullsafe operator ?->* - When the evaluation of one element in the chain fails, 
the execution of the entire chain aborts and the entire chain evaluates to null.                        

```php
$country = $session?->user?->getAddress()?->country;
```

7. *Saner string to number comparison*
```php
0 == 'foobar' // false
```

8. *Consistent type errors for internal functions*
```php
strlen([]); // TypeError: strlen(): Argument #1 ($str) must be of type string, array given

array_chunk([], -1); // ValueError: array_chunk(): Argument #2 ($length) must be greater than 0
```

9. *JIT compilation* - two JIT compilation engines (Tracing and Function) introduced to 
increase PHP performance.

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