# PHP 8.3 new features

[Documentation: PHP 8.3 release](https://www.php.net/releases/8.3/en.php)

## Main new Features

1. *Typed class constants*
```php
//php8.3<
interface I {
    // We may naively assume that the PHP constant is always a string.
    const PHP = 'PHP 8.2';
}

class Foo implements I {
    // But implementing classes may define it as an array.
    const PHP = [];
}

//php8.3
interface I {
    const string PHP = 'PHP 8.3';
}

class Foo implements I {
    // Fatal error: Cannot use array as value for class constant
    // Foo::PHP of type string
    const string PHP = []; 
}
```

2. *Dynamic class constant fetch*
```php
//php8.3<
class Foo {
    const PHP = 'PHP 8.2';
}

$searchableConstant = 'PHP';

var_dump(constant(Foo::class . "::{$searchableConstant}"));

//php8.3
class Foo {
    const PHP = 'PHP 8.3';
}

$searchableConstant = 'PHP';

var_dump(Foo::{$searchableConstant});
```

3. *New #[\Override] attribute*
- By adding the `#[\Override]` attribute to a method,
  PHP will ensure that a method with the same name exists in a parent class or in an implemented interface.
  Adding the attribute makes it clear that overriding a parent method is intentional and simplifies refactoring,
  because the removal of an overridden parent method will be detected.
```php
//php8.3<
use PHPUnit\Framework\TestCase;

final class MyTest extends TestCase {
    protected $logFile;

    protected function setUp(): void {
        $this->logFile = fopen('/tmp/logfile', 'w');
    }

    // The log file will never be removed, because the
    // method name was mistyped (taerDown vs tearDown).
    protected function taerDown(): void {
        fclose($this->logFile);
        unlink('/tmp/logfile');
    }
}

//php8.3
use PHPUnit\Framework\TestCase;

final class MyTest extends TestCase {
    protected $logFile;

    protected function setUp(): void {
        $this->logFile = fopen('/tmp/logfile', 'w');
    }

    // Fatal error: MyTest::taerDown() has #[\Override] attribute,
    // but no matching parent method exists
    #[\Override]
    protected function taerDown(): void {
        fclose($this->logFile);
        unlink('/tmp/logfile');
    }
}
```

4. *Deep-cloning of readonly properties*
- `readonly` properties may now be modified once within the magic `__clone`
  method to enable deep-cloning of `readonly` properties.
```php
class PHP {
    public string $version = '8.2';
}

readonly class Foo {
    public function __construct(
        public PHP $php
    ) {}

    public function __clone(): void {
        $this->php = clone $this->php;
    }
}

$instance = new Foo(new PHP());
// Fatal error: Cannot modify readonly property Foo::$php
$cloned = clone $instance;
```

5. *New `json_validate()` function*
```php
//php8.3<
function json_validate(string $string): bool {
    json_decode($string);

    return json_last_error() === JSON_ERROR_NONE;
}

var_dump(json_validate('{ "test": { "foo": "bar" } }')); // true

//php8.3
var_dump(json_validate('{ "test": { "foo": "bar" } }')); // true
```

6. *New `Randomizer::getBytesFromString()` method*
- The Random Extension that was added in PHP 8.2 was extended by a new method
  to generate random strings consisting of specific bytes only.
  This method allows the developer to easily generate random identifiers, such as domain names,
  and numeric strings of arbitrary length.
```php
//php8.3<
// This function needs to be manually implemented.
function getBytesFromString(string $string, int $length) {
    $stringLength = strlen($string);

    $result = '';
    for ($i = 0; $i < $length; $i++) {
        // random_int is not seedable for testing, but secure.
        $result .= $string[random_int(0, $stringLength - 1)];
    }

    return $result;
}

$randomDomain = sprintf(
    "%s.example.com",
    getBytesFromString(
        'abcdefghijklmnopqrstuvwxyz0123456789',
        16,
    ),
);

echo $randomDomain

//php8.3
// A \Random\Engine may be passed for seeding,
// the default is the secure engine.
$randomizer = new \Random\Randomizer();

$randomDomain = sprintf(
    "%s.example.com",
    $randomizer->getBytesFromString(
        'abcdefghijklmnopqrstuvwxyz0123456789',
        16,
    ),
);

echo $randomDomain;
```

7. *New `Randomizer::getFloat()` and `Randomizer::nextFloat()` methods*
- Due to the limited precision and implicit rounding of floating point numbers,
  generating an unbiased float lying within a specific interval is non-trivial and the commonly
  used userland solutions may generate biased results or numbers outside the requested range.
- The Randomizer was also extended with two methods to generate random floats in an unbiased fashion.
  The `Randomizer::getFloat()` method uses the γ-section algorithm that was published
  in Drawing Random Floating-Point Numbers from an Interval. Frédéric Goualard, ACM Trans.
  Model. Comput. Simul., 32:3, 2022.
```php
//php8.3<
// Returns a random float between $min and $max, both including.
function getFloat(float $min, float $max) {
    // This algorithm is biased for specific inputs and may
    // return values outside the given range. This is impossible
    // to work around in userland.
    $offset = random_int(0, PHP_INT_MAX) / PHP_INT_MAX;

    return $offset * ($max - $min) + $min;
}

$temperature = getFloat(-89.2, 56.7);

$chanceForTrue = 0.1;
// getFloat(0, 1) might return the upper bound, i.e. 1,
// introducing a small bias.
$myBoolean = getFloat(0, 1) < $chanceForTrue;

//php8.3
$randomizer = new \Random\Randomizer();

$temperature = $randomizer->getFloat(
    -89.2,
    56.7,
    \Random\IntervalBoundary::ClosedClosed,
);

$chanceForTrue = 0.1;
// Randomizer::nextFloat() is equivalent to
// Randomizer::getFloat(0, 1, \Random\IntervalBoundary::ClosedOpen).
// The upper bound, i.e. 1, will not be returned.
$myBoolean = $randomizer->nextFloat() < $chanceForTrue;
```

8. *Command line linter supports multiple files*
- The command line linter now accepts variadic input for filenames to lint
```php
//php8.3<
$ php -l foo.php bar.php //No syntax errors detected in foo.php

//php8.3
$ php -l foo.php bar.php
//No syntax errors detected in foo.php
//No syntax errors detected in bar.php
```

## New Classes, Interfaces, and Functions
- New `DOMElement::getAttributeNames()`, `DOMElement::insertAdjacentElement()`,
  `DOMElement::insertAdjacentText()`, `DOMElement::toggleAttribute()`, `DOMNode::contains()`,
  `DOMNode::getRootNode()`, `DOMNode::isEqualNode()`, `DOMNameSpaceNode::contains()`, and `DOMParentNode::replaceChildren()` methods.
- New `IntlCalendar::setDate()`, `IntlCalendar::setDateTime()`,
  `IntlGregorianCalendar::createFromDate()`, and `IntlGregorianCalendar::createFromDateTime()` methods.
- New `ldap_connect_wallet()`, and `ldap_exop_sync()` functions.
- New `mb_str_pad()` function.
- New `posix_sysconf()`, `posix_pathconf()`, `posix_fpathconf()`, and `posix_eaccess()` functions.
- New `ReflectionMethod::createFromMethodName()` method.
- New `socket_atmark()` function.
- New `str_increment()`, `str_decrement()`, and `stream_context_set_options()` functions.
- New `ZipArchive::getArchiveFlag()` method.
- Support for generation EC keys with custom EC parameters in OpenSSL extension.
- New INI setting `zend.max_allowed_stack_size` to set the maximum allowed stack size.
- php.ini now supports fallback/default value syntax.
- Anonymous classes can now be readonly.

## Deprecations and backward compatibility breaks
- More Appropriate Date/Time Exceptions.
- Assigning a negative index `n` to an empty array will now make sure that the next index is `n + 1` instead of `0`.
- Changes to the `range()` function.
- Changes in re-declaration of static properties in traits.
- The `U_MULTIPLE_DECIMAL_SEPERATORS` constant is deprecated in favor of `U_MULTIPLE_DECIMAL_SEPARATORS`.
- The `MT_RAND_PHP` Mt19937 variant is deprecated.
- `ReflectionClass::getStaticProperties()` is no longer nullable.
- INI settings `assert.active`, `assert.bail`, `assert.callback`, `assert.exception`, and `assert.warning` have been deprecated.
- Calling `get_class()` and `get_parent_class()` without arguments are deprecated.
- SQLite3: Default error mode set to exceptions.