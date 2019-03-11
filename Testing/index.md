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

### Assertion

It is the heart and soul of unit testing. An assertion comes together with constraint. 

*   assertTrue() : This verifies that a condition is true
*   assertFalse() : This verifies that a condition is false
*   assertEquals() : This verifies that expected and actual values are equal, the same way as the PHP comparison operator ==
*   assertSame() : This is similar to assertEquals() , but it checks whether values are identical, the same way as the === op.
*   assertNull() : This verifies that value is null
*   assertEmpty() : This verifies that value is empty, but it uses the PHPfunction empty() , which means empty can be false, null, '' , []

[https://phpunit.de/manual/6.0/en/appendixes.assertions.html](https://phpunit.de/manual/6.0/en/appendixes.assertions.html)

you can create your own assertion by extending the _PHPUnit\Framework\Constraint\Constraint_ class.


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


### Installation
`composer require --dev phpunit/phpunit`


### 3 Step Setup
#### 1.Composer class autoload
```json
"autoload-dev": {
    "psr-4": {
    "TalentPitch\\Tests\\Unit\\": "tests/unit/"
    }
}
```

#### 2.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit backupGlobals="false"
        backupStaticAttributes="false"
        bootstrap="tests/bootstrap.php"
        colors="true"
        convertErrorsToExceptions="true"
        convertNoticesToExceptions="true"
        convertWarningsToExceptions="true"
        processIsolation="false"
        stopOnFailure="false">

    <testsuites>
        <testsuite name="Unit Test Suite">
            <directory>./tests/unit/</directory>
        </testsuite>
    </testsuites>
        <filter>
            <whitelist processUncoveredFilesFromWhitelist="true">
                <directory suffix=".php">./src</directory>
            </whitelist>
        </filter>
    <php>
        <env name="APP_ENV" value="devRilwan"/>
        <env name="APP_DOMAIN" value="harver.dev"/>
    </php>
</phpunit>
```

#### 3. tests/bootstrap.php
`require_once __DIR__ . '/../vendor/autoload.php';`

### Example test class.
```php
namespace TalentPitch\Tests\Unit\Model\ResultPage;

use PHPUnit\Framework\TestCase;

class CompetenceScoreTest extends TestCase
{
    public function testAddition()
    {
        $this->assertEquals(2, 1 + 1);
    }
    
    public function testSubtraction()
    {
        $this->assertEquals(0.17, (1- 0.83));
    }
    
    public function testMultiplication()
    {
        $this->assertEquals(10, 2 * 5);
    }
    
    public function testDivision()
    {
        $this->assertTrue(2 == (10 / 5));
    }
```

Run **vendor/bin/phpunit**


### Example 2:

We have an array of vegetables, where name is the key and price is the value. We gonna test sorting arrays: asort() and ksort().

```php
public function testAsort()
{
    $vegetables = ['carrot' => 1, 'broccoli' => 2.99, 'garlic' => 3.98, 'swede' => 1.75,];
    
    $sortedVegetables = ['carrot' => 1, 'swede' => 1.75, 'broccoli' => 2.99, 'garlic' => 3.98,];
    
    asort($vegetables, SORT_NUMERIC);
    
    $this->assertSame($sortedVegetables, $vegetables);

}

public function testKsort()
{
    $fruits = ['orange' => 1.75, 'apple' => 2.05, 'banana' => 0.68, 'pear' => 2.75];
    
    $sortedFruits = ['apple' => 2.05, 'banana' => 0.68, 'orange' => 1.75, 'pear' => 2.75];
    
    ksort($fruits, SORT_STRING);
    
    $this->assertSame($sortedFruits, $fruits);

}
```
### Example 03

Returns the largest sum of contiguous integers in an ordered array. if the input is [0, 1, 2, 3, 6, 7, 8, 9, 11, 12, 14] the largest sum is 6 + 7 + 8 + 9 = 30.

the best approach:

1. Use the PHP function usort() to sort an array by values using a user-defined comparison function.
2. Then return the last element of the sorted array, which is going to be the largest group.

```php
public function testGetLargestSumOfContiguousIntegers()
{
    $input = [0, 1, 2, 3, 6, 7, 8, 9, 11, 12, 14,];
    
    $results = [
        'group' => '6, 7, 8, 9',
        'sum' => 30
    ];
    
    $this->assertEquals($results, $this->getLargestSumOfContiguousIntegers($input));
}

public function testCompare()
{
    $a = [0, 1, 2, 3,];
    
    $b = [6, 7, 8, 9,];
    
    // $b > $a
    $this->assertEquals(-1, $this->compare($a, $b));
    
    // $b < $a
    $this->assertEquals(1, $this->compare($b, $a));
    
    // $b = $a
    $this->assertEquals(0, $this->compare($b, $b));
}
```
```php
public function getLargestSumOfContiguousIntegers(array $a)
{
    $arrayGroups = [];
    
    foreach ($a as $item) {
        if (! isset($previousItem)) {
        $previousItem = $item;
        
        $arrayGroupNumber = 0;
        
        }
    
        if (($previousItem + 1) != $item) {
        
        $arrayGroupNumber += 1;
        
        }
    
        $arrayGroups[$arrayGroupNumber][] = $item;
        
        $previousItem = $item;
    }

    usort($arrayGroups, [$this, 'compare']);
    
    $highestGroup = array_pop($arrayGroups);
    
    return [
        'group' => implode(', ', $highestGroup),
        'sum' => array_sum($highestGroup),
    ];
}

public function compare(array $a, array $b)
{
    $sumA = array_sum($a);
    
    $sumB = array_sum($b);
    
    if ($sumA == $sumB) {
        return 0;
    } elseif ($sumA > $sumB) {
        return 1;
    } else {
        return -1;
    }
}
```

### Testing Dependencies


#### Global Variables

you create a $config object when the application starts, and then you use this object almost anywhere in the application (in PHP stored, in $_GLOBALS array).


#### Session Variables

This is a standard way to handle the application state with the stateless HTTP protocol.


#### Usage of Registry

This is a slightly different way of accessing global variables.


#### Other code
```php
class User
{
    private $userId;
    private $firstName;
    private $lastName;
    private $email;
    private $password;
    
    private $salt;
    
    public function __construct(array $options)
    {
        foreach ($options as $key => $value) {
            if (property_exists($this, $key)) {
                $this->{$key} = $value;
            }
        }
    }

    public function createPassword()
    {
        $this->salt = substr(str_shuffle("0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"), 0, 15);
    
        $this->password = sha1($this->password . $this->salt);
    }

    public function verifyPassword($password)
    {
        return ($this->password === sha1($password . $this->salt));
    }

    public function isInputValid()
    {
        if (empty($this->firstName) || empty($this->lastName) || empty($this->email) || empty($this->password) || ! filter_var($this->email, FILTER_VALIDATE_EMAIL)) {
            return false;
        }
    
        return true;
    }
}
```

```php
private function sendActivationEmail()
{
    global $config;
    
    $email = new \Util\Mail($config);
    
    $email->setEmailFrom($config->email);
    
    $email->setEmailTo($this->email);
    
    $email->setTitle('Your account has been activated');
    
    $email->setBody("Dear {$this->firstName}\n Your account has been activated\n");
    
    $email->send();
}
```
```php
public function createUser()
{
    global $config;
    
    if (!$this->isInputValid()) {
        return false;
    }
    
    $this->createPassword();
    
    $db = $config->db;
    
    /* @var $db \PDO */
    $sql = "INSERT INTO users(firstname) VALUES (:firstname)";
    
    $statement = $db->prepare($sql);
    
    $statement->bindParam(':firstname', $this->firstName);
    
    if ($statement->execute()) {
        $this->userId = $db->lastInsertId();
    
        $this->sendActivationEmail();

        return true;
    } else {
        throw new \Exception(implode(':', $statement->errorInfo()));
    }
}
```

```php
class UserManager
    private $db;
    private $email;
    private $config;
    public function __construct(
        \Util\Mail $email,
        \PDO $db,
        $config
    ) {
        $this->email = $email;
        
        $this->db = $db;
        
        $this->config = $config;
    }
    
    private function sendActivationEmail(User $user)
    {
        $this->email->setEmailFrom($this->config->email);
        
        $this->email->setEmailTo($user->email);
        
        $this->email->setTitle('Your account has been activated');
        
        $this->email->setBody("Dear {$user->firstName}\n Your account has been activated\n");
        
        $this->email->send();
    }
    
    public function createUser(User $user)
    {
        if (! $user->isInputValid()) {
            throw new \InvalidArgumentException('Invalid user data');
        }
        
        $user->createPassword();
        
        $sql = "INSERT INTO users(firstname) VALUES (:firstname)";
        
        $statement = $this->db->prepare($sql);
        
        $statement->bindParam(':firstname', $user->firstName);
        
        if ($statement->execute()) {
            $this->userId = $this->db->lastInsertId();
            
            $this->sendActivationEmail($user);
            
            return true;
        
        } else {
            throw new \Exception(implode(':', $statement->errorInfo()));
        }
        
        return false;
    }
```

The following are the problems while testing:

1. Usage of the global variable $config
2. Usage of the \Util\Mail class, which is an external class, and as expected, will send an e-mail
3. Usage of the PDO database connection ( $config->db )

By adding dependencies in your code, you are adding complexity. The more complex the code, the greater the chance of it containing bugs, and it is more difficult to refactor. For example, global variables can be passed to tests very easily by simply setting $config as the global variable. However, the problem is that what starts as a simple passed object usually ends up being more complex. To set up the application's

configuration, you need to create a class loader, error handler, load configuration, and probably much more. Suddenly, you have to start your application and execute hundreds of lines of code; instead of testing one class, you are testing half of your application. 

A simple advice is to avoid global variables and session variables inside your classes.

The following might be basic options of how to test this code:

•	 Refactor code and split logic into an entity and a manager

•	 Use integration testing and connect to the database

•	 Use mocks to create a dummy class that imitates the functionality of the dependent code


### Handling dependencies

*   separate the User class from database access and the Mail class usage.

    The logic is that the User class is an entity, but then we will have a second UserManager class that will allow us to persist (store) the objects of the User class in the database. To test the User class, we will use unit tests, and to test UserManager , we will use integration testing. In our case, we will move sendActivationEmail() and createUser() to the UserManager class.

```php
private $user;
public function setUp()
{
    $this->user = new User([
        'firstName' => 'FirstName',
        'lastName' => 'LastName',
        'email' => 'example@test.com',
        'password' => 'password123'
    ]);
}

public function testValidInput()
{
    $this->assertTrue($this->user->isInputValid());
}

public function testInValidInput()
{
    $this->user->email = null;

    $this->assertFalse($this->user->isInputValid());
}

public function testCreatedPassword()
{
    $this->user->createPassword();

    $this->assertEquals(sha1('password123' . $this->user->salt), $this->user->password);

    $this->assertNotEquals(sha1(null), $this->user->password);
}

public function testEmptyPassword()
{
    $this->user->createPassword();

    $this->assertNotEquals(sha1(null), $this->user->password);
}

public function testValidPassword()
{
    $this->user->createPassword();

    $this->assertTrue($this->user->verifyPassword('password123'));
}

public function testInvalidPassword()
{
    $this->user->createPassword();

    $this->assertFalse($this->user->verifyPassword(null));
}
```

### Integration testing
This verifies the interaction with other components.

If you go down the unit-test-only route, it might be difficult to verify if we can really store/retrieve data from the database. In this

case, it would be a problem. For e-mails, let's say we are not worried about sending e -mails

```php
class UserManagerTest extends TestCase
{
   public function testCreateUser()
   {
       $db = new \PDO('mysql:host=localhost;port=3306;dbname=test','homestead','secret');

       $config = new \stdClass();
       $config->email = 'test@example.com';
       $config->site_url = 'http://example.com';

       $user = new User(['firstName' => 'FirtsName', 'lastName' => 'LastName', 'email' => 'user@example.com', 'password' => 'password123']);
       $email = $this->getMock('Mail');
       $userManager = new UserManager($email, $db, $config);
       $this->assertTrue($userManager->createUser($user));
       $this->assertEquals(sha1('password123'.$user->salt), $user->password);
       $this->assertTrue($user->userId > 0);
   }
}
```

#### Behat

[http://behat.org/en/latest/](http://behat.org/en/latest/)

[https://www.hongkiat.com/blog/automated-php-test/](https://www.hongkiat.com/blog/automated-php-test/)


### Exceptions are expected

A better way to handle unexpected or unwanted situations is to use exceptions. The reason is that you can recover from exceptions by wrapping code into a try-catch statement and then decide what to do, instead of letting it die.


#### Testing errors and exceptions
```php
public function testCreateUserException()
{
   $this->expectException(InvalidArgumentException::class);
   // ...
   $userManager->createUser($user);
}
```



## Test Isolation and Tests Interaction


### Framework TestCase

_abstract class TestCase extends Assert implements Test, SelfDescribing_

*   abstract : This is used because you always extend the PHPUnit\Framework\TestCase class and do not execute this class directly
*   extends _Assert_ : This is the class providing a set of assert methods such as assertTrue() and assertEquals()
*   implements Test : This is a simple interface containing just one method, run() , to execute the test
*   implements SelfDescribing : This is another simple interface containing just one method toString()


### Testing the installation

> phpunit --version

> phpunit CompetenceScoreTest.php

--verbose    	 more detailed information


### Test Statuses

*   . : As you can see in our case, a dot means the test is successful
*   F : You get this result when a test has failed, or an assertion didn't match
*   E : You get this result when an error is triggered during test execution
*   S : You get this result when the test is skipped $this->markTestSkipped()
*   I : You get this result when the test is incomplete or not implemented yet $this->markTestIncomplete()


### PHPStorm setup

1. Setup PHP interpreter
    1. Make sure the path to php binary (/usr/bin/php) and install XDebug
2. Setup PHPUnit installed path (composer autoloader /home/vagrant/Code/tmp/**vendor/autoloader.php**)
3. First PHPUnit Test Class, Alt + Insert PHPUnit->PHPUnitTest
4. Run Test Alt + Shift + F10


### TestDox

It will turn the test method name _testBankBalanceCannotGoIntoOverdraftUnlessAllowed into_ "Bank balance cannot go into overdraft unless allowed".

But be careful: if you have tests that have the same name but you append an integer to the end, TestDox does not know that the two are different.

phpunit --testdox


### Code Coverage

Code coverage is a process that detects usage of your code when execute tests.

phpunit --coverage-text ./

phpunit --coverage-html c:/temp/codeCoverage ./


#### With XDebug

you need to have the PHP CodeCoverage component and [XDebug](http://xdebug.org) installed

--coverage-clover optional/path/to/file for generating Clover-formatted reports that can be read by Jenkins code coverage plugins.

--coverage-html optional/path to create a series of HTML files that you can view in your browser to see code coverage results.

--coverage-php : This generates a serialized PHP_CodeCoverage object for a file. This might come in handy when you want to use it in your application.

--coverage-text : This generates code coverage results in text files.


#### With PHPDBG

[https://hackernoon.com/generating-code-coverage-with-phpunite-and-phpdbg-4d20347ffb45](https://hackernoon.com/generating-code-coverage-with-phpunite-and-phpdbg-4d20347ffb45)


### Code coverage analysis

The Change Risk Anti-Patterns (CRAP) index and the number tell you how complex the code is and how big a risk is it going to be to change this code. A lower number means better code, lower risk when changing this code.


### Configuring the code coverage

you would want to specify what exactly should be or shouldn't be included in the code coverage.

In phpunit.xml , you can use the filter tag to specify what is included or excluded from code coverage by using blacklist to exclude files and whitelist to include files.

### Managing Global State

--no-globals-backup will disable the default backup-and-restore $GLOBALS

• --static-backup will backup and restore static attributes by default

• @backupGlobals annotation can be used to temporarily disable the backup-and-restore functionality for globals

• @backupStaticAttributes does the same, but for static attributes


### Test Environment Configuration

phpunit --configuration /path/to/your/phpunit.xml


## Command-Line Switches

```xml
<phpunit backupGlobals="true" processIsolation="false">
    <!-- other stuff goes here -->
</phpunit>
```


### Process Isolation

laravel/integrated


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


## Mockery

[Mockery](https://github.com/padraic/mockery) is a powerful Mock Object framework for PHP testing.

composer require --dev mockery/mockery

When mock needed



1. Imagine that your code triggers a method that will log a bit of data to a file


#### Tear down

Between each test, you need to clean up Mockery so that any expectations from the previous test do not interfere with the current test.


```
public function tearDown() { Mockery::close();}
```



### Mockery Expectations


#### Should Receive

This sets Mockery's expectation for which method you want to call on the Mock Object. 

When left on its own, it does not require that the method be triggered; the default is zero or more times ( zeroOrMoreTimes() ).

To assert that you require the method to be called once, or potentially more times, a handful of options are available:


<table>
  <tr>
   <td>$this->mock->shouldReceive('all')->once();
<p>
$mock->shouldReceive('method')->times(1);
<p>
$mock->shouldReceive('method')->atLeast()->times(1);
   </td>
   <td>$mock = \Mockery::<em>mock</em>('SomeClass');
<p>
$mock->shouldReceive('getName')->once()->andReturn('MH Rilwan');
   </td>
  </tr>
</table>


[http://docs.mockery.io/en/latest/reference/expectations.html](http://docs.mockery.io/en/latest/reference/expectations.html)


#### Once, Twice, Times 

the **once**() method will call the expected method once on the Mock Object.

the **twice**() will obviously call the method twice, 

and **times**() allows you to specify N times.

For example, say you wanted to find the average amount of Twitter followers of random people, you could use something along the lines of:


```
public function testAverageTwitterFollowers()
{
   $twitter = m::mock('twitter');
   $twitter->shouldReceive('followers')->times(3)->andReturn(203, 344, 4524);
   $analytics = new Analytics($twitter);
   $this->assertEquals(2055, $analytics->average());
}
```



#### With

The **with**() method adds a constraint that this expectation only applies to method calls which match the expected **argument list**.

This means that the expectation only applies to the method when it is called with these exact arguments. This allows you to test methods under different circumstances.

 

For example, say you had a method that accepts input and validates that input against a default set of validation rules. The method might accept an optional closure that would overwrite those default validation rules. Using the with() method you could mock both of these expectations.


<table>
  <tr>
   <td colspan="2" >$mock->shouldReceive('get')->withAnyArgs()->once(); // the default
<p>
$mock->shouldReceive('get')->with('foo.txt')->once();
<p>
$mock->shouldReceive('put')->with('foo.txt', 'foo bar')->once();
   </td>
  </tr>
  <tr>
   <td>public function fire()
<p>
{
<p>
   $this->file->put('foo.txt', $this->getContent());
<p>
}
   </td>
   <td>$mock = \Mockery::<em>mock</em>('File');
<p>
$mock->shouldReceive('put')->with('foo.txt', 'content')->once();
<p>
$this->fire();
   </td>
  </tr>
</table>


This can be extended even further to allow for the argument values to be dynamic in nature, as long as they meet certain criteria. 



1. Perhaps we only wish to ensure that a string is passed to a method.
2. maybe the argument needs to match a regular expression. Let's assert that any file name that ends with .txt should be matched.
3. And as a final (but not limited to) example, let's allow for an array of acceptable values, using the anyOf matcher.

<table>
  <tr>
   <td colspan="2" >
$mock->shouldReceive('get')->with(\Mockery::<em>type</em>('string'))->once();
<p>
$mock->shouldReceive('put')->with('/\.txt$/', \Mockery::<em>any</em>())->once();
<p>
$mock->shouldReceive('put')->with(\Mockery::<em>anyOf</em>('log.txt', 'cache.txt'))->once();
   </td>
  </tr>
</table>


https://robertbasic.com/blog/complex-argument-matching-in-mockery/


#### And Return

the **andReturn**() method allows you to set a return value from the Mock. This is useful when you want to assert that the stage has been correctly set for the view.


```php
public function testArchiveHasPosts()
{
   $this->mock->shouldReceive('all') ->once()->andReturn('foo');

   $this->app->instance('Post', $this->mock);

   $this->call('GET', 'posts');

   $this->assertViewHas('posts');
}
```



#### Never

The **never**() method is useful for asserting that a method should never be called. This is useful to ensure that under certain conditions, your method is following the correct path.

For example, say we had a method that should only be run if the return value of another method is true, we could test it like this:


```php
public function testDoesNotRequestUpdates()
{
    $this->mock ->shouldReceive('expired')->once() ->andReturn('false');
    
    $this->mock ->shouldReceive('update') ->never();
    
    $this->app->instance('Request', $this->mock);
    
    $this->call('GET', 'request/update');

public function fire()
{
    $file = 'foo.txt';
    if (! $this->file->exists($file))
    {
        $this->file->put($file, $this->getContent());
    }
}

public function testDoesNotOverwriteFile()
{
    $mockedFile = \Mockery::mock('File');
    
    $mockedFile->shouldReceive('exists')->once()->andReturn(true);
    
    $mockedFile->shouldReceive('put')->never();
    
    $generator = new Generator($mockedFile);
    
    $generator->fire();

}
```


### **Partial Mocks**

Sometimes you will only want to mock certain methods on an object and let the rest work as normal.

A t**raditional partial mock is where you define which methods y**ou want to mock when the mock is created:


```
\Mockery::mock('MyClass[save, send]');
```


You can also set** passive mock**s, This method does not require that you declare which methods to mock ahead of time.


```
\Mockery::mock('MyClass')->makePartial();
```


With both forms of partial mocks, you need to set the expectations of the methods that you do want to mock.


```php
$mock->shouldReceive('save')->once()->andReturn('true');
```


for the purposes of this example, that a method on your class references a custom global function (gasp) to fetch a value from a configuration file.

```php
class MyClass {
    public function getOptions($option)
    {
        return config($option);
    }

    public function fire()
    {
        $this->getOptions('timeout');
    }
}

public function test()
{
    $mock = \Mockery::mock('MyClass[getOption]');

    $mock->shouldReceive('getOption')->once()->andReturn(10000);

    $this->fire();
}

public function testWithPassiveMock()
{

    $mock = \Mockery::mock('MyClass')->makePartial();

    $mock->shouldReceive('getOption')->once()->andReturn(10000);

    $this->fire();
}
```

### Mocking class properties

use Mockery's partial mocks:

Parts of the class is mocked, but other parts of it is not and functions as the class normally would. In this case we're making a partial mock because we want the attribute setting to work as normal


```php
// MyRepository
   public function updateStuff(MyModel $model)
   {
       if (!$model->exists) throw new Exception;
       if ($this->model->where('foo', '=', $model->foo)->exists()) return false;
       $model->foo = 'foo';
       $model->bar = 'bar';
       return $model->save();
   }

// MyRepositoryTest
   public function testUpdateSomeStuff()
   {
       $model = Mockery::mock('MyModel');
       $model->shouldReceive('where->exists')->andReturn(false);
       $repo = new MyRepository($model);
       $model = Mockery::mock('MyModel')->makePartial();
       $model->shouldReceive('save')->once()->andReturn(true);
       $model->exists = true;
       $this->assertTrue($repo->updateStuff($model));
       $this->assertEquals('foo', $model->foo);
       $this->assertEquals('bar', $model->bar);
   }

```


Partial mocks should in general be avoided when possible. If you need a lot of partial mocks in your tests it might be an indication that your classes are doing too much, and every time you make a partial mock to inject into another class you can not easily be confident that the two classes are only loosely decoupled. That being said, they are an extremely powerful tool - but use them with caution.

[https://www.lutro.me/posts/assertions-on-method-calls-using-mockery](https://www.lutro.me/posts/assertions-on-method-calls-using-mockery)

[Mockery](http://docs.mockery.io/en/latest/) or [Prophecy](https://github.com/phpspec/prophecy) that can provide easier creation of mocks;


## Mock with Closures or Anonymous classes

it's easy for us to create mocks and stubs that are precisely customised for our individual tests with minimal lines of code, and often more readable within our tests as well.


### Simple Mock Objects

Mock objects needn't always reference a class. you might pass an array to the mock method - where, for each item, the key and value correspond

to the method name and return value, respectively.

```php
$mock = \Mockery::mock(['getName' => 'MH Rilwan']);
$mock->getName()
```



## Eloquent 

Mockery usage with laravel
```php
Hash::make($password);

Hash::shouldReceive('make')->once()->andReturn('hashed');
```

### Mock accessor

[https://stackoverflow.com/questions/30664743/how-to-mock-laravels-eloquent-accessor-attribute](https://stackoverflow.com/questions/30664743/how-to-mock-laravels-eloquent-accessor-attribute)


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


## Tape for js

[https://medium.com/javascript-scene/why-i-use-tape-instead-of-mocha-so-should-you-6aa105d8eaf4#.nqeetjqr8](https://medium.com/javascript-scene/why-i-use-tape-instead-of-mocha-so-should-you-6aa105d8eaf4#.nqeetjqr8)



*   

Mockery


```php
$this->getMockery('Swift_Transport_SmtpAgent')->shouldIgnoreMissing()
```



```php
private function mockResponse():Mockery\MockInterface
{
   $body = Mockery::mock(StreamInterface::class);
   $body->shouldReceive('getContents')->andReturn('This is the response body.');

   $response = Mockery::mock(ResponseInterface::class);
   $response->shouldReceive('getStatusCode')->andReturn(200);
   $response->shouldReceive('getBody')->andReturn($body);

   return $response;
}
```


 \



```php
/**
* @return RabbitMQQueue|MockInterface
*/
private function buildRabbitMQQueueMock()
{
   // Create RabbitMQ mock based on the contract/interface
   $payload = [
       'event' => 'event',
       'data'  => ['payload'],
   ];

   $queue = Mockery::mock(RabbitMQQueue::class);
   /** @noinspection PhpMethodParametersCountMismatchInspection */
   $queue->shouldReceive('push')
         ->with('channel', $payload)
         ->once()
         ->andReturn(true);

   return $queue;
}

```


[https://gist.github.com/JeffreyWay/4968363](https://gist.github.com/JeffreyWay/4968363)

[http://culttt.com/2013/07/22/getting-started-with-mockery/](http://culttt.com/2013/07/22/getting-started-with-mockery/)

[https://lumen.laravel.com/docs/5.3/testing](https://lumen.laravel.com/docs/5.3/testing)

[https://github.com/JeffreyWay/Laravel-Test-Helpers/blob/master/docs/05-Model-Helpers.md](https://github.com/JeffreyWay/Laravel-Test-Helpers/blob/master/docs/05-Model-Helpers.md)

[http://ryantablada.com/post/an-easy-way-to-test-eloquent-model-attributes](http://ryantablada.com/post/an-easy-way-to-test-eloquent-model-attributes)

[https://www.lutro.me/posts/mocking-models-using-mockery](https://www.lutro.me/posts/mocking-models-using-mockery)

[http://www.levihackwith.com/how-to-read-and-improve-the-c-r-a-p-index-of-your-code/](http://www.levihackwith.com/how-to-read-and-improve-the-c-r-a-p-index-of-your-code/)

[https://blog.alejandrocelaya.com/2017/02/01/run-phpunit-tests-inside-docker-container-from-phpstorm/](https://blog.alejandrocelaya.com/2017/02/01/run-phpunit-tests-inside-docker-container-from-phpstorm/)


## Codeception


### **Installation**


### **Run only one test from a class**

codecept run tests/acceptance/VisitorCest.php:myTestName

[http://psysh.org/#install](http://psysh.org/#install)



<!-- Docs to Markdown version 1.0β15 -->
