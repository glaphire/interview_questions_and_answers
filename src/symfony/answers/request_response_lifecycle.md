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
                3. 'service id' => 'service class' map is created and set into the container
                4. Service aliases are set.
                5. Internal classes/interfaces of Symfony core are added into the container.
                6. EventDispatcher is instantiated and all event listeners are added.
                7. CompilerPass is called to add additional info if some services need to be specifically compiled.
                8. Data from the request is set to the container (TODO: check order in vendor code)
        4. Bundles are set into container.
        5. Each request in the request stack is handled:
            1. \Symfony\Component\HttpKernel\HttpKernel::handleRaw is called:
                1. Request is pushed to the request stack
                2. 'kernel.request' event with Request object is fired (dispatched).
                RouteListener is subscribed to this event and adds **_controller**, **_route** and **_route_params** parameters to the Request Object.
				3. Controller is resolved via ControllerResolver.
				4. 'kernel.controller' event is dispatched
				5. Controller arguments are resolved
				6. 'kernel.controller_arguments' event is dispatched
				7. Response is got from processed controller action
				    1. If response is not the Response class instance, 'kernel.view' is fired and view is rendered
				    2. If response is a Response instance, 'kernel.response' event is fired.
				    3. 'kernel.finish_request' event is fired
				    4. Request is moved from the RequestStack (because it was successfully processed)
				    5. Response is returned to the handle method
	6. HTTP response is sent to the client
	7. 'kernel.terminate' event is fired. [Purpose of this event](https://symfony.com/doc/current/components/http_kernel.html#the-kernel-terminate-event)
				    
				     
				        
    
![Request lifecycle from SF documentation](https://github.com/glaphire/interview_questions_and_answers/blob/main/src/symfony/answers/images/symfony_request_lifecycle.png)

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

```php
<?php
//symfony/http-kernel/kernel.php
private function handleRaw(Request $request, int $type = self::MASTER_REQUEST): Response
    {
        $this->requestStack->push($request);

        // request
        $event = new RequestEvent($this, $request, $type);
        $this->dispatcher->dispatch($event, KernelEvents::REQUEST);

        if ($event->hasResponse()) {
            return $this->filterResponse($event->getResponse(), $request, $type);
        }

        // load controller
        if (false === $controller = $this->resolver->getController($request)) {
            throw new NotFoundHttpException(sprintf('Unable to find the controller for path "%s". The route is wrongly configured.', $request->getPathInfo()));
        }

        $event = new ControllerEvent($this, $controller, $request, $type);
        $this->dispatcher->dispatch($event, KernelEvents::CONTROLLER);
        $controller = $event->getController();

        // controller arguments
        $arguments = $this->argumentResolver->getArguments($request, $controller);

        $event = new ControllerArgumentsEvent($this, $controller, $arguments, $request, $type);
        $this->dispatcher->dispatch($event, KernelEvents::CONTROLLER_ARGUMENTS);
        $controller = $event->getController();
        $arguments = $event->getArguments();

        // call controller
        $response = $controller(...$arguments);

        // view
        if (!$response instanceof Response) {
            $event = new ViewEvent($this, $request, $type, $response);
            $this->dispatcher->dispatch($event, KernelEvents::VIEW);

            if ($event->hasResponse()) {
                $response = $event->getResponse();
            } else {
                $msg = sprintf('The controller must return a "Symfony\Component\HttpFoundation\Response" object but it returned %s.', $this->varToString($response));

                // the user may have forgotten to return something
                if (null === $response) {
                    $msg .= ' Did you forget to add a return statement somewhere in your controller?';
                }

                throw new ControllerDoesNotReturnResponseException($msg, $controller, __FILE__, __LINE__ - 17);
            }
        }

        return $this->filterResponse($response, $request, $type);
    }
```