#CompilerPass

Compiling service container allows:
- to prevent circular references in service dependencies
- to resolve parameters beforehand
- to remove unused dependencies if there were any

Container is compliled as:
```php
<?php

$container->compile();

```

Method `compile` uses Compiler Passes for the compilation.
Some Compiler passes are predefined for proper container compilation.
Custom compiler passes allow you to manipulate other service definitions
that has been registered in the service container.

Compiler passes are registered in the `build()` method of the application kernel:
```php
<?php

// src/Kernel.php
namespace App;

use App\DependencyInjection\Compiler\CustomPass;
use Symfony\Bundle\FrameworkBundle\Kernel\MicroKernelTrait;
use Symfony\Component\DependencyInjection\ContainerBuilder;
use Symfony\Component\HttpKernel\Kernel as BaseKernel;

class Kernel extends BaseKernel
{
    use MicroKernelTrait;

    // ...

    protected function build(ContainerBuilder $container): void
    {
        $container->addCompilerPass(new CustomPass());
    }
}

```

One of the most common use-cases of compiler passes 
is to work with **tagged services**. In those cases, 
instead of creating a compiler pass, you can make 
the kernel implement 
`Symfony\Component\DependencyInjection\Compiler\CompilerPassInterface` 
and process the services inside the process() method:

```php
<?php

// src/Kernel.php
namespace App;

use Symfony\Bundle\FrameworkBundle\Kernel\MicroKernelTrait;
use Symfony\Component\DependencyInjection\Compiler\CompilerPassInterface;
use Symfony\Component\DependencyInjection\ContainerBuilder;
use Symfony\Component\HttpKernel\Kernel as BaseKernel;

class Kernel extends BaseKernel implements CompilerPassInterface
{
    use MicroKernelTrait;

    // ...

    public function process(ContainerBuilder $container): void
    {
        // in this method you can manipulate the service container:
        // for example, changing some container service:
        $container->getDefinition('app.some_private_service')->setPublic(true);

        // or processing tagged services:
        foreach ($container->findTaggedServiceIds('some_tag') as $id => $tags) {
            // ...
        }
    }
}
```

[Official docs for compiler passes](https://symfony.com/doc/current/service_container/compiler_passes.html)

__Notes__:

Parent services are abstract classes for several extended class.
Symfony allows configure these services in config files to properly pass
and instantiate dependencies for extended classes.
[Documentation - parent classes](https://symfony.com/doc/current/service_container/parent_services.html)

