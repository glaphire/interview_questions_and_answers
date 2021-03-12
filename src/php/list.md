# PHP questions

##Language features
- Data types in PHP. Describe Resource and Callable types
- this VS self, self VS static, parent VS self
- Magic constants list. Does magic constant's value depend on the place where it is called
- Generators usage examples
- Cases of passing variable by reference by default
- Multiple inheritance in PHP
- What is OPCache
- What is an anonymous function
- What is autoload
- How sessions works
- How to call object destructor
- Calling object destructor VS garbage collector
- Interfaces. Inheritance of interfaces
- Magic methods
- Late static bindings

##Traits
- Traits - how add them. Inheriting and Instantiating traits
- How traits are included on low level

##PHP versions
- New features in PHP7 (main differences from PHP5)
- New features in PHP7.4
- New features in PHP 8.0

##Tools
- Composer - purpose and possibilities

------
##Tricky questions
1. Here is an array with keys 0, 1, 2, 3 and "Hello". What will be the key for the next value?
2. There are classes A, B, C with some restrictions:
    - A with a public constructor
    - B with a private constructor
    - C  extends B
    
    Which constructor will be inherited in class C?

3. How can you call a method with the same name from parent class inside child class?