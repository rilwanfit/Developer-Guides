---
id: anonymous-functions-and-closures
title: DDD
---
## Dynamic Properties in PHP and StdClass


## Closures

[https://wiki.php.net/rfc/closures](https://wiki.php.net/rfc/closures)



*   Closures allow you to define anonymous functions that can be passed around as callbacks.
*   Closure is nothing but an object representation of anonymous function.
*   Closures can be used as callbacks for any function that accepted traditional callbacks prior to 5.3, such as usort(), array_map, array_filter, array_reduce or array_walk.

<table>
  <tr>
   <td>
$users = ["John", "Jane", "Sally", "Philip"];
<p>
array_walk($users, <strong>function </strong>($name) {
<p>
   <strong>echo </strong>"Hello $name" . <em>PHP_EOL</em>;
<p>
});
   </td>
   <td>Hello John
<p>
Hello Jane
<p>
…..
   </td>
  </tr>
</table>




*   Closures are created simply by defining a function with **NO name**, and assigning it to a variable
*   New closure type-hint
*   A Closure is essentially the same as a Lambda apart from it can access variables outside the scope that it was created.
*   two simple yet powerful features that PHP closure offers us to use:
    *   accessing private data of an object instance and
    *   lazy loading

[http://php.net/functions.anonymous](http://php.net/functions.anonymous)


<table>
  <tr>
   <td>$closure = <strong>function </strong>($who) {
<p>
   <strong>echo </strong>"Hello $who";
<p>
};
   </td>
   <td>
    $closure('Rilwan'); // Hello Rilwan
   </td>
  </tr>
  <tr>
   <td>var_dump($closure('Rilwan'));
<p>
// Hello Rilwan
<p>
Test.php:6: NULL
   </td>
   <td>var_dump($closure);
<p>
class Closure#1 (1) {
<p>
  public $parameter =>
<p>
  array(1) {
<p>
	'$who' =>
<p>
	string(10) "<required>"
<p>
  }
<p>
}
   </td>
  </tr>
</table>


Under the hood, the $closure variable is actually an **instance of the Closure** class. Until PHP 5.4, this was considered an implementation detail and was viewed as unreliable. With the release of PHP 5.4, this has changed, and the Closure class is used to bring additional functionality to closures.


```
class Closure#9 (2) {
   public $static =>
       array(1) {
   'field' => string(5) "title"
       }
   public $parameter =>
       array(2) {
   '$valueA' => string(10) ""
           '$valueB' => string(10) ""
       }
}

```


the $parameter values are the parameters to be passed as run-time arguments when we call the closure, the $static values are the "use" variables set when we create the Closure – although we can't access these properties as we would normally be able to access public properties of an object.


### Scope

A closure encapsulates its scope, meaning that it has no access to the scope in which it is defined or executed. It is, however, possible to inherit variables from the parent scope (where the closure is defined) into the closure with the **use** keyword:


<table>
  <tr>
   <td>function createGreeter($who) {
<p>
   return function () <strong>use</strong> ($who) {
<p>
       echo "Hello $who";
<p>
   };
<p>
}
<p>
$greeter = createGreeter("Rilwan");
   </td>
  </tr>
  <tr>
   <td>$greeter(); // Hello Rilwan
   </td>
  </tr>
  <tr>
   <td><strong>function </strong>createGreeter($who) {
<p>
   <strong>return function </strong>($hello) <strong>use </strong>($who) {
<p>
       <strong>echo </strong>"$hello $who";
<p>
   };
<p>
}
<p>
$closure = createGreeter('World');
<p>
$closure('Hello');  // Hello Rilwan
   </td>
  </tr>
</table>


This inherits the variables by-value, that is, a copy is made available inside the closure using its original name. It is also possible to inherit the variables by-reference, using the reference operator, &:


```
// Make sure to use reference here also
function createGreeter(&$who) {
   return function() use (&$who) {
       echo "Hello $who";
       $who = null;
   };
}
$who = "world";
$greeter = createGreeter($who); // Passed in by-reference
$who = ucfirst($who); // changes to World,
// including the closure reference
$greeter(); // Hello World, changes $who to null
var_dump($who); // null
```



### Using $this

[https://wiki.php.net/rfc/closures/object-extension](https://wiki.php.net/rfc/closures/object-extension)

When closures were first introduced in PHP 5.3, they did not allow access to $this, even when it was created inside of an object scope. However, with PHP 5.4 this has been changed.


```
class foo {
   public function getClosure() {
       return function() { return $this; };
   }
}
class bar {
   public function __construct() { }
}
```


in PHP 5.4 and later, when a closure is defined within an object scope, this is determined by the object within whose scope it is defined (or in which it is inherited for child classes).

This can be changed after the fact by using the **bindTo** and (static) **bind** methods of the closure class.

Using either of these methods will not modify the closure itself; instead it will create a clone of the closure with $**this** modified.

It is possible to change the $**this** independently from the scope, meaning that unlike a regular method you may not have access to all of the methods on $**this** unless the scope is also the same class of which $**this** is an instance.


#### Changing $this dynamically


<table>
  <tr>
   <td colspan="2" >class Greeter {
<p>
   public function getClosure() {
<p>
       return function() {
<p>
           echo $this->hello;
<p>
           $this->world();
<p>
       };
<p>
   }
<p>
}
<p>
class WorldGreeter {
<p>
   public $hello = "Hello ";
<p>
   private function world() { echo "World"; }
<p>
}
   </td>
  </tr>
  <tr>
   <td>$greeter = <strong>new </strong>Greeter();
<p>
$closure = $greeter->getClosure();
<p>
var_dump($closure);
   </td>
   <td><strong>class </strong>Closure#2 (1) {
<p>
 <strong>public </strong>$this =>
<p>
 <strong>class </strong>Greeter#1 (0) {
<p>
 }
<p>
}
   </td>
  </tr>
  <tr>
   <td>$worldGreeter = <strong>new </strong>WorldGreeter();
<p>
$newClosure = $closure->bindTo($worldGreeter);
<p>
$newClosure();
   </td>
   <td><strong>Hello PHP Fatal error:  Uncaught Error: Call to private method WorldGreeter::world() from context 'Greeter'</strong>
   </td>
  </tr>
</table>


Here we have a class **Greeter**, which returns a closure that outputs a property, $this->hello, and then calls a method $this->world(), neither of which exist in the Greeter class.

We then create another—completely separate—class, **WorldGreeter**, which has both the property and method we wish to call.

To allow this to work, we can rebind $**this** to an instance of **WorldGreeter** by calling the **Closure->bindTo()** method on our closure.

**PHP Fatal error** because the **world** method is private and we only changed the object to which $this is pointing, not the scope—it remains as it was before: Greeter.

To fix this, we also need to pass in the class from which our scope should be taken. We can pass either the string 'WorldGreeter' or an instance of the WorldGreeter class as the second argument to bindTo():


```
// Rebind $this and scope to $worldGreeter
$newClosure = $closure->bindTo($worldGreeter, 'WorldGreeter');

$newClosure();
```




We can also rebind using the static bind() method of the Closure class:


#### Using static bind()


```
// Rebind $this and scope to $worldGreeter
$newClosure = Closure::bind($closure, $worldGreeter, 'WorldGreeter');

$newClosure();
```


To unbind a closure, pass in null for the new $this.


### Lazy Loading With PHP Closure:

By definition, lazy loading will help preventing any initialization until it is used.


```
use Monolog\Logger;
use Monolog\Handler\StreamHandler;

$logClosure = function() {
   $log = new Logger('event');
   $log->pushHandler(new StreamHandler("logfile.log", Logger::DEBUG));
   return $log;
};

//logger will not be initialized until this point
$logger = $logClosure();
```



### The new Closure::fromCallable() in PHP 7.1

[https://wiki.php.net/rfc/closurefromcallable](https://wiki.php.net/rfc/closurefromcallable)

easily converting any **Callable** into a proper **Closure** using the new Closure::fromCallable() method.


### Creating a closure that wraps a private method

Another scenario where you'd want a closure is if you want to pass in an array-callable of a private method. As mentioned in[ the PHP docs](http://php.net/manual/en/language.types.callable.php), you can create a callable to a method on an object by using an array with this format: [$object, 'method']. This can be quite convenient when using a method on your class that has the logic for the callback:


```
$due = $orders->filter([$this, 'isDue']);
```


This works well, with the only caveat being that the isDue method now has to be made public - otherwise the collection class won't be able to access it.

This is where Closure::fromCallable() once again comes to the rescue. When used from within a class, the resulting closure will automatically be bound to that class, allowing it to access private methods:


```
$due = $orders->filter(Closure::fromCallable([$this, 'isDue']));
```


The isDue method can now stay private or protected, but the collection will still be able to access it through the wrapping closure!

[https://www.exakat.io/6-good-practices-for-use/](https://www.exakat.io/6-good-practices-for-use/)

[https://markbakeruk.net/2017/03/05/closures-anonymous-classes-test-mocking-1/](https://markbakeruk.net/2017/03/05/closures-anonymous-classes-test-mocking-1/)


## Callables

In PHP 5.4, callable was added as a type hint for any value that could be called dynamically. This includes:



*   Closures
*   String function names
*   Objects that define the __invoke() magic method
*   An array containing a string class name and a method to call for static method calls (e.g. [className, 'staticMethod] )
*   An array containing an object and a method to call for instance method calls (e.g., [$object, method] )


## Callbacks


### Internal Functions

Prior to PHP 5.3, PHP supported limited types of callbacks.



*   a string containing a valid function name:

    ```
$callback = "myFunction";
usort($array, $callback);
```


*   Array to denote object or static class method calls:

<table>
  <tr>
   <td>
// object method call:
<p>
$callback = [$obj, 'method']; // $obj->method() callback
<p>
usort($array, $callback);
   </td>
   <td>// or static method:
<p>
$callback = ['SomeClass', 'method']; // SomeClass::method()
<p>
usort($array, $callback);
   </td>
  </tr>
</table>




*   Closures in PHP 5.3, you can now use a closure itself, as well as using an object as a callback if it defines the __invoke() magic method.

<table>
  <tr>
   <td>
<strong>class </strong>Sorter
<p>
{
<p>
   <strong>public function </strong>__invoke($a, $b) {
<p>
       // Sort
<p>
   }
<p>
}
   </td>
   <td>$sorter = <strong>new </strong>Sorter();
<p>
usort($sorter);
   </td>
  </tr>
</table>



### Userland Functions

Prior to PHP 5.4, you could only use the simple string callbacks in userland— this was known as a dynamic function call—however, in PHP 5.4 all valid callbacks can be used.


```
// Variable Functions
$callback = "myFunction";
$callback();

// object method call:
$callback = [$obj, 'method']; // $obj->method() callback
$callback();

// or static method:
$callback = ['SomeClass', 'method']; // SomeClass::method()
$callback();

// Closures
$callback = function() { }
$callback();
// Objects with Invoke magic method:
class invokeCallback {
   public function __invoke() { }
}
$callback = new invokeCallback();
$callback();

```


### Anonymous classes
*   Added in PHP 7. Anonymous classes are useful when simple, one-off objects need to be created.

    ```
$util->setLogger(new class {
   public function log($msg)
   {
       echo $msg;
   }
});
```


*   They can pass arguments through to their constructors, extend other classes, implement interfaces, and use traits just like a normal class can:

<table>
  <tr>
   <td>
var_dump(<strong>new class</strong>(10) <strong>extends </strong>SomeClass <strong>implements </strong>SomeInterface {
<p>
   <strong>private </strong>$num;
<p>
   <strong>public function </strong>__construct($num)
<p>
   {
<p>
       $this->num = $num;
<p>
   }
<p>
   <strong>use </strong>SomeTrait;
<p>
});
   </td>
   <td>object(<strong>class</strong>@<em>anonymous</em>)#1 (1) {
<p>
 ["Command line code0x104c5b612":"class@anonymous":<strong>private</strong>]=>
<p>
 int(10)
<p>
}
   </td>
  </tr>
</table>




*   Nesting an anonymous class within another class does not give it access to any private or protected methods or properties of that outer class. In order to use the outer class' protected properties or methods, the anonymous class can extend the outer class. To use the private properties of the outer class in the anonymous class, they must be passed through its constructor:

    ```
class Outer
{
   private $prop = 1;
   protected $prop2 = 2;

   protected function func1()
   {
       return 3;
   }

   public function func2()
   {
       return new class($this->prop) extends Outer {
           private $prop3;

           public function __construct($prop)
           {
               $this->prop3 = $prop;
           }

           public function func3()
           {
               return $this->prop2 + $this->prop3 + $this->func1();
           }
       };
   }
}

echo (new Outer)->func2()->func3(); // 6
```


*   All objects created by the same anonymous class declaration are instances of that very class.

    ```
function anonymous_class()
{
   return new class {};
}

(get_class(anonymous_class()) === get_class(anonymous_class())) // TRUE
```



[http://blog.adamcameron.me/2016/11/php-different-ways-of-defining-one-off.html](http://blog.adamcameron.me/2016/11/php-different-ways-of-defining-one-off.html)


#### Dynamically applying[ Traits](http://php.net/manual/en/language.oop5.traits.php) to a class at run-time.



##### GET_CLASS_METHODS($object)
