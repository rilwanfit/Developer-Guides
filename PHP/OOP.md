---
id: oop
title: DDD
---
<!----- Conversion time: 17.626 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Sat Feb 09 2019 02:44:30 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1E5R806_F0m2B4GNxTWMSA7n9o57k9xJM-Lv-1m2ZUAQ
----->

# OOP vs procedural programming

## OOP Fundamentals

* OOP is based on the concept of grouping code and data together in logical units called **classes**.


*   This process is usually referred to as encapsulation, or information hiding,
*   since its goal is to divide an application into separate entities whose internal components can change without altering their external interfaces.
*   Thus, classes are essentially a representation of a set of functions (also called methods) and variables (called properties) designed to work together and to provide a specific interface to the outside world.
*   It is important to understand that classes are just blueprints that cannot be used directly—they must be instantiated into objects, which can then interact with the rest of the application.

You can think of classes as the blueprints for building a car, while objects are, in fact, the cars themselves as they come off the production line. Just like a single set of blueprints can be used to produce an arbitrary number of cars, an individual class can normally be instantiated into an arbitrary number of objects.


### Declaring a Class

```php
class myClass {  // Class contents go here }
```

you are declaring a class called myClass whose contents will normally be a combination of constants, variables, and functions or methods.

Q: What is wrong with classes that predominantly define getters and setters, that map straight to it's internal members, without actually having methods that execute behaviour?

A: This might be a code smell since the object acts as an ennobled array, without much other use.


### Instantiating an Object

Once you have declared a class, you need to instantiate it in order to take advantage of the functionality it offers. This is done by using the **new** construct:


```
$myClassInstance = new myClass();
```


It is also possible to dynamically instantiate a class:


```
$className = "myClass";
$myClassInstance = new $className();
```




*   An object is created unless it has a constructor defined that throw an exception
*   Classes should be defined "PRIOR" to instantiation
    *   With autoloading, a class can be defined (loaded) at the moment it is required by the new Operator
    *   Assigning an existing object to a new variable, OR passing as a function parameter, results in a reference to the same object.


### Duplicating Objects

Since PHP 5.0, objects are treated differently from other types of variables. An object is **always passed by reference** (in reality, it is passed by handle, but for all practical purposes there is no difference), rather than by value.
```php
$myClassInstance = new myClass();
$copyInstance = $myClassInstance;
$cloneInstance = clone $myClassInstance;
```

In this case, both $myInstance and $copyInstance will reference the same object, even though we didn't specify that we wanted this to happen by means of any special syntax.

If you wish to create an actual second copy of an object, you can use the **clone** operator:

When you do this, PHP will create an exact duplicate of the object $myClassInstance and assign it to the $cloneInstance variable. This behavior can be modified if the __clone() magic method is defined for the class.

Cloning by default copies all the properties, but uses assignment, NOT clone, so cloning in shallow by default


### Converting Object to String

The magic method __toString() is called, if available.


*   Includes print , string interpolation, operation with strings, calling functions that expect strings….


### Comparing Objects

[http://php.net/manual/en/language.oop5.object-comparison.php](http://php.net/manual/en/language.oop5.object-comparison.php)


## Class Methods and Properties

*   classes can contain both methods and variables (properties).
*   Methods are declared within a class just like traditional functions: From outside the scope of a class, its methods are called using the indirection operator ->

```php
$obj = new Class(); $obj->test();
```

Naturally, the _$obj _variable is only valid within the scope of our small snippet of code above, which leaves us with a dilemma: how do you reference a class method from within the class itself?


```
class myClass {
   function myFunction($data) {
       echo "The value is $data";
   }
   function callMyFunction($data) {
       // Call myFunction()
       $this->myFunction($data);
   }
}
```


PHP defines a special variable called **this**. This variable is only defined within an object's scope, and always points to the object itself:

It is also possible to call methods dynamically, using the $obj->$var() or $obj->{expression}() syntax:


```
$method = 'callMyFunction';
$obj->$method(123);
// or
$obj->{$method}(123);
```


When using the curly brace syntax, you can use any valid expression, for example:


```
$method = 'callMy';
$obj->{$method . 'Function'}(123);
```


NOTE: From PHP 5.6, you can also use instantiation time access, _(new myClass)->callMyFunction(123)_ but this means the object is

temporary, and not reusable. It isn't recommended.


### Declaring and Accessing Properties

Properties are declared in PHP using one of the PPP operators, followed by their name

```
class foo {
   public $bar;
   protected $baz;
   private $bas;
   public $var1 = "Test"; // String
   public $var2 = 1.23; // Numeric value
   public $var3 = array(1, 2, 3);
}
```

Note that, like a normal variable, a class property can be initialized while it is being declared. However, the initialization is limited to assigning values (but not by evaluating expressions). You can't, for example, initialize a variable by calling a function.

```
class foo {
  public $created = time();
}
```

PHP Fatal error:  Constant expression contains invalid operations

That's something you can only do within one of the class's methods (typically, the constructor).

```
class foo {
   public $created;
   public function __construct() {
       $this->created = time();
   }
}
```

Instance variable $created ??

An instance variable belongs to the object and is available as $this->created  in any method inside the class.

*   Class member variables are called properties OR attributes
*   Visible keywords public, private and protected
*   Declared like any variable; If initialized, must be with a constant value


#### Variable variable

```
$this->a = "hello";

$this->b = "hi";

$this->val = "howdy";

$val = "a";

echo $this->{$val}; // outputs "hello"

$val = "b";

echo $this->{$val}; // outputs "hi"

echo $this->val; // outputs "howdy"

echo $this->{"val"}; // also outputs "howdy"

$a = "hello";

$b = "hi"; $val = "a"; echo $$val; // outputs "hello" $val = "b"; echo $$val; // outputs "hi"
```

#### Creating default object from empty value

```
$res = NULL;  \
$res->success = false; // Warning: Creating default object from empty value
```

Ex: 02

PHP will report a different error message if $res is already initialized to some value but is not an object

$res = 33;  \
$res->success = false; // Warning: Attempt to assign property of non-object

In order to comply with E_STRICT standards prior to PHP 5.4, or the normal E_WARNING error level in PHP >= 5.4, assuming you are trying to create a generic object and assign the property success, you need to declare $res as an object of stdClass:

$res = new stdClass(); 		$res->success = false;


#### Call to member function on non-object

```
$a = 1;
$a->grow();
```

In PHP 5.5: Fatal error: Call to a member function grow() on **a non-object**

In PHP 5.6: PHP Fatal error: Call to a member function grow() on **integer**

**PHP 7:	**Fatal error: Uncaught Error: Call to a member function grow() on integer


### Visibility

PHP 5 adds the notion of object method and property visibility (often referred to as "PPP"), which enables you to determine the scope from which each component of your class interfaces can be accessed.

There are four levels of visibility:

**public	-	**The resource can be accessed from any scope.

**Protected  -**The resource can only be accessed from within the class where


        it is defined and its descendants.

**private	- 	**The resource can only be accessed from within the class where  it is defined.

**final		-	**The resource is accessible from any scope, but cannot be overridden in descendant classes.

NOTE :  The final visibility level only applies to methods and classes. Classes that are declared as final cannot be extended

Typically, you should make all API methods and properties public, since you will want them to be accessible from outside of your objects, while you should keep those used for internal operation as helpers to the API calls protected or private. Constructors and destructors—along with all other magic methods (see below)—should be declared as public. In fact, declaring many of the magic methods, like __destruct, __get, and set, as private will result in a warning error and the method will not be called. There are, however, times when you wish to make the constructor private—for example, when using certain design patterns like Singleton or Factory.
```
class foo {
   public $foo = 'bar';
   protected $baz = 'bat';
   private $qux = 'bingo';
   function __construct() {
       var_dump(get_object_vars($this));
   }
}
class bar extends foo {
   function __construct() {
       var_dump(get_object_vars($this));
   }
}
class baz {
   function __construct() {
       $foo = new foo();
       var_dump(get_object_vars($foo));
   }
}
new foo();
new bar();
new baz();
 ```

```bash
 / Output from "foo" itself:
 array(3) {
    ["foo"]=>
    string(3) "bar"
    ["baz"]=>
    string(3) "bat"
    ["qux"]=>
    string(5) "bingo"
 }
 // Output from sub-class "bar":
 array(2) {
    ["foo"]=>
    string(3) "bar"
    ["baz"]=>
    string(3) "bat"
 }
 // Output from stand-alone class "baz":
 array(1) {
    ["foo"]=>
    string(3) "bar"
 }
 ```

The example above creates three classes:

*   foo,
*   bar, which extends foo and has access to all of the public and protected properties of foo,
*   baz, which creates a new instance of foo and can only access its public  properties.
