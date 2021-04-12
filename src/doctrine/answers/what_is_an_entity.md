# What is an Entity?

[Official documentation](https://www.doctrine-project.org/projects/doctrine-orm/en/2.8/tutorials/getting-started.html#what-are-entities)

Entity is a plain PHP object that can be identified over many requests by a unique identifier 
or primary key.

Doctrine Entities don't need to extend any classes or implement interfaces.
Also they don't need to implement __clone or __wakeup (if they can't do it safely).

Doctrine's Entity contains persistable properties (as class properties).
Properties are mapped to the persistable representation layer by special mapping configurations
(usually done in via Annotatons or XML files).

Example of simple Entity in Doctrine:

```php
<?php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="products")
 */
class Product
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue
     */
    protected int $id;
    /**
     * @ORM\Column(type="string")
     */
    protected string $name;

	public function getId(): int
	{
		return $this->id;
	}

	public function getName(): string
	{
		return $this->name;
	}

	public function setName(string $name)
	{
		$this->name = $name;
	}
}
```