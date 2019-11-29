---
id: index
title: Behat
---

### Create a project using composer \
`composer create-project symfony/skeleton the_spacebar`

Symfony 4 runs by default with php server


### Installing the Server


```
composer require server 
./bin/console server:run
```


Better PHPStorm plugins to work with symfony

1.  Symfony Plugin
1.  PHP Annotations
1.  PHP Toolbox
*   Search for symfony and Enable plugin for this project
*   Search for composer and make sure the path to composer.json

TODO: make this work with docker images.


### Create the first page

Route: configuration that defines the URL for a page

Controller: a function that we write that builds the content for the page.


<table>
  <tr>
   <td colspan="2" ><strong>config/routes.yaml</strong>
<p>
<strong>index:</strong>
<p>
<strong>   path: </strong>/
<p>
   <strong>controller: </strong>App\Controller\ArticleController::homepage
   </td>
   <td><strong>src/Controller/ArticleController.php</strong>
<p>
<strong>class ArticleController</strong>
<p>
<strong>{</strong>
<p>
<strong>   public function homepage(): Response</strong>
   </td>
  </tr>
</table>



#### Annotation Routes

Instead of creating routes in yml file we can annotate in the actions.

What exactly are annotations? They're PHP comments that are read as configuration. Everything in one place.



1.  `composer require annotations  `
1.  Comment out the yml configuratiosn
1.  Add route in action 

<table>
  <tr>
   <td colspan="2" >
<strong>  /** @Route("/") */</strong>
<p>
<strong>   public function homepage(): Response</strong>
   </td>
   <td><strong>   /** @Route("/{slug}") */</strong>
<p>
<strong>  public function homepage(string $slug): Response</strong>
   </td>
  </tr>
</table>


Double quote is important in the route.


#### Installing security checker


```


#### composer require sec-checker --dev
./bin/console security:check
```


Flex Aliases  [symfony.sh](https://symfony.sh/).

Flex Recipes

Flex: Install a package using composer, flex helps you to setup all the basic configuratiosn for you :)

### Twig settings
[Here](twig)

#### Debug Toolbar & Profiler

[http://symfony.com/doc/current/setup.html](http://symfony.com/doc/current/setup.html)

[http://phpdeveloper.org/news/22983](http://phpdeveloper.org/news/22983)


# Request to Response


## HttpKernelInterface

```php
namespace Symfony\Component\HttpKernel;
use Symfony\Component\HttpFoundation\Request;

use Symfony\Component\HttpFoundation\Response;

interface HttpKernelInterface

{

    const MASTER_REQUEST = 1;
    
    const SUB_REQUEST = 2;

    public function handle(Request $request, $type = self::MASTER_REQUEST, $catch = true);

```

```php
// public/index.php
$kernel = new Kernel($env, $debug);

$request = Request::createFromGlobals();

$response = $kernel->handle($request);

$response->send();
```

Implementation of this interface will have a capable of converting a given Request into a Response.

Example usage: public/index.php



1.  **Kernel** get instantiated. This is a class specific to your project, and you can find it in src/Kernel.php. It allows you to register your bundles, and to change some major settings, like location of cache directory, logs directory or the configuration file that should be loaded. Its constructor arguments are the name of the environment and whether or not the kernel should run in debug mode.

	Environment can be any string. It is mainly a way to determine which configuration file should be loaded. Symfony 4 will be using folder with environment name. Ex: cache/dev, config/packages/dev or config/routes/dev.

In debug mode you will have



*   A pretty, verbose exception page
*   Elaborate information about the time required to run different part of the application (bootstrapping, database calls, template rendering etc.).
*   Extensive information about requests(using the web profiler and the accompanying toolbar).
*   Automatic cache invalidation: this makes sure that changes to config.yml, routing.yml and the likes will be taken into account without recompiling the entire service container or routing matcher for each request (which would take a lot of time).
1.  A Request object created based the existing PHP superglobals ($_GET, $_POST, $_- COOKIE, $_FILES and $_SERVER). The Request class together with other classes from the HttpFoundation component provide object-oriented ways to wrap the superglobals. These classes also cover many corner cases you may experience with different versions of PHP or on different platforms. It is wise (in a Symfony context) to always use Request to retrieve any data you would normally have taken directly from the superglobals.
1.  Then the handle() method of the AppKernel instance gets called. Its only argument is the current Request object. The default arguments for the type of the request ("master") and whether or not to catch and handle exceptions (yes) will be added automatically.

    The result of this handle() method is guaranteed to be an instance of Response (also from the HttpFoundation component). Finally the response will be sent back to the client that made the request - for instance a browser.



## Booting the kernel

Of course, the magic happens inside the handle() method of the kernel. You will find this method implemented in the Kernel class, which is the parent class of AppKernel

First of all, it is made sure that the Kernel is booted, before the HttpKernel is asked to do the rest. The process of booting includes:



*   Initializing all the registered bundles
*   Initializing the service container


### Bundles as container extensions

Bundles are known amongst Symfony developers as the place to put your own code. Each bundle should have a name that reflects what kind of things you could do with the code inside it. For instance you may have a BlogBundle, a CommunityBundle, a CommentBundle, etc. You register your bundles in config/bundles.php, by adding them to the existing list of bundles:

[https://symfony.com/doc/current/bundles.html](https://symfony.com/doc/current/bundles.html)

bundles are mainly treated as ways to extend the service container, not as libraries of code. This is why you find a DependencyInjection folder inside many bundles, accompanied by a {nameOfTheBundle}Extension class. During the process of initializing the service container, each bundle is allowed to register some services of its own to the service container, maybe add some parameters too, and possibly modify some service definitions before the container gets compiled and dumped to the cache directory:


```
class WebProfilerExtension extends Extension
{
   public function load(array $configs, ContainerBuilder $container)
   {
       $loader = new XmlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
       $loader->load('profiler.xml');

       $configuration = $this->getConfiguration($configs, $container);
       $config = $this->processConfiguration($configuration, $configs);
    
       $container->getDefinition('web_profiler.debug_toolbar')->replaceArgument(4, $config['excluded_ajax_paths']);
       $container->setParameter('web_profiler.debug_toolbar.intercept_redirects', $config['intercept_redirects']);
   }

```


The name of the bundle is actually the key under

which you can set configuration values (for instance in config.yml):

web_profiler:

    debug_toolbar: true


### Creating the service container

[https://symfony.com/doc/current/service_container.html](https://symfony.com/doc/current/service_container.html)


### Registering event listeners

[https://symfony.com/blog/new-in-symfony-4-1-invokable-event-listeners](https://symfony.com/blog/new-in-symfony-4-1-invokable-event-listeners)

How to setup maintenance mode in symfony 4?

[https://12factor.net/config](https://12factor.net/config)


# Security & the User Class


# Database



1.  Install doctrine composer require doctrine

<!-- Docs to Markdown version 1.0β14 -->
