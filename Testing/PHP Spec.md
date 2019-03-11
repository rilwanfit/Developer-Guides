# PHPSpec

### What is PHPSpec?
It's unit testing tool but that's not its main job. Its a tool for helps you design your code in a well organized, 
meaningful, and maintainable way.

- Design of your php classes not the design and user experience of FE.

### How to install?
```bash
composer require phpspec/phpspec --dev
```

### Where the executable located by default?
```bash
./vendor/bin/phpspec
```

### Configuration autoload in composer.json

*   Make sure composer's autoloader knows where our classes live. 
*   Make composer notice this change `composer dump-autoloader`

```json
"autoload": {
    "psr-4": {
      "App\\": "src/"
    }
}
```

### How PHPSpec knows to create a code for you?
it require composer json for auto loading and also phpspec.yaml
1. create phpspec.yaml at the root of the project with following content
    - phpspec automatically knows to look for this.

```yaml
suites:
 default:
   namespace: App
   psr4_prefix: App
```

### Generating the Specification
```bash
./vendor/bin/phpspec describe -h
./vendor/bin/phpspec describe App/Creator
```

Each time you want create a new class, you should first use above command to create a corresponding specification class. 

Every function in this class that starts with it_ or its_ will be read by phpspec.

Very important things to understand in spec methods.

1. you're supposed to use the $this variable as if we were inside of the Creator class itself.
 Literally: you treat $this like a Creator object - showing examples of how you want it to work by calling methods on that class - like $this->create() if the Creator class had a create() method.
2. You can also call a huge number of methods that start with `should` or `shouldNot`. 
These are called "matchers" - and they are the way you assert that things are working correctly in phpspec.
> Every matcher start with `should` or `shouldNot`

Note: it violates coding standards! 

1. Missing public before function
2. Method name are written using snake-case

Those are because for one important reason that is Readability


### Generating the Class with run

./vendor/bin/phpspec run -h

Main command in phpspec. ITs job is to look at all of our spec files - and all of the methods inside - and verify whether or not actual class behave like we've described.

It can help us creating the class if we want to.

### Examples
```php
functiokn it_should_default_to_zero_length()
{
    $this->getLength()->shouldReturn(0);
    // same as phpunit, $this->assertEquals(0, $this->getLenght());
}

function it_should_allow_to_set_length()
{
    $this->setLength(3);
    $this->getLength()->shouldReturn(3);
}
```

### How to create a custom inline matcher?
```php
function getMatchers(): array
{
    return [
        'returnZero' => function($subject) {
            if ($subject !== 0) {
                throw new FailureExecption(sprintf(
                    'Returned value should be Zero, got "%s",
                    $subject
                ));
            }
            
            return true;
        }
    ];
}

functiokn it_should_default_to_zero_length_using_custom_matcher()
{
    $this->getLength()->shouldReturnZero();
}
```
by default, `shouldNotReturnZero()` also available.
> Can't reuse inline matcher outside of the class.
> if you pass a parametes, it will be available as 2nd, 3rd.. parameters.

### How to create customer matchers to use globally?
https://github.com/karriereat/phpspec-matchers
