---
id: dependency-injection
title: Behat
---

# Dependency Injection


### What is the value of the resource and exclude options?
It can be any valid glob pattern
https://en.wikipedia.org/wiki/Glob_(programming)


### How to explicitly configure a service?
```php
class SiteUpdateManager
{
    public function __construct($adminEmail)
```
and 
```yaml
# config/services.yaml
services:
    App\SiteUpdateManager:
            arguments:
                $adminEmail: 'manager@example.com'
```

$adminEmail should match in both places.

#### Can we use service parameter without hardcoding an email?
Yes,
```yaml
# config/services.yaml
parameters:
    admin_email: manager@example.com
services:
    App\SiteUpdateManager:
            arguments:
                $adminEmail: '%admin_email%'
```

#### How to fetch the parameter value inside a controller?
1. If you extends AbstractController
```php
$adminEmail = $this->getParameter('admin_email');
```

2. Otherwise,
```php
$adminEmail = $this->container->get('parameter_bag')->get('admin_email');
```

### How does the container know which service to use in following scenario?
Multiple services in the container that implement LoggerInterface, such as logger, monolog.logger.request, monolog.logger.php, etc.

Configure container to use one of these as follows
```yaml
# config/services.yaml
services:
    App\Service\MessageGenerator:
        arguments:
            # the '@' symbol is important: that's what tells the container
            # you want to pass the *service* whose id is 'monolog.logger.request',
            # and not just the *string* 'monolog.logger.request'
            $logger: '@monolog.logger.request'
```

```xml
<service id="App\Service\MessageGenerator">
    <argument key="$logger" type="service" id="monolog.logger.request" />
</service>
```

```php
// config/services.php
use App\Service\MessageGenerator;
use Symfony\Component\DependencyInjection\Reference;

$container->autowire(MessageGenerator::class)
    ->setAutoconfigured(true)
    ->setPublic(false)
    ->setArgument('$logger', new Reference('monolog.logger.request'));
```

### How to bind arguments by name or type?
```yaml
# config/services.yaml
services:
    _defaults:
        bind:
            # pass this value to any $adminEmail argument for any service
            # that's defined in this file (including controller arguments)
            $adminEmail: 'manager@example.com'

            # pass this service to any $requestLogger argument for any
            # service that's defined in this file
            $requestLogger: '@monolog.logger.request'

            # pass this service for any LoggerInterface type-hint for any
            # service that's defined in this file
            Psr\Log\LoggerInterface: '@monolog.logger.request'

            # optionally you can define both the name and type of the argument to match
            string $adminEmail: 'manager@example.com'
            Psr\Log\LoggerInterface $requestLogger: '@monolog.logger.request'
```

### How to import many services at once?
you can import many services at once by using the `resource` key. [check here](#how-services-automatically-loads)

### What is container parameters?


### How to get container parameters as a Service?
```php
// src/Service/MessageGenerator.php
// ...

use Symfony\Component\DependencyInjection\ParameterBag\ParameterBagInterface;

class MessageGenerator
{
    private $params;

    public function __construct(ParameterBagInterface $params)
    {
        $this->params = $params;
    }

    public function someMethod()
    {
        // get any param from $this->params, which stores all container parameters
        $sender = $this->params->get('mailer_sender');
        // ...
    }
```
