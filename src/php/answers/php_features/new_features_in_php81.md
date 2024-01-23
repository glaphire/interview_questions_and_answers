# PHP 8.1 new features

[Documentation: PHP 8.1 release](https://www.php.net/releases/8.1/en.php)

## Main new Features

1. *Enumerations*
```php
//php8.1<
class Status
{
    const DRAFT = 'draft';
    const PUBLISHED = 'published';
    const ARCHIVED = 'archived';
}
function acceptStatus(string $status) {...}

//php8.1
enum Status
{
    case Draft;
    case Published;
    case Archived;
}
function acceptStatus(Status $status) {...}
```

2. *Readonly Properties*
- Readonly properties cannot be changed after initialization, i.e. after a value is assigned to them.
- They are a great way to model value objects and data-transfer objects.
```php
//php8.1<
class BlogData
{
    private Status $status;
   
    public function __construct(Status $status) 
    {
        $this->status = $status;
    }
    
    public function getStatus(): Status 
    {
        return $this->status;    
    }
}

//php8.1
class BlogData
{
    public readonly Status $status;
   
    public function __construct(Status $status) 
    {
        $this->status = $status;
    }
}
```

3. *First-class Callable Syntax*
- It is now possible to get a reference to any function – this is called first-class callable syntax.
```php
//php8.1<
$foo = [$this, 'foo'];
$fn = Closure::fromCallable('strlen');

//php8.1
$foo = $this->foo(...);
$fn = strlen(...);
```

4. *New in initializers*
- Objects can now be used as default parameter values, static variables, and global constants, as well as in attribute arguments.
```php
//php8.1<
class Service 
{
    private Logger $logger;
 
    public function __construct(
        ?Logger $logger = null,
    ) {
        $this->logger = $logger ?? new NullLogger();
    }
}

//php8.1
class Service 
{
    private Logger $logger;
    
    public function __construct(
        Logger $logger = new NullLogger(),
    ) {
        $this->logger = $logger;
    }
}
```
- This effectively makes it possible to use *nested attributes*.
```php
//php8.1<
PHP < 8.1
class User 
{
    /**
     * @Assert\All({
     *     @Assert\NotNull,
     *     @Assert\Length(min=5)
     * })
     */
    public string $name = '';
}

//php8.1
class User 
{
    #[\Assert\All(
        new \Assert\NotNull,
        new \Assert\Length(min: 5))
    ]
    public string $name = '';
}
```

5. *Pure Intersection Types*
- Use intersection types when a value needs to satisfy multiple type constraints at the same time.
- It is not currently possible to mix intersection and union types together such as `A&B|C`.
```php
//php8.1<
function count_and_iterate(Iterator $value) {
    if (!($value instanceof Countable)) {
        throw new TypeError('value must be Countable');
    }

    foreach ($value as $val) {
        echo $val;
    }

    count($value);
}

//php8.1
function count_and_iterate(Iterator&Countable $value) {
    foreach ($value as $val) {
        echo $val;
    }

    count($value);
}
```

6. ***Never** return type*
- A function or method declared with the `never` type indicates that it will not return a value and will either throw 
an exception or end the script’s execution with a call of `die()`, `exit()`, `trigger_error()`, or something similar.
```php
//php8.1<
function redirect(string $uri) {
    header('Location: ' . $uri);
    exit();
}
 
function redirectToLoginPage() {
    redirect('/login');
    echo 'Hello'; // <- dead code
}

//php8.1
function redirect(string $uri): never {
    header('Location: ' . $uri);
    exit();
}
 
function redirectToLoginPage(): never {
    redirect('/login');
    echo 'Hello'; // <- dead code detected by static analysis 
}
```

7. *Final class constants*
- It is possible to declare final class constants, so that they cannot be overridden in child classes.
```php
//php8.1<
class Foo
{
    public const XX = "foo";
}

class Bar extends Foo
{
    public const XX = "bar"; // No error
}

//php8.1
class Foo
{
    final public const XX = "foo";
}

class Bar extends Foo
{
    public const XX = "bar"; // Fatal error
}
```

8. *Explicit Octal numeral notation*
- It is now possible to write octal numbers with the explicit `0o` prefix
```php
//php8.1<
016 === 16; // false because `016` is octal for `14` and it's confusing
016 === 14; // true 

//php8.1
0o16 === 16; // false — not confusing with explicit notation
0o16 === 14; // true 
```

9. *Fibers*
- Fibers are primitives for implementing lightweight cooperative concurrency.
  They are a means of creating code blocks that can be paused and resumed like Generators,
  but from anywhere in the stack. Fibers themselves don't magically provide concurrency, 
  there still needs to be an event loop. However, they allow blocking and non-blocking implementations
  to share the same API.
- Fibers allow getting rid of the boilerplate code previously seen with `Promise::then()` or `Generator` based coroutines.
  Libraries will generally build further abstractions around Fibers,
  so there's no need to interact with them directly.

```php
//php8.1<
$httpClient->request('https://example.com/')
        ->then(function (Response $response) {
            return $response->getBody()->buffer();
        })
        ->then(function (string $responseBody) {
            print json_decode($responseBody)['code'];
        });

//php8.1
$response = $httpClient->request('https://example.com/');
print json_decode($response->getBody()->buffer())['code'];
```

10. *Array unpacking support for string-keyed arrays*
- PHP supported unpacking inside arrays through the spread operator before, but only if the arrays had integer keys.
  Now it is possible to unpack arrays with string keys too.
```php
//php8.1<
$arrayA = ['a' => 1];
$arrayB = ['b' => 2];

$result = array_merge(['a' => 0], $arrayA, $arrayB); // ['a' => 1, 'b' => 2]

//php8.1
$arrayA = ['a' => 1];
$arrayB = ['b' => 2];

$result = ['a' => 0, ...$arrayA, ...$arrayB]; // ['a' => 1, 'b' => 2]
```

## New  classes and functions
- New `#[ReturnTypeWillChange]` attribute.
- New `fsync and fdatasync` functions.
- New `array_is_list` function.
- New `Sodium XChaCha20` functions.

## Deprecations and backward compatibility breaks
- Passing `null` to non-nullable internal function parameters is deprecated.
- Tentative return types in PHP built-in class methods [explanation](https://php.watch/versions/8.1/internal-method-return-types#:~:text=A%20tentative%20return%20types%20means,has%20an%20incompatible%20return%20type.)
- `Serializable` interface deprecated.
- HTML entity en/decode functions process single quotes and substitute by default.
- `$GLOBALS` variable restrictions.
- `MySQLi`: Default error mode set to exceptions.
- Implicit incompatible float to int conversion is deprecated.
- finfo Extension: `file_info` resources migrated to existing finfo objects.
- IMAP: imap resources migrated to `IMAP\Connection` class objects.
- FTP Extension: Connection resources migrated to `FTP\Connection` class objects.
- GD Extension: Font identifiers migrated to `GdFont` class objects.
- LDAP: resources migrated to `LDAP\Connection`, `LDAP\Result`, and `LDAP\ResultEntry` objects.
- PostgreSQL: resources migrated to `PgSql\Connection`, `PgSql\Result`, and `PgSql\Lob` objects.
Pspell: pspell, pspell config resources migrated to `PSpell\Dictionary`, `PSpell\Config` class objects.

## Performance related features in PHP 8.1:
- JIT backend for ARM64 (AArch64)
- Inheritance cache (avoid relinking classes in each request)
- Fast class name resolution (avoid lowercasing and hash lookup)
- timelib and ext/date performance improvements
- SPL file-system iterators improvements
- serialize/unserialize optimizations
- Some internal functions optimization (get_declared_classes(), explode(), strtr(), strnatcmp(), dechex())
- JIT improvements and fixes