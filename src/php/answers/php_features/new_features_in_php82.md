# PHP 8.2 new features

[Documentation: PHP 8.2 release](https://www.php.net/releases/8.2/en.php)

## Main new Features

1. *Readonly classes*
```php
//php8.2<
class BlogData
{
    public readonly string $title;

    public readonly Status $status;

    public function __construct(string $title, Status $status)
    {
        $this->title = $title;
        $this->status = $status;
    }
}

//php8.2
readonly class BlogData
{
    public string $title;

    public Status $status;

    public function __construct(string $title, Status $status)
    {
        $this->title = $title;
        $this->status = $status;
    }
}
```

2. *Disjunctive Normal Form (DNF) Types*
- DNF types allow us to combine union and intersection types, following a strict rule:
  when combining union and intersection types, intersection types must be grouped with brackets.
```php
//php8.2<
class Foo {
    public function bar(mixed $entity) {
        if ((($entity instanceof A) && ($entity instanceof B)) || ($entity === null)) {
            return $entity;
        }

        throw new Exception('Invalid entity');
    }
}

//php8.2
class Foo {
    public function bar((A&B)|null $entity) {
        return $entity;
    }
}
```

3. *Allow `null`, `false`, and `true` as stand-alone types*
```php
//php8.2<
class Falsy
{
    public function almostFalse(): bool { /* ... */ *}
    public function almostTrue(): bool { /* ... */ *}
    public function almostNull(): string|null { /* ... */ *}
}

//php8.2
class Falsy
{
    public function alwaysFalse(): false { /* ... */ *}
    public function alwaysTrue(): true { /* ... */ *}
    public function alwaysNull(): null { /* ... */ *}
}
```

4. *New "Random" extension*
- The "random" extension provides a new object-oriented API to random number generation.
  Instead of relying on a globally seeded random number generator (RNG) using the Mersenne Twister algorithm 
  the object-oriented API provides several classes ("Engine"s) providing access to modern algorithms
  that store their state within objects to allow for multiple independent seedable sequences.
- The `\Random\Randomizer` class provides a high level interface to use the engine's randomness to generate
  a random integer, to shuffle an array or string, to select random array keys and more.
```php
use Random\Engine\Xoshiro256StarStar;
use Random\Randomizer;

$blueprintRng = new Xoshiro256StarStar(
    hash('sha256', "Example seed that is converted to a 256 Bit string via SHA-256", true)
);

$fibers = [];
for ($i = 0; $i < 8; $i++) {
    $fiberRng = clone $blueprintRng;
    // Xoshiro256**'s 'jump()' method moves the blueprint ahead 2**128 steps, as if calling
    // 'generate()' 2**128 times, giving the Fiber 2**128 unique values without needing to reseed.
    $blueprintRng->jump();

    $fibers[] = new Fiber(function () use ($fiberRng, $i): void {
        $randomizer = new Randomizer($fiberRng);

        echo "{$i}: " . $randomizer->getInt(0, 100), PHP_EOL;
    });
}

// The randomizer will use a CSPRNG by default.
$randomizer = new Randomizer();

// Even though the fibers execute in a random order, they will print the same value
// each time, because each has its own unique instance of the RNG.
$fibers = $randomizer->shuffleArray($fibers);
foreach ($fibers as $fiber) {
    $fiber->start();
}
```

5. *Constants in traits*
- You cannot access the constant through the name of the trait,
  but, you can access the constant through the class that uses the trait.
```php
trait Foo
{
    public const CONSTANT = 1;
}

class Bar
{
    use Foo;
}

var_dump(Bar::CONSTANT); // 1
var_dump(Foo::CONSTANT); // Error
```

6. *Deprecate dynamic properties*
- The creation of dynamic properties is deprecated to help avoid mistakes and typos,
  unless the class opts in by using the `#[\AllowDynamicProperties]` attribute. `stdClass` allows dynamic properties.
- Usage of the `__get`/`__set` magic methods is not affected by this change.
```php
//php8.2<
class User
{
    public $name;
}

$user = new User();
$user->last_name = 'Doe';

$user = new stdClass();
$user->last_name = 'Doe';

//php8.2
class User
{
    public $name;
}

$user = new User();
$user->last_name = 'Doe'; // Deprecated notice

$user = new stdClass();
$user->last_name = 'Doe'; // Still allowed
```

## New Classes, Interfaces, and Functions
- New `mysqli_execute_query` function and `mysqli::execute_query` method.
- New `#[\AllowDynamicProperties]` and `#[\SensitiveParameter]` attributes.
- New `ZipArchive::getStreamIndex`, `ZipArchive::getStreamName`, and `ZipArchive::clearError` methods.
- New `ReflectionFunction::isAnonymous` and `ReflectionMethod::hasPrototype` methods.
- New `curl_upkeep`, `memory_reset_peak_usage`, `ini_parse_quantity`, `libxml_get_external_entity_loader`,
  `sodium_crypto_stream_xchacha20_xor_ic`, `openssl_cipher_key_length` functions.

## Deprecations and backward compatibility breaks
- Deprecated ${} string interpolation.
- Deprecated `utf8_encode` and `utf8_decode` functions.
- Methods `DateTime::createFromImmutable` and `DateTimeImmutable::createFromMutable` has a tentative return type of `static`.
- Extensions `ODBC` and `PDO_ODBC` escapes the username and password.
- Functions `strtolower` and `strtoupper` are no longer locale-sensitive.
- Methods `SplFileObject::getCsvControl`, `SplFileObject::fflush`, `SplFileObject::ftell`, `SplFileObject::fgetc`, and `SplFileObject::fpassthru` enforces their signature.
- Method `SplFileObject::hasChildren` has a tentative return type of false.
- Method `SplFileObject::getChildren` has a tentative return type of null.
- The internal method `SplFileInfo::_bad_state_ex` has been deprecated.