---
id: symfony-architecture
title: Symfony Architecture
---

### Symfony 4 default folder structure
```bash
your-project/
├── assets/
├── bin/
│   └── console
├── config/
│   ├── bundles.php
│   ├── packages/
│   ├── routes.yaml
│   └── services.yaml
├── public/
│   └── index.php
├── src/
│   ├── ...
│   └── Kernel.php
├── templates/
├── tests/
├── translations/
├── var/
└── vendor/
```

### Symfony/skeleton folder structure
```bash
your-project/
├── bin/
│   └── console
├── config/
│   ├── packages/
│   ├── bootstrap
│   ├── routes.yaml
│   └── services.yaml
├── public/
│   └── index.php
├── src/
│   ├── Controller/
│   └── Kernel.php
├── var/
└── vendor/
```

### Where the bundle assets are located?

### Symfony mail service transport parameter name?

### Symfony flex
https://symfony.com/doc/4.0/setup/flex.html


### How to send an email?
Symfony provides a mailer feature based on the popular `Swift Mailer` library via the `symfony/swiftmailer-bundle

### Mailer configuration
https://symfony.com/doc/4.0/reference/configuration/swiftmailer.html
`
https://symfony.com/doc/4.0/email.html


### HttpKernel component
```php
class Kernel extends BaseKernel
{
    use MicroKernelTrait;

    private const CONFIG_EXTS = '.{php,xml,yaml,yml}';

    public function registerBundles(): iterable
    {
        $contents = require $this->getProjectDir().'/config/bundles.php';
        foreach ($contents as $class => $envs) {
            if ($envs[$this->environment] ?? $envs['all'] ?? false) {
                yield new $class();
            }
        }
    }

    protected function configureContainer(ContainerBuilder $container, LoaderInterface $loader): void
    {
        $container->addResource(new FileResource($this->getProjectDir().'/config/bundles.php'));
        $container->setParameter('container.dumper.inline_class_loader', true);
        $confDir = $this->getProjectDir().'/config';

        $loader->load($confDir.'/{packages}/*'.self::CONFIG_EXTS, 'glob');
        $loader->load($confDir.'/{packages}/'.$this->environment.'/**/*'.self::CONFIG_EXTS, 'glob');
        $loader->load($confDir.'/{services}'.self::CONFIG_EXTS, 'glob');
        $loader->load($confDir.'/{services}_'.$this->environment.self::CONFIG_EXTS, 'glob');
    }

    protected function configureRoutes(RouteCollectionBuilder $routes): void
    {
        $confDir = $this->getProjectDir().'/config';

        $routes->import($confDir.'/{routes}/'.$this->environment.'/**/*'.self::CONFIG_EXTS, '/', 'glob');
        $routes->import($confDir.'/{routes}/*'.self::CONFIG_EXTS, '/', 'glob');
        $routes->import($confDir.'/{routes}'.self::CONFIG_EXTS, '/', 'glob');
    }
}
```

###  Kernel request
https://symfony.com/doc/4.0/components/http_kernel.html#component-http-kernel-kernel-request

### HttpKernelInterface
```php
namespace Symfony\Component\HttpKernel;

use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

/**
 * HttpKernelInterface handles a Request to convert it to a Response.
 */
interface HttpKernelInterface
{
    const MASTER_REQUEST = 1;
    const SUB_REQUEST = 2;

    /**
     * Handles a Request to convert it to a Response.
     *
     * When $catch is true, the implementation must catch all exceptions
     * and do its best to convert them to a Response instance.
     */
    public function handle(Request $request, $type = self::MASTER_REQUEST, $catch = true);
}
```

> Return `Symfony\Component\HttpFoundation\Response`

### Two main classes implementing the `HttpKernelInterface`
```
Symfony\Component\HttpKernel\HttpCache\HttpCache
Symfony\Component\HttpKernel\HttpKernel
```

### How to instantiate the Http kernel?
```php
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpKernel\HttpKernel;
use Symfony\Component\EventDispatcher\EventDispatcher;
use Symfony\Component\HttpFoundation\RequestStack;
use Symfony\Component\HttpKernel\Controller\ArgumentResolver;
use Symfony\Component\HttpKernel\Controller\ControllerResolver;

// create the Request object
$request = Request::createFromGlobals();

$dispatcher = new EventDispatcher();
// ... add some event listeners

// create your controller and argument resolvers
$controllerResolver = new ControllerResolver();
$argumentResolver = new ArgumentResolver();

// instantiate the kernel
$kernel = new HttpKernel($dispatcher, $controllerResolver, new RequestStack(), $argumentResolver);

// actually execute the kernel, which turns the request into a response
// by dispatching events, calling a controller, and returning the response
$response = $kernel->handle($request);

// send the headers and echo the content
$response->send();

// trigger the kernel.terminate event
$kernel->terminate($request, $response);
```

### How to instantiate the HttpKernel/Kernel?
```php
public function __construct(string $environment, bool $debug)
```

### Which kernel event exist?
https://symfony.com/doc/4.0/components/http_kernel.html#component-http-kernel-event-table

### 
```php
/**
 * The REQUEST event occurs at the very beginning of request
 * dispatching.
 *
 * This event allows you to create a response for a request before any
 * other code in the framework is executed.
 *
 * @Event("Symfony\Component\HttpKernel\Event\GetResponseEvent")
 */
const REQUEST = 'kernel.request';

/**
 * The EXCEPTION event occurs when an uncaught exception appears.
 *
 * This event allows you to create a response for a thrown exception or
 * to modify the thrown exception.
 *
 * @Event("Symfony\Component\HttpKernel\Event\GetResponseForExceptionEvent")
 */
const EXCEPTION = 'kernel.exception';

/**
 * The VIEW event occurs when the return value of a controller
 * is not a Response instance.
 *
 * This event allows you to create a response for the return value of the
 * controller.
 *
 * @Event("Symfony\Component\HttpKernel\Event\GetResponseForControllerResultEvent")
 */
const VIEW = 'kernel.view';

/**
 * The CONTROLLER event occurs once a controller was found for
 * handling a request.
 *
 * This event allows you to change the controller that will handle the
 * request.
 *
 * @Event("Symfony\Component\HttpKernel\Event\FilterControllerEvent")
 */
const CONTROLLER = 'kernel.controller';

/**
 * The CONTROLLER_ARGUMENTS event occurs once controller arguments have been resolved.
 *
 * This event allows you to change the arguments that will be passed to
 * the controller.
 *
 * @Event("Symfony\Component\HttpKernel\Event\FilterControllerArgumentsEvent")
 */
const CONTROLLER_ARGUMENTS = 'kernel.controller_arguments';

/**
 * The RESPONSE event occurs once a response was created for
 * replying to a request.
 *
 * This event allows you to modify or replace the response that will be
 * replied.
 *
 * @Event("Symfony\Component\HttpKernel\Event\FilterResponseEvent")
 */
const RESPONSE = 'kernel.response';

/**
 * The TERMINATE event occurs once a response was sent.
 *
 * This event allows you to run expensive post-response jobs.
 *
 * @Event("Symfony\Component\HttpKernel\Event\PostResponseEvent")
 */
const TERMINATE = 'kernel.terminate';

/**
 * The FINISH_REQUEST event occurs when a response was generated for a request.
 *
 * This event allows you to reset the global and environmental state of
 * the application, when it was changed during the request.
 *
 * @Event("Symfony\Component\HttpKernel\Event\FinishRequestEvent")
 */
const FINISH_REQUEST = 'kernel.finish_request';
```

### How to check for attached listeners for events?

https://symfony.com/doc/4.0/event_dispatcher.html

### Which method allows to prevent any other Event listeners from being called?
`stopPropagation()`

### Is it possible to detect if an Event was stopped during runtime?
yes

### EventDispatcher Component
The EventDispatcher component provides tools that allow your application
components to communicate with each other by dispatching events and listening to them.

### Available interfaces
- EventDispatcherInterface
- EventSubscriberInterface
- TraceableEventDispatcherInterface - deprecated as of SF 4.1

### How to install EventDispatcher Component?
```bash
composer require symfony/event-dispatcher
```

### Event naming conventions
- Use only lowercase letters, numbers, dots (.) and underscores (_)
- Prefix names with a namespace followed by a dot (e.g. order., user.*)
- End names with a verb that indicates what action has been taken (e.g. order.placed)

### Symfony\Component\EventDispatcher\EventDispatcherInterface
The EventDispatcherInterface is the central point of Symfony's event listener system.
Listeners are registered on the manager and events are dispatched through the manager.

### Dispatches an event to all registered listeners.
```php
/**
 * @param string     $eventName The name of the event to dispatch. The name of
 *                              the event is the name of the method that is
 *                              invoked on listeners.
 * @param Event|null $event     The event to pass to the event handlers/listeners
 *                              If not supplied, an empty Event instance is created
 *
 * @return Event
 */
public function dispatch($eventName, Event $event = null);
```
#### Usage
```php
$dispatcher->dispatch(OrderPlaced::NAME, new OrderPlaced(new Order()));
```

### Connecting Listeners
```php
/**
 * Adds an event listener that listens on the specified events.
 *
 * @param string   $eventName The event to listen on
 * @param callable $listener  The listener
 * @param int      $priority  The higher this value, the earlier an event
 *                            listener will be triggered in the chain (defaults to 0)
 */
public function addListener($eventName, $listener, $priority = 0);
```
#### usage with callable
```php
use Symfony\Component\EventDispatcher\Event;

class AcmeListener
{
    // ...

    public function onFooAction(Event $event)
    {
        // ... do something
    }
}

$listener = new AcmeListener();
$dispatcher->addListener('acme.foo.action', array($listener, 'onFooAction'));
```

#### usage with closures
```php
use Symfony\Component\EventDispatcher\Event;

$dispatcher->addListener('acme.foo.action', function (Event $event) {
    // will be executed when the acme.foo.action event is dispatched
});
```
###
```php
/**
 * Adds an event subscriber.
 *
 * The subscriber is asked for all the events it is
 * interested in and added as a listener for these events.
 */
public function addSubscriber(EventSubscriberInterface $subscriber);
```
###
```php
/**
 * Removes an event listener from the specified events.
 *
 * @param string   $eventName The event to remove a listener from
 * @param callable $listener  The listener to remove
 */
public function removeListener($eventName, $listener);
```
###
```php
public function removeSubscriber(EventSubscriberInterface $subscriber);
```
###
```php
/**
 * Gets the listeners of a specific event or all listeners sorted by descending priority.
 *
 * @param string|null $eventName The name of the event
 *
 * @return array The event listeners for the specified event, or all event listeners by event name
 */
public function getListeners($eventName = null);
```
###
```php
/**
 * Gets the listener priority for a specific event.
 *
 * Returns null if the event or the listener does not exist.
 *
 * @param string   $eventName The name of the event
 * @param callable $listener  The listener
 *
 * @return int|null The event listener priority
 */
public function getListenerPriority($eventName, $listener);
```
###
```php
/**
 * Checks whether an event has any registered listeners.
 *
 * @param string|null $eventName The name of the event
 *
 * @return bool true if the specified event has any listeners, false otherwise
 */
public function hasListeners($eventName = null);
```

### Symfony\Component\EventDispatcher\EventSubscriberInterface
An EventSubscriber knows itself what events it is interested in.
 * If an EventSubscriber is added to an EventDispatcherInterface, the manager invokes
 * {@link getSubscribedEvents} and registers the subscriber as a listener for all
 * returned events.
```php
interface EventSubscriberInterface
{
    /**
     * Returns an array of event names this subscriber wants to listen to.
     *
     * The array keys are event names and the value can be:
     *
     *  * The method name to call (priority defaults to 0)
     *  * An array composed of the method name to call and the priority
     *  * An array of arrays composed of the method names to call and respective
     *    priorities, or 0 if unset
     *
     * For instance:
     *
     *  * ['eventName' => 'methodName']
     *  * ['eventName' => ['methodName', $priority]]
     *  * ['eventName' => [['methodName1', $priority], ['methodName2']]]
     *
     * @return array The event names to listen to
     */
    public static function getSubscribedEvents();
}
```

### Which design pattern the EventDispatcher component implements?
Mediator and Observer

### Explain event subscriber?
Event subscriber is an another way to listen to events.

### Symfony is released under which license ?
MIT license

https://symfony.com/doc/current/contributing/code/license.html


### Which component provides built-in symfony events?
HttpKernel component

### What built-in events does?
during HTTP request handling it dispatch some events which you can use to modify how the request is handled. 

### Symfony Kernel event system
https://symfony.com/doc/4.0/reference/events.html

### What is the prefix of environment variables used by Symfony?
SYMFONY_

### The ClassLoader Component
It is removed in Symfony 4.0

### What is FrameworkBundle?
It is used to define the main framework configuration. All these options are configures under the `framework` key

### Display the default config values defined by symfony
```bash
bin/console config:dump-reference framework
```

### Display the actual config values used by your application
```bash
bin/console debug:config framework
```

### What is the namespace used when using XML configuration for framework?
```xml
http://symfony.com/schema/dic/symfony
```

### What is the XSD schema used when using XML configuration for framework?
```xml
http://symfony.com/schema/dic/symfony/symfony-1.0.xsd
```


### What is the namespace used when using XML configuration for services?
```xml
http://symfony.com/schema/dic/services
```

### What is the XSD schema used when using XML configuration for services?
```xml
http://symfony.com/schema/dic/symfony/services-1.0.xsd
```

### Example
```xml
<container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:framework="http://symfony.com/schema/dic/symfony"
    xsi:schemaLocation="http://symfony.com/schema/dic/services
        http://symfony.com/schema/dic/services/services-1.0.xsd
        http://symfony.com/schema/dic/symfony http://symfony.com/schema/dic/symfony/symfony-1.0.xsd">
```

### Usage of FrameworkBundle configuration keys
https://symfony.com/doc/4.0/reference/configuration/framework.html#configuration

### What is the right parameter path to set a version number for assets?
`framework.assets.version: v2`

### All supported assets configurations
```yaml
framework:
    assets:
        base_path: '/images'
        base_urls:
            - 'http://cdn.example.com/'
        version: 'v2'
        version_format: '%%s?%%s'
        version_strategy: 'app.asset.my_versioning_strategy'
        # this manifest is applied to every asset (including packages)
        json_manifest_path: "%kernel.project_dir%/public/build/manifest.json"
        packages:
            avatars:
                base_urls: 'http://static_cdn.example.com/avatars'
        
```

```xml
<framework:config>
    <framework:assets version="v2" />
</framework:config>
```

```php
$container->loadFromExtension('framework', array(
    // ...
    'assets' => array(
        'version' => 'v2',
    ),
));
```

### How symfony handles when multiple `base_urls` provided?
Symfony will select one from the collection each time it generates an asset's path.

### What `packages` configuration does?

### How to Use a Custom Version Strategy for Assets?
[Here](https://symfony.com/doc/4.0/frontend/custom_version_strategy.html)

[https://symfony.com/doc/4.0/reference/configuration/framework.html#packages](https://symfony.com/doc/4.0/reference/configuration/framework.html#packages)


#### secret
```yaml
framework:
    secret: '%env(APP_SECRET)%'
```
- recommended length is around 32 characters

https://symfony.com/doc/4.0/reference/configuration/framework.html#secret

#### http_method_override
https://symfony.com/doc/4.0/reference/configuration/framework.html#http-method-override


### How to Configure Symfony to Work behind a Load Balancer or a Reverse Proxy?
https://symfony.com/doc/4.0/deployment/proxies.html

### Component vs bundle

### What is the path to set session cookie_lifetime parameter in a project configuration?
```
framework.session.cookie_lifetime
```

### Managing the session
https://symfony.com/doc/4.0/reference/configuration/framework.html#session

### What is the default Symfony session storage?
Files `session.handler.native_file`

### How to use database to store session?
1. Register service
2. Configure the handler_id
3. Configure the table and column names
4. Prepare the database
https://symfony.com/doc/4.0/doctrine/pdo_session_storage.html

### How to Configure the database credentials as environment variable
https://symfony.com/doc/4.0/configuration/external_parameters.html

### Session proxy
https://symfony.com/doc/4.0/session/proxy_examples.html

### How to use session handling of legacy application using symfony session
https://symfony.com/doc/4.0/session/php_bridge.html

### locale sticky
https://symfony.com/doc/4.0/session/locale_sticky_session.html

### anonymous user and session
https://symfony.com/doc/4.0/session/avoid_session_start.html

### Where sessions saved?
https://symfony.com/doc/4.0/session/sessions_directory.html

### Twig bridge
https://symfony.com/doc/4.1/reference/twig_reference.html

### The Yaml Component
https://symfony.com/doc/4.0/components/yaml.html

### Yaml dump
```php
/**
 * Dumps a PHP value to YAML.
 *
 * @param mixed $input  The PHP value
 * @param int   $inline The level where you switch to inline YAML
 * @param int   $indent The level of indentation (used internally)
 * @param int   $flags  A bit field of Yaml::DUMP_* constants to customize the dumped YAML string
 *
 * @return string The YAML representation of the PHP value
 */
public function dump($input, int $inline = 0, int $indent = 0, int $flags = 0): string
```

### What is default Yaml::dump indentation?
By default the YAML component will use 4 spaces for indentation

