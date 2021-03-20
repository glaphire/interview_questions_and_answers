# Request-Response flow

1. Web-server routes request to the application entry point (public/index.php).
    1. Env variables are loading
    2. Enabling debug mode (if is set), allowing trusted hosts and proxies (if provided)
    3. Kernel instance is created (with environment type and debug mode flag).
    4. Request instance is created from superglobal variables.
    The data from request query and/or body are transferred into \Symfony\Component\HttpFoundation\InputBag instance.
    5. Kernel is handling request instance. Steps inside \Symfony\Component\HttpKernel\Kernel::handle:
        1. Service that are allowed to be reset are reset (if request stack is not empty and kernel was previously booted).
        2. Bundles are initialized (from list in config/bundles.php)
        3. Container is initialized.
            1. Cached container is created on a first call or got from from cache on the next calls.
            2. Container is configured and compiled (if not cached):
                1. Internal settings are set
                2. MethodMap is set (to work with EventDispatcher, HttpKernel, RequestStack and Router).
                2. 'service id' => 'service class' map is created and set into the container
                3. service aliases are set.
                4. Internal classes/interfaces of Symfony core are added into the container.
                5. CompilerPass is called to add additional info if some services need to be specifically compiled.
        4. Bundles are set into container.
    
![Image example](Image-link.jpg)

Code fragments from Symfony 5.1 

```php
<?php
//public/index.php
use App\Kernel;
use Symfony\Component\Dotenv\Dotenv;
use Symfony\Component\ErrorHandler\Debug;
use Symfony\Component\HttpFoundation\Request;

require dirname(__DIR__).'/vendor/autoload.php';

(new Dotenv())->bootEnv(dirname(__DIR__).'/.env');

if ($_SERVER['APP_DEBUG']) {
    umask(0000);

    Debug::enable();
}

if ($trustedProxies = $_SERVER['TRUSTED_PROXIES'] ?? false) {
    Request::setTrustedProxies(explode(',', $trustedProxies), Request::HEADER_X_FORWARDED_ALL ^ Request::HEADER_X_FORWARDED_HOST);
}

if ($trustedHosts = $_SERVER['TRUSTED_HOSTS'] ?? false) {
    Request::setTrustedHosts([$trustedHosts]);
}

$kernel = new Kernel($_SERVER['APP_ENV'], (bool) $_SERVER['APP_DEBUG']);
$request = Request::createFromGlobals();
$response = $kernel->handle($request);
$response->send();
$kernel->terminate($request, $response);

```

```php
<?php
//symfony/http-kernel/kernel.php
public function handle(Request $request, int $type = HttpKernelInterface::MASTER_REQUEST, bool $catch = true)
{
	$this->boot();
	++$this->requestStackSize;
	$this->resetServices = true;

	try {
		return $this->getHttpKernel()->handle($request, $type, $catch);
	} finally {
		--$this->requestStackSize;
	}
}

```

```php
//symfony/http-kernel/kernel.php
<?php
    public function boot()
    {
        if (true === $this->booted) {
            if (!$this->requestStackSize && $this->resetServices) {
                if ($this->container->has('services_resetter')) {
                    $this->container->get('services_resetter')->reset();
                }
                $this->resetServices = false;
                if ($this->debug) {
                    $this->startTime = microtime(true);
                }
            }

            return;
        }
        if ($this->debug) {
            $this->startTime = microtime(true);
        }
        if ($this->debug && !isset($_ENV['SHELL_VERBOSITY']) && !isset($_SERVER['SHELL_VERBOSITY'])) {
            putenv('SHELL_VERBOSITY=3');
            $_ENV['SHELL_VERBOSITY'] = 3;
            $_SERVER['SHELL_VERBOSITY'] = 3;
        }

        // init bundles
        $this->initializeBundles();

        // init container
        $this->initializeContainer();

        foreach ($this->getBundles() as $bundle) {
            $bundle->setContainer($this->container);
            $bundle->boot();
        }

        $this->booted = true;
    }

```