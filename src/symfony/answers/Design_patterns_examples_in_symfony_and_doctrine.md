# Design patterns examples in Symfony and Doctrine

## Symfony
- Singleton - in ServiceContainer (not technically singleton, but is one instance per request)
- Service Locator - Container internals in request lifecycle (\Symfony\Component\DependencyInjection\ServiceLocator)
- Decorator - in Traceable classes-wrappers - TraceableResolver, TraceableEvent etc.; 
- Adapter - for different cache drivers (file cache, db cache, redis cache etc.)
- Observer+Mediator - in EventDispatcher component
- Command bus - in Messenger component
- Factory - as part of service container settings; in HttpKernel\ControllerMetadata\ArgumentMetadataFactory
- Composite - in Forms Component - (the way of Form parts are rendered)
[documentation](https://symfony.com/doc/current/service_container/factories.html)

## Doctrine

- Unit Of Work
- Facade - in Entity Manager
- Identity Map
- Data Mapper
- Proxy - in lazy loading entity classes
- Fluent Interface - in calling QueryBuilder methods, in calling Entity setters
- Builder - in 
[Query Builder](https://github.com/doctrine/orm/blob/old-prototype-3.x/lib/Doctrine/ORM/QueryBuilder.php)