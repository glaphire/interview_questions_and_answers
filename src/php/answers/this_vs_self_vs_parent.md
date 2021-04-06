# this VS self, self VS static, parent VS self

## this

Pseudovariable `$this` is an object instance of a called class within method context.

```php
<?php 

class SomeClass
{
    public $someVariable = 'some variable';
    
    public function someMethod()
    {
        return $this->someVariable; 
    }
}
```

You can't use `$this` outside object context.

## parent
A keyword `parent`, along with scope resolution operator `::`, 
allows to call methods, constants and properties of the parent class inside extended class.
`parent` can be called in static and dynamic context.

When an extending class overrides the parents definition of a method, PHP will not call the parent's method. 
**It's up to the extended class on whether or not the parent's method is called.**
This also applies to Constructors and Destructors, Overloading, and Magic method definitions.

```php
<?php 

class BaseClass
{
    public $baseProperty = 'some variable';
    
    public function __construct($prop) {
        $this->baseProperty = $prop;
    }
    
    public function getBaseProperty()
    {
        return $this->baseProperty; 
    }
}

class ExtendedClass extends BaseClass
{
    private $baseProperty = 'new variable';
    
    public function getBaseProperty()
    {
        return parent::$baseProperty . ' ' . $this->baseProperty;
    }
}

```

## self
A keyword `self` with `::` is used to access class methods and variables in a static context.
In the class context, it is possible to create current class instance using `new self` or `new parent`.

```php
<?php 

class SomeClass
{
	public static $someProp;
	
	public static function newInstance()
	{
	    return new self;
	}
	
	public static function setProp($prop)
	{
	    self::$someProp = $prop;
	}
}
```
## static
Keyword `static` is used for:
- declaring static properties in the class (properties that can be used without class instantiation)
- declaring static methods
- calling methods and properties in the context of current class ([late static bindings](https://www.php.net/manual/en/language.oop5.late-static-bindings.php))

**Note**:

In non-static contexts, the called class will be the class of the object instance. 
Since `$this->` will try to call private methods from the same scope, using static:: may give different results. 
Another difference is that static:: can only refer to static properties.

## Conclusion
- this VS self - `this` is used for dynamical context of accessing class properties/methods, `self` - in static context.
- self VS static - `self` will call parent methods/properties of base class if they exist in both classes, 
`static` will look for child class properties/methods first.
- parent VS self - `parent` call will use properties/methods of parent class instead of the current one.