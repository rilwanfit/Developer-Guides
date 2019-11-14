# Automated Test
Test written in code to test code.

## Types of automated test

### Testing pyramid

**Unit test**: It should have strong foundation of using test. Test covering very small amount of code.
**Integration**: Tests multiple modules in the system for compliance. Ex: test own system against database

**Acceptance**: Test the system for high-level compliance.

UI: Enter through the UI and test a specific flow.


This pyramid reflect the quantity of the tests that you should have in relation to one another in order to have a well-tested application.

[https://martinfowler.com/bliki/TestPyramid.html](https://martinfowler.com/bliki/TestPyramid.html)

## Acceptance Test
What is acceptance test?
Does the application as a whole behave how you'd expect? With this mind write acceptance tests.

*   One acceptance test will cover end-to-end, it doesn't cover entire application.
*   Acceptance test are written for specific scenario: Given a set of inputs. Do I get these output? 
*   Suite of acceptance tests that cover the behaviors we care about.
*   check the higher-level requirement.
*   Run automatically


### How to write acceptance test?


#### Anotomy

1. **Setup** - start the system being tested
2. Invoke
3. Verify - assert it contains what was expected

API example:

1. API running
2. Call API
3. Verify


##### Setup
*   We do not need: if the system already running locally
*   If tests are running on CI build, you might want to set your tests up to hit **stage** instance


##### Invoke
*   In the case of API we will make HTTP request
*   Assert the results matches the expectation


##### Verify

```
TestUserProfileEndPoint() {
   Set userId to new guid
   Set name to "rilwan"
   Save user info to database  ---> Setup

   Hit api endpoint with userId
   Save api response as actual  ---> Invoke

   Verify actual.userId equals userId
   Verify actual.name equals name  --> Verify

   Delete user from database   --> cleanup
}
```



#### Arrange, Act and Assert

setup , Invoke and Verify


#### Red, Green Refactor

Split Test

Sitespect

*   

## UnitTest

### What is a unit test?
A unit test is code that executes another piece of code (function/method) in a known context with known inputs, and compares the output to the expected values. This is also called an assertion.

### The importance of unit testing
*   give us confidence that written code works as expected
*   Reduces the risk of introducing bugs
*   Best investments for expends
*   Documentation
*   Refactoring 


### What makes a good test?
*   Independent: Each test needs to run independently from other test and environments.
*   Fast: To have useful tests and be able to run them as often as possible (for example, as pre- or post-commit hooks), tests need to be fast.
*   Repeatable: You should be able to run a test as many times as you want with the same result.
*   Up to date: Tests are written once, but code can be changed or extended.

If you are not going to keep tests up to date, the initial investment in tests will be just a waste of time and money. The rule is, whoever breaks the test, fixes the test.

*   Short: Tests should be just a few lines—easy to read and understand.
*   Resilient: Once written, tests shouldn't change till the behavior of tested class/method changes.


### When to write tests
*   TDD
*   Class first and then verifies the tests cases. Make sure you do that in the same day
*   Must have feature is to have the entire core functionality covered.

You might be tempted to put in the code print statement or use var_dump(). This might also be the moment to write the test, to understand what the code is doing, and verify the functionality without needing to open a browser page. Tests are a good way of learning about your  code.

## Test Doubles

*   Help to eliminate dependencies such as other code, database, or 3rd-party APIs.
*   Replace complex code and simply focus on testing the isolated code.

[https://phpunit.de/manual/current/en/test-doubles.html#test-doubles.stubs](https://phpunit.de/manual/current/en/test-doubles.html#test-doubles.stubs)

**Mocks vs. Stubs?**

Mock refers to the process of defining expectations and ensuring desired behavior. In other words, a mock can potentially lead to a failed test.

A stub, on the other hand, is simply a dummy set of data that can be passed around to meet certain criteria.


### Creating Test Doubles

Two ways available. 


1. $double = $this->**getMock**('MyClass');
2. $double = $this->**getMockBuilder**('MyClass')->**getMock**();

Todo:  check parameters of getMock()



1. string $originalClassName : This is a class name from which the double will be created.
2. array $methods : This will replace all methods by default and will return Null . If the name of the methods are passed in an array, then only these methods will be replaced and the original methods stay untouched (partial mock)
3. array $arguments : These arguments are used for the original constructor.
4. string $mockClassName : This indicates the class name for the created test double.
5. boolean $callOriginalConstructor : This is used when you want to enable/disable the original constructor call.
6. boolean $callOriginalClone : This is used when you want to enable/disable the original clone method usage.
7. boolean $callAutoload : This is used when you want to disable __autoload() for loading classes.
8. boolean $cloneArguments : This allows to enable/disable cloning of all object parameters.
9. boolean $callOriginalMethods : This argument allows to call the original methods, when test double is used
10. Object $proxyTarget : This calls the original object for the test proxy.

$double = $this->getMockBuilder('MyClass')->enableOriginalClone()->enableArgumentCloning()->getMock();

When we create test doubles, PHPUnit uses reflection to create a modified version of your class, which will probably look similar

but will behave as though it's configured to the original class.

```php
class MockTester
{
    public function getOne()
    {
        return 1;
    }
    
    public function getTwo()
    {
        return 2;
    }
}

    public function testOne()
    {
        $mockTester = $this->getMock('MockTester');
        
        $this->assertEquals(1, $mockTester->getOne());
        
        $this->assertEquals(2, $mockTester->getTwo());
    }

    // Failed asserting that null matches expected 1

    .

public function testTwo()
{
    $mockTester = $this->getMock('MockTester', ['getTwo']);
    
    $this->assertEquals(1, $mockTester->getOne());
    
    $this->assertEquals(2, $mockTester->getTwo());
}

// partial mock
// Failed asserting that null matches expected 2
public function testThree()
{
    $mockTester = $this->getMock('MockTester', ['getOne']);
    
    $mockTester->expects($this->any())
    
    ->method('getOne')
    
    ->will($this->returnValue(3));
    
    $this->assertEquals(3, $mockTester->getOne());
    
    $this->assertEquals(2, $mockTester->getTwo());
}
```

### [PHP traits to create test doubles](https://robertbasic.com/blog/php-traits-to-create-test-doubles/)

Keeping your application or library code well organized, easy to follow, and read is important. Your test code should not be exempt from those rules, you should follow good

<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: undefined internal link (link text: " testing conventions"). Did you generate a TOC? </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

[ testing conventions](#heading=h.bqkfxil2rg09).

One part of my tests that I feel like that are out of control are the test doubles. Dummies, fakes, mocks… Seems like they are everywhere and that I keep writing the same ones over and over again.

I do follow some good practices on how to [reduce code duplication](https://refactoring.guru/smells/duplicate-code) in my tests, but these mocks.


```php
<?php declare(strict_types = 1);

namespace Harver\Connector\Tests\Gr8people;

use PHPUnit\Framework\TestCase;

class TransactionTest extends TestCase
{
   protected $asset;
   protected $expense;

   public function setup()
   {
       $this->asset = new Account(new AccountType('asset'), 'Cash');
       $this->expense = new Account(new AccountType('expense'), 'Groceries');
   }

   /** @test */
   public function transaction_can_be_executed_between_asset_and_expense_accounts()
   {
       $transaction = new Transaction($this->asset, $this->expense, '5', 'EUR');

       $result = $transaction->execute();

       self::assertTrue($result);
   }

   /** @test */
   public function transaction_can_be_executed_between_expense_asset_accounts()
   {
       $transaction = new Transaction($this->expense, $this->asset, '5', 'EUR');

       $result = $transaction->execute();

       self::assertTrue($result);
   }
}
```


We create a couple of account types, so we can create a couple of account objects which are then used to test can a transaction be executed or not.

Code in setUp() will be repeated when we introduce more test classes. It is a problem.


#### Traits to the rescue

What if we move the creation of those test doubles to traits?

```php
class AccountTypeTrait
{

public function fakeAssetAccountType() : AccountType

{

return new AccountType('asset');

}

public function fakeExpenseAccountType() : AccountType

{

return new AccountType('expense');

}

}

class AccountTrait
{

public function fakeAssetAccount() : Account

{

return new Account($this->fakeAssetAccountType());

}

public function fakeExpenseAccount() : Account

{

return new Account($this->fakeExpenseAccountType());

}

}

use AccountTrait;
/** @test */

public function transaction_can_be_executed_between_asset_and_expense_accounts()

{

$transaction = new Transaction($this->fakeAssetAccount(), $this->fakeExpenseAccount(), '5', 'EUR');

$result = $transaction->execute();

self::assertTrue($result);

}
```


Each test double can now be reused more easily across different test cases, if for some reason they need to be changed, we change them only in one place.

Just need to be disciplined on the naming of the traits and the methods they provide.


## Types of test doubles


### dummy objects

This is just an empty shell which is not called or used; however, it is used only when you need to pass things such as required arguments.

```php
class Baz
{
    public $foo;
    
    public $bar;
    
    public function __construct(Foo $foo, Bar $bar)
    {
        $this->foo = $foo;
        $this->bar = $bar;
    }
    
    public function processFoo()
    {
        return $this->foo->process();
    }
    
    public function mergeBar()
    {
        if ($this->bar->getStatus() == 'merge-ready') {
        
            $this->bar->merge();
    
            return true;
        }
    
        return false;
    }
}

public function testThatBarMergesCorrectly()
{
    $foo = $this->getMockBuilder('Foo')->getMock();
    
    $bar = $this->getMockBuilder('Bar')
    
    ->setMethods(['getStatus', 'merge'])
    
    ->getMock();
    
    $bar->expects($this->once())
    
    ->method('getStatus')
    
    ->will($this->returnValue('merge-ready'));
    
    // Create our Baz object and then test our functionality
    
    $baz = new Baz($foo, $bar);
    
    $expectedResult = true;
    
    $testResult = $baz->mergeBar();
    
    $this->assertEquals(
    
    $expectedResult,
    
    $testResult
    
    );

}

public function testMergeOfBarDidNotHappen()
{
    $foo = $this->getMockBuilder('Foo')->getMock();
    
    $bar = $this->getMockBuilder('Bar')->getMock();
    
    $bar->expects($this->any())
    
    ->method('getStatus')
    
    ->will($this->returnValue('pending'));
    
    $baz = new Baz($foo, $bar);
    
    $testResult = $baz->mergeBar();
    
    $this->assertFalse(
    
    $testResult,
    
    'Bar with pending status should not be merged'
    
    );

}
```


Our dummy object in this test is the Foo object we created.


### test stubs

This returns predefined values for the method that is called or null for other methods. Sometimes, they are also called indirect input to the tests.


### test spies

This is similar to the stub. It just remembers returned values that can be verified later


### test mocks

The simplest definition of this double is a stub with expectations.

An expectation is the specification of the method on when and how it should be called during a test execution.


### test fakes

This imitates the real object functionality, but is written and used only for tests.


#### createMock


### Mocking an Eloquent Model

Test Controller for an Eloquent database save() by Mocking the model. 


```php
class AppControllerTest extends TestCase {
   public function setUp() {
       parent::setUp();
       Session::start();
       Mail::pretend();
   }
   public function tearDown() {
       parent::tearDown();
       \Mockery::close();
   }
   public function testPostApp() {
       $myvar = array();
       $this->mock = \Mockery::mock('Eloquent','EventRsvp');
       $this->app->instance('EventRsvp', $this->mock);
       $this->mock
           ->shouldReceive('save')
           ->once()
           ->andReturn('true');
       $response = $this->call('POST', '/3tDYSL0', $myvar);
   }
}
```



```php
class AppController extends BaseController {
    public function saveApp($shortUrl){
        $rsvp = new EventRsvp;
        $rsvp->fieldone = '124';
        $rsvp->fieldtwo = '30233';
        $rsvp->save();

        $returnredirect = Redirect::to(Request::path(). '/complete');
        return $returnredirect;
    }
}
```



```php
class EventRsvp extends Eloquent {

    protected $guarded = array('id');
    use Illuminate\Database\Eloquent\SoftDeletingTrait;
    protected $dates = ['deleted_at'];

    public function relationshipone()
    {
        return $this->belongsTo('RelationshipOne','idone');
    }

    public function relationshiptwo()
    {
        return $this->belongsTo('RelationshipTwo','idtwo');
    }
}
```


Mocks?

A Mock Object is basically just a simulated version of a real object from within your application. Once your application reaches a certain level of complexity, certain objects will depends on certain other objects. By simulating the dependent objects, we can isolate only the things we want to test.

Benefits:

You can tell the mock object what methods you want to call, in what order and what parameters it should accept. You can then tell the Mock Object what you expect it to return.


## Hamcrest

[https://packagist.org/packages/hamcrest/hamcrest-php](https://packagist.org/packages/hamcrest/hamcrest-php)


## Automatically Run Unit Tests With entr


### Introduction

I do a lot of test driven development, and generally use tests a lot when I'm writing code. It's nice to have your tests automatically run every time you save a file, so you know if you broke something.

This guide will show you how to make that happen. Since I mostly do PHP development, this guide will focus on using PHPUnit, but this would work for any test runner.


### Entr

[Entr](http://entrproject.org/) is a great little unix utility that does one thing well - automatically execute a task when something changes. There are other utilities like[ watchman](https://facebook.github.io/watchman/) plus a million node projects, but I like entr because it's simple.


### Installation

sudo apt-get install entr

[https://bitbucket.org/eradman/entr/](https://bitbucket.org/eradman/entr/)


## **Usage**

Now that entr is installed, let's use it to run phpunit!


```
find src/ | entr -c phpunit
```


 \


Ok so here is what's happening.


```
find src/
```


 \


This command will list all the files in my src directory. You can change this to include whatever directories you want to watch.


```
|
```


 \


This is a[ unix pipe](https://en.wikipedia.org/wiki/Pipeline_%28Unix%29) to pipe the output of find in to entr, so entr knows what files to watch.


```
entr -c
```


 \
The -c flag tells entr to clear the terminal before invoking the command you specified. For more flags, checkout man entr.


```
phpunit
```


The command to run.


### In Action

[https://yuloh.github.io/2016/automatically-run-unit-tests-with-entr/](https://yuloh.github.io/2016/automatically-run-unit-tests-with-entr/)

[http://www.sitepoint.com/using-selenium-with-phpunit/](http://www.sitepoint.com/using-selenium-with-phpunit/)

[https://medium.com/@kevincennis/on-unit-testing-1cc6798f81ee](https://medium.com/@kevincennis/on-unit-testing-1cc6798f81ee)


## watchman

[https://medium.com/hacker-daily/automatically-running-phpunit-with-watchman-e02757e733e7](https://medium.com/hacker-daily/automatically-running-phpunit-with-watchman-e02757e733e7)


## Creating custom @requires annotations

I was working on a project this weekend that required skipping certain tests in a particular environment (Travis CI). I wrote something like this in my base test class:


```php
private function skipIfTravis()
{
   if (getenv('TRAVIS') === true) {
       $this->markTestSkipped('This test should not run if on Travis.');
   }
}
```


Then in a test, I would use it like this:


```php
public function test_it_can_do_something_that_wont_work_on_travis()
{
   $this->skipIfTravis();

   // Do stuff...
}
```


[https://mattstauffer.co/blog/creating-custom-requires-annotations-for-phpunit](https://mattstauffer.co/blog/creating-custom-requires-annotations-for-phpunit)


## Environment specific variables in Laravel's testing environment

[https://mattstauffer.co/blog/environment-specific-variables-in-laravels-testing-environment](https://mattstauffer.co/blog/environment-specific-variables-in-laravels-testing-environment)



*   We should write unit tests in order to prevent defects from being deployed to production.
*   Essential ingredients of unit test?

We will use PHPUnit's --filter flag to only run our latest test:

phpunit --filter=show_should_return_a_valid_ad


## TDD



*   Can reduce bug density
*   Can encourage more modular designs
*   Can reduce code complexity


## Clean code architecture

[https://www.sitepoint.com/clean-code-architecture-and-test-driven-development-in-php/](https://www.sitepoint.com/clean-code-architecture-and-test-driven-development-in-php/)


## Snapshot Testing in PHPUnit

[https://medium.com/@sebdedeyne/a-package-for-snapshot-testing-in-phpunit-2e4558c07fe3](https://medium.com/@sebdedeyne/a-package-for-snapshot-testing-in-phpunit-2e4558c07fe3)

[https://facebook.github.io/jest/docs/snapshot-testing.html](https://facebook.github.io/jest/docs/snapshot-testing.html)


## Test Convention

[http://blog.nikolaposa.in.rs/2017/02/13/testing-conventions/](http://blog.nikolaposa.in.rs/2017/02/13/testing-conventions/)

[https://davedevelopment.co.uk/2015/01/14/intro-to-reducing-duplication-in-tests.html](https://davedevelopment.co.uk/2015/01/14/intro-to-reducing-duplication-in-tests.html)


## Test Discipline



1. **Design Aid**: writing tests first gives you a clearer perspective on the ideal API Design. Before you implement, write the test.
2. **Feature documentation (For Developers)**: Test descriptions enshrine in code every implemented feature requirements
3. **Test your developer understanding**: Does the developer understand the problem enough to articulate in code all critical component requirements?
4. **Quality Assurance:** manual QA is error prone. It;s impossible to remember all feature.
5. **Continuous Delivery Aid:** Automated QA affords the opportunity to automatically prevent broken builds from being deployed to production.  

Unit tests don't need to be twisted to serve all of those broad-ranging doals. Rather, it is essential nature of unit test to satisfy all of those needs. These benefits are all side-effects of a well-written test suites with good coverage.


## What's in a Good Unit Test?


## Unit Test as a Bug Report

A failing test should read like a high-quality bug report.

What's in a good test failure bug report?



1. What are you testing?
    1. What component aspect are you testing?
    2. What should the the feature do? What specific behavior requirement are you testing?
2. What should it do?

	Ex: 



1. 'compose() should return a function.'
3. What was the output (actual behavior)?
4. What was the expected output (expected behavior)? 




[https://gist.github.com/JeffreyWay/4968363](https://gist.github.com/JeffreyWay/4968363)

[http://culttt.com/2013/07/22/getting-started-with-mockery/](http://culttt.com/2013/07/22/getting-started-with-mockery/)

[https://lumen.laravel.com/docs/5.3/testing](https://lumen.laravel.com/docs/5.3/testing)

[https://github.com/JeffreyWay/Laravel-Test-Helpers/blob/master/docs/05-Model-Helpers.md](https://github.com/JeffreyWay/Laravel-Test-Helpers/blob/master/docs/05-Model-Helpers.md)

[http://ryantablada.com/post/an-easy-way-to-test-eloquent-model-attributes](http://ryantablada.com/post/an-easy-way-to-test-eloquent-model-attributes)

[https://www.lutro.me/posts/mocking-models-using-mockery](https://www.lutro.me/posts/mocking-models-using-mockery)

[http://www.levihackwith.com/how-to-read-and-improve-the-c-r-a-p-index-of-your-code/](http://www.levihackwith.com/how-to-read-and-improve-the-c-r-a-p-index-of-your-code/)

[https://blog.alejandrocelaya.com/2017/02/01/run-phpunit-tests-inside-docker-container-from-phpstorm/](https://blog.alejandrocelaya.com/2017/02/01/run-phpunit-tests-inside-docker-container-from-phpstorm/)

[http://psysh.org/#install](http://psysh.org/#install)

