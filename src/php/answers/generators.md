# Generators

[official documentation](https://www.php.net/manual/en/language.generators.php)

Generators allow to use simplified iterators without overhead of implementing real Iterator interface.

A generator allows you to write code that uses `foreach` to iterate over a set of data **without 
needing to build an array in memory, which may cause you to exceed a memory limit, or 
require a considerable amount of processing time to generate.** 

A generator function looks just like a normal function, 
except that instead of returning a value, 
a generator yields as many values as it needs to. 
Any function containing yield is a generator function.

When a generator function is called, it returns an object that can be iterated over. 
When you iterate over that object, PHP will call the object's iteration methods each time it needs a value, 
then saves the state of the generator when the generator yields a value 
so that it can be resumed when the next value is required.

Once there are no more values to be yielded, 
then the generator can simply exit, and the calling code continues just as if an array has run out of values.


**Note**:
A generator can return values, which can be retrieved using Generator::getReturn().

## Generators usage examples

Basic example: 
```php
<?php
function gen_one_to_three() {
    for ($i = 1; $i <= 3; $i++) {
        // Note that $i is preserved between yields.
        yield $i;
    }
}

$generator = gen_one_to_three();
foreach ($generator as $value) {
    echo "$value\n";
}

/*
 output:
	1
	2
	3
 */

```

Example with key-value pairs:

The syntax for yielding 
a key/value pair is very similar to that used to define an associative array, as shown below.
```php
<?php
/*
 * The input is semi-colon separated fields, with the first
 * field being an ID to use as a key.
 */

$input = <<<'EOF'
1;PHP;Likes dollar signs
2;Python;Likes whitespace
3;Ruby;Likes blocks
EOF;

function input_parser($input) {
    foreach (explode("\n", $input) as $line) {
        $fields = explode(';', $line);
        $id = array_shift($fields);

        yield $id => $fields;
    }
}

foreach (input_parser($input) as $id => $fields) {
    echo "$id:\n";
    echo "    $fields[0]\n";
    echo "    $fields[1]\n";
}

/*
  output:
1:
    PHP
    Likes dollar signs
2:
    Python
    Likes whitespace
3:
    Ruby
    Likes blocks
 */
?>
```

**Caution**

As with the simple value yields shown earlier, yielding a key/value pair 
in an expression context requires the yield statement to be parenthesised:

`$data = (yield $key => $value);`

**Notes**:

1) Generator functions are able to yield values by reference as well as by value. 
This is done in the same way as returning references from functions: 
by prepending an ampersand to the function name.
2) Generator delegation allows you to yield values from another generator, 
`Traversable` object, or array by using the `yield from` keyword. 
The outer generator will then yield all values from the inner generator, 
object, or array until that is no longer valid, 
after which execution will continue in the outer generator.
If a generator is used with `yield from`, 
the yield from expression will also return any value returned by the inner generator.

Examples may be found in the official documentation.
 
## What makes function to become a generator?

Function becomes a generator when `yield` is used to return a value instead of usual `return`.