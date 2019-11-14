## Mockery

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