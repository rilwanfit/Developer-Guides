## Class Inheritance

One of the key fundamental concepts of OOP is inheritance. This allows a class to **extend** another class, essentially adding new methods and properties, as well as overriding existing ones as needed.

*   A class can inherit from only one class

```php
class a {
   function test() {
       echo __METHOD__ . " called\n";
   }
   function func() {
       echo __METHOD__ . " called\n";
   }
}
class b extends a {
   function test() {
       echo __METHOD__ . " called\n";
   }
}
class c extends b {
   function test() {
       parent::test();
   }
}
class d extends c {
   function test() {
       b::test();
   }
}
```

```php
$a = new a();
$b = new b();
$c = new c();
$d = new d();
$a->test(); // Outputs "a::test called"
$b->test(); // Outputs "b::test called"
$b->func(); // Outputs "a::func called"
$c->test(); // Outputs "b::test called"
$d->test(); // Outputs "b::test called"
```

In this script, we start by declaring a class called a. We then declare the class b, which extends a. As you can see, this class also has a test() method, which **overrides** the one declared in a, thus outputting b::test called. Note, however, that we can still access a's other methods, so that calling $b->func() effectively executes the function in the a class. In this way, b has inherited the methods of its parent a.

Naturally, extending objects in this fashion would be very limiting, since you would only be able to **override** the functionality provided by parent classes, without any opportunity for reuse (unless you implement your methods using different names). Luckily, parent classes can be accessed using the special **parent::** identifier, as we did for class c above.

You can also access any other ancestor classes by addressing their methods by name—like we did, for example, in class d.

The alternative for inheritance is to create an instance of the other class and use that, but that is significantly more work

*   Overridden properties and methods cannot have a lower visibility

Public > protected > private

*   Classes and methods marked with final can NOT be overridden
*   The parameter signature can NOT be "STRICTER" than before OR an E_STRICT Error will be thrown (Except for the constructors)


### Inheriting constructors

```php
class a {
   function __construct($title,$text) {
   }
}
class b extends a {
   function __construct($title,$text,$introduction) {
       parent::__construct($title,$text);
   }
}
```

constructors/destructors, overloading, and magic methods
