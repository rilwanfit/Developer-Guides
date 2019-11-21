---
id: phpunit
title: PHPUnit
sidebar_label: PHPUnit
---

### Installation
`composer require --dev phpunit/phpunit`

1.Composer class autoload
```json
"autoload-dev": {
    "psr-4": {
    "App\\Tests\\": "tests/"
    }
}
```

2. phpunit.xml
https://gist.github.com/rilwanfit/7777f38a1749635d0f0fbba73dfb909e

Note: this file located in the root of the symfony application

3. tests/bootstrap.php
`require_once __DIR__ . '/../vendor/autoload.php';`

4. add to `.gitignore`

```bash
###> phpunit/phpunit ###
/phpunit.xml
.phpunit.result.cache
###< phpunit/phpunit ###
```

5. Run `./vendor/bin/phpunit`

NOTE: to run the tests in symfony application use `./bin/phpunit`

### If you want to specify configurtion file

`phpunit --configuration /path/to/your/phpunit.xml`

### If you want to run a tests of a specific folder
`./bin/phpunit tests/name_of_folder`

### If you want to run a specific tests
`./bin/phpunit tests/SomeTest.php`

### Assertion

It is the heart and soul of unit testing. An assertion comes together with constraint. 

*   `assertTrue()` : This verifies that a condition is true
*   `assertFalse()` : This verifies that a condition is false
*   `assertEquals()` : This verifies that expected and actual values are equal, the same way as the PHP comparison operator ==
*   `assertSame()` : This is similar to assertEquals() , but it checks whether values are identical, the same way as the === op.
*   `assertNull()` : This verifies that value is null
*   `assertEmpty()` : This verifies that value is empty, but it uses the PHP function empty(), which means empty can be false, null, '' , []

[https://phpunit.de/manual/6.0/en/appendixes.assertions.html](https://phpunit.de/manual/6.0/en/appendixes.assertions.html)

you can create your own assertion by extending the _`PHPUnit\Framework\Constraint\Constraint`_ class.


#### Test Statuses

*   . : As you can see in our case, a dot means the test is successful
*   F : You get this result when a test has failed, or an assertion didn't match
*   E : You get this result when an error is triggered during test execution
*   S : You get this result when the test is skipped `$this->markTestSkipped()`
*   I : You get this result when the test is incomplete or not implemented yet `$this->markTestIncomplete()`



### Write first test
  
- Test class must extend `PHPUnit\Framework\TestCase` and class name ends with `Test`
- All your test methods must be `public`
- Methods name start with `test` or have annotation `@test`
 
```php
namespace App\Tests\Entity;

use PHPUnit\Framework\TestCase;

class DinosaurTest extends TestCase
{
    /** @test */
    public function setting_length(): void
    {
        $dinosaur = new Dinosaur();

        $this->assertSame(0, $dinosaur->getLength());

        $dinosaur->setLength(9);

        $this->assertSame(9, $dinosaur->getLength());
    }
```

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

## Test Hooks
### 1. The setUp Hook
`public function setUp()`
-  PHPUnit will automatically call it before each test.

### 2. The tearDown Hook
`public function tearDown()`
-  PHPUnit will automatically call it after each test.

Note: `setUp` and `tearDown` used to share the code accross tests of a class

### 3. setUpBeforeClass()
-  are called once for the entire class

### 4. tearDownAfterClass
-  are called once for the entire class

### 5. onNotSuccessfulTest


## Exceptions are expected

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

## Data providers
1. public method return array of data
    ```php
    public function getSpecificationTests()
    {
        return [
            // specification, is large, is carnivorous
            ['large carnivorous dinosaur', true, true],
            ['large herbivore', true, false],
        ];
    }
    ```
2. Hooking up the Data Provider
    ```php
    /** @dataProvider getSpecificationTests */
    public function testItGrowsADinosaurFromSpecification(string $spec, bool $expectedIsLarge, bool $expectedIsCarnivorous)
    {
    ```

## Code Coverage

Code coverage is a process that detects usage of your code when execute tests.

`phpunit --coverage-text ./`

`phpunit --coverage-html c:/temp/codeCoverage ./`

## TestDox

It will turn the test method name _testBankBalanceCannotGoIntoOverdraftUnlessAllowed into_ "Bank balance cannot go into overdraft unless allowed".

But be careful: if you have tests that have the same name but you append an integer to the end, TestDox does not know that the two are different.

`phpunit --testdox`

### Upgrade from phpunit 7 to phpunit 8

#### changes 
https://github.com/sebastianbergmann/phpunit/blob/8.0.0/ChangeLog-8.0.md#800---2019-02-01

## Mocks

