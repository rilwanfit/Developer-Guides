---
id: automated-tests
title: Behat
---
## Automated Tests

### Unit tests with PHPUnit
https://symfony.com/doc/4.0/testing.html#unit-tests


### How to disable constructor when mocking an object?
`$this->getMockBuilder('My\Class')->disableOriginalConstructor()->getMock()`


### Which Symfony class offers method to test commands?
https://symfony.com/doc/4.0/console.html#testing-commands

### Using PHPUnit, which method allows to specify a mock response on second call?
`$mock->expects($this->at(1)`

### Using PHPUnit, which method allows you to expect an exception to be thrown?
`$this->expectedException('MyException')`

## Functional test
from the docs, "Functional tests check the integration of the different layers of an application (from the routing to the views)."

Simply saying it is an end to end test where you send an HTTP request and get the response from the application and you do assertion based on the response. 
however, you do have a power to go to the database and assert the changes (state) in the persistent layer.

Following could be the best options to consider when you start writing functional tests.

1. Testing with persistence layer.
Make sure to have a separate database for testing env. which can be setup with `.env.test` file as follows:
```yaml
DATABASE_URL=mysql://user:pass@127.0.0.1:3306/database-test?serverVersion=5.7
```

Second, configure PHPUnit with APP_ENV to test
```xml
<php>
    <server name="APP_ENV" value="test" force="true" />
```

2. LiipFunctionalTestBundle 

3. Flushing the database with every test
https://symfony.com/doc/current/testing/database.html#resetting-the-database-automatically-before-each-test

4. Helper traits for testing data. i.e. CreateUsers,
```php
namespace Tests\Helpers;

use App\Entity\User;

trait CreatesUsers
{
    public function makeUser(): User
    {
        $user = new User();
        $user->setEmail($this->faker->email);
        $user->setFirstName($this->faker->firstName);
        $user->setLastName($this->faker->lastName);
        $user->setRoles([User::ROLE_USER]);
        $user->setBio($this->faker->paragraph);

        return $user;
    }
}
```

5. swap services in a container
before symfony 4.1 while writing you tests, you simply cannot access the service from the container and you cannot set another service (a mock service)

in theory, you should not mock anything on functional tests. 
in practice, there may be situations where you don't have a sandbox environment for a 3rd party services

as of symfony 4.1, during testing, symfony will allow you to access the container and changes services as you wish.

```php
public function test_a_user_can_purchase_product()
{
    $paymentProcessorClient = $this->createMock(PaymentProcessorClient::class);
    $paymentProcessorClient->expects($this->once())
        ->method('purchase')
        ->willReturn($successResponse);

    // this is a hack to make the container use the mocked instance after the redirects
    $client->disableReboot();
    $client->getContainer()->set(PaymentProcessorClient::class, $paymentProcessorClient)
}
```

> ps: during a single test symfony kernel may boot up couple of times. 
  basically all the dependencies and leaving your mocked service left out.   
> ps: disable kernel reboot and make sure the same app kernel instance is used during the lifetime of the test method.
 This will ensure that the mocked service will not be lost.

6. Use SQLite in memory
SQLite is serverless, meaning that the SQLite program will write and read all the data from a file.

https://www.sqlite.org/inmemorydb.html
https://www.doctrine-project.org/projects/doctrine-dbal/en/2.10/reference/configuration.html#connecting-using-a-url

```yaml
DATABASE_URL=sqlite:///:memory:
```

7. Testing the Elasticsearch layer
- you can use different index names during testing
- pay attentions to the Elasticsearch’s refresh interval


### base component required to write functional test
```
composer require --dev symfony/browser-kit
composer require --dev symfony/css-selector
```

### functional tests location directory
`tests/Controller`

### functional tests namespace
`App\Tests\Controller`

### Example functional tests
```php
// tests/Controller/PostControllerTest.php
namespace App\Tests\Controller;

use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class PostControllerTest extends WebTestCase
{
    public function testShowPost()
    {
        $client = static::createClient();

        $crawler = $client->request('GET', '/post/hello-world');

        $this->assertEquals(200, $client->getResponse()->getStatusCode());
    }
}
```

### Base class for functional tests
`WebTestCase` 
Which is 
1. an abstract class with a `createClient` static method
2. extends `KernelTestCase`


### What does the `client` object which is created by `createClient` method?
 it does the browser simulation and helps to make http calls into your Symfony application
```php
$crawler = $client->request('GET', '/post/hello-world');
```

### What is the parameter order in the request?
- 1. HTTP method
- 2. URL

> Hardcoding the request URLs is a best practice for functional tests

### What Client Request method returns?
a Crawler instance

### What is Crawler?
A Crawler instance is returned each time you make a request with the Client. It allows you to traverse HTML documents, select nodes, find links and forms.

### Where the crawler located?
`Symfony\Component\DomCrawler`

### how to apply css selector in crawler
```$crawler->filter('html:contains("Hello World")')```
it will return Nodes that match the CSS selector.

### how to apply XPath expression in crawler
```$crawler->filterXpath('h1')```
it will return Nodes that match the XPath expression.

### How to return a node by index
`$crawler->filter('html:contains("Hello World")')->eq(1)`

### How to return first node
`$crawler->filter('html:contains("Hello World")')->first()`

### How to return last node
`$crawler->filter('html:contains("Hello World")')->last()`

### How to return siblings nodes
`$crawler->filter('html:contains("Hello World")')->siblings()`

### How to return all following siblings nodes
`$crawler->filter('html:contains("Hello World")')->nextAll()`

### How to return all preceding siblings nodes
`$crawler->filter('html:contains("Hello World")')->previousAll()`

### How to return all parent nodes
`$crawler->filter('html:contains("Hello World")')->parents()`

### How to return all children nodes
`$crawler->filter('html:contains("Hello World")')->children()`

> Each of these methods returns a new `Crawler` instance

### get the number of nodes stored in a Crawler
`$crawler->filter('ul.tags > li')->count()`

### What is Profiler?
Symfony provides a powerful profiler to get detailed information about the execution of any request.

### How to install profiler
`composer require --dev symfony/profiler-pack`

https://symfony.com/doc/4.0/profiler/data_collector.html
https://symfony.com/doc/4.0/profiler/profiling_data.html
https://symfony.com/doc/4.0/profiler/matchers.html
https://symfony.com/doc/4.0/profiler/storage.html

### How to Use the Profiler in a Functional Test?
https://symfony.com/doc/4.0/testing/profiling.html


### Framework objects access
It's highly recommended that a functional test only tests the response. But under certain very rare circumstances, 
you might want to access some internal objects to write assertions. In such cases, you can access the Dependency Injection Container:

```php
// will be the same container used in your test, unless you're using
// $client->insulate() or using real HTTP requests to test your application
$container = $client->getContainer();
```

> If the information you need to check is available from the profiler, use it instead.

### Client configuration
https://symfony.com/doc/4.0/testing.html#testing-configuration


### Request and response objects introspection
### PHPUnit bridge
### Handling legacy deprecated code