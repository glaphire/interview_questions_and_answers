# PHP questions

## Language features
- [Data types in PHP. Describe Resource and Callable types](https://github.com/glaphire/interview_questions_and_answers/blob/main/src/php/answers/data_types_in_php.md)
- [this VS self, self VS static, parent VS self](https://github.com/glaphire/interview_questions_and_answers/blob/main/src/php/answers/this_vs_self_vs_parent.md)
- [Magic constants list. Does magic constant's value depend on the place where it is called?](https://github.com/glaphire/interview_questions_and_answers/blob/main/src/php/answers/magic_constants.md)
- [Generators usage examples. What makes a function become a generator?](https://github.com/glaphire/interview_questions_and_answers/blob/main/src/php/answers/generators.md)
- Cases of passing variable by reference by default
- Multiple inheritance in PHP
- What is OPCache
- Closure vs anonymous function
- What is autoload
- How sessions works
- How to call object destructor
- Calling object destructor VS garbage collector
- Interfaces. Inheritance of interfaces
- Magic methods
- __invoke method useful examples
- Late static bindings
- Persistent Database Connections
- Reflection practical usage examples

## Traits
- Traits - how add them. Inheriting and Instantiating traits
- How traits are included on low level
- Can you add constant to a trait? How can you use that constant?
- Can you use private or protected methods in a trait?

## PHP versions
- New features in PHP7 (main differences from PHP5)
- New features in PHP7.4
- New features in PHP 8.0

## Tools
- Composer - purpose and possibilities
- composer install vs composer update

------

## Tricky questions
1. Here is an array with keys 0, 1, 2, 3 and "Hello". What will be the key for the next value?
2. There are classes A, B, C with some restrictions:
    - A with a public constructor
    - B with a private constructor
    - C  extends B
    
    Which constructor will be inherited in class C?

3. How can you call a method with the same name from parent class inside child class?
4. How to find second largest number in an unsorted list? You can use only one iteration to find it.