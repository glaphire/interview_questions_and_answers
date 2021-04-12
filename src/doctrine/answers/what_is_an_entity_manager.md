# What is an Entity Manager?

[Official documentation](https://www.doctrine-project.org/projects/doctrine-orm/en/2.8/tutorials/getting-started.html#obtaining-the-entitymanager)

EntityManager class is a public interface for manipulating entities lifecycle.
This class provides access points to the complete lifecycle management for entities, 
and transforms entities from and back to the persistence layer.

Basic configuration file for doctrine:

```php
<?php
// bootstrap.php
use Doctrine\ORM\Tools\Setup;
use Doctrine\ORM\EntityManager;

require_once "vendor/autoload.php";

// Create a simple "default" Doctrine ORM configuration for Annotations
$isDevMode = true; //adds internal settings when dev mode is enabled (e.g. enables ArrayCache)
$proxyDir = null; //set directory for Proxy classes
$cache = null; //set cache adapter that implements Cache interface
$useSimpleAnnotationReader = false; //simple annotation reader uses simpliest caching - in-memory in php arrays (to avoid parsing docblock everytime)
$config = Setup::createAnnotationMetadataConfiguration(array(__DIR__."/src"), $isDevMode, $proxyDir, $cache, $useSimpleAnnotationReader);
// or if you prefer yaml or XML

// database configuration parameters
$conn = array(
    'driver' => 'pdo_sqlite',
    'path' => __DIR__ . '/db.sqlite',
);

// obtaining the entity manager
$entityManager = EntityManager::create($conn, $config);

```

More about `$isDevMode`:

- If `$isDevMode` is true caching is done in memory with the ArrayCache. 
Proxy objects are recreated on every request.
- If `$isDevMode` is false, check for Caches in the order APC, Xcache, 
Memcache (127.0.0.1:11211), Redis (127.0.0.1:6379) unless `$cache` is passed as fourth argument.
- If `$isDevMode` is false, set then proxy classes have to be explicitly created through the command line.
- If third argument `$proxyDir` is not set, use the systems temporary directory.