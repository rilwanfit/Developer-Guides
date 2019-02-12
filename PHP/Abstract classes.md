## Interfaces and Abstract Classes

### Abstract class

*   Interfaces and abstract classes are added in PHP 5.
*   Both are used to create a series of constraints on the base design of a group of classes.
*   An abstract class essentially defines the basic skeleton of a specific type of encapsulated entity.

For example, you can use an abstract class to define the basic concept of "car" as having two doors, a lock, and a method that locks or unlocks the doors.

*   Abstract classes cannot be used directly /  can not be directly instantiated; they must be extended so that the descendent class provides a full complement of methods.
*   Abstract classes May contain method implementations
*   Abstract methods must be implemented in derived classes
*   Visibility can become weaker / More permissive, but NOT stronger / less permissive (ex: you can not go from public to private)
*   Abstract Classes as a way to ensure a certain level of code quality because they enforce certain standards and can reduce the amount of duplicate code that you have to write.

```php
abstract class DataStore_Adapter {
  private $id;
  abstract function insert();
  abstract function update();
  public function save() {
    if (! is_null($this->id)) {
      $this->update();
    } else {
      $this->insert();
    }
  }
}
```

```php
class PDO_DataStore_Adapter extends DataStore_Adapter {
  function insert() {}
  function update() {}
}
```
You must declare a class as abstract so long as it has (or inherits without providing a body) at least one abstract method.

As you can see, in this example we define a class called DataStore_Adapter and declare two abstract methods called insert() and

update(). Note that these methods don't actually have a body—that's one of the requirements of abstract classes—and that the class itself must be declared as abstract in order for the compiler to satisfy the parser's syntactic requirements. We then extend **DataStore_Adapter** into **PDO_DataStore_Adapter**, which is no longer abstract because we have now provided implementations for both insert() and update().


#### When should you use Abstract Classes?
whenever you have multiple implementations of a certain bit of functionality.

For example an Audi, a BMW or a Land Rover are all different types of cars and so inheriting from a AbstractCar class would make a lot of sense. If you can say to yourself, "an Audi is a type of Car, it usually make sense to structure your code around the use of an Abstract class.


### Interfaces

*   Interfaces, are used to specify an API that a class must implement.
*   This allows you to create a common contract that your classes must implement in order to satisfy certain logical requirements. For example, you could use interfaces to abstract the concept of database provider into a common API that could then be implemented by a series of classes that interface to different DBMSs.
*   Interface may inherit from other interfaces using the **extends **keyword
*   When a class implements multiple interfaces, there cannot be any naming collision between methods defined in the different interfaces unless the duplicate methods have same signature

Interface methods contain no body:

```php
interface DataStore_Adapter {
    public function insert();
    public function update();
    public function save();
    public function newRecord($name = null);
}
```

```php
class PDO_DataStore_Adapter implements DataStore_Adapter {
    public function insert() { }
    public function update() { }
    public function save() { }
    public function newRecord($name = null) { }
}
```

In the example above, if you fail to define all of the methods for a particular interface, or all of the arguments for any given interface

method, you will see Fatal error

Remember—a class can only extend one parent class, but it can implement multiple interfaces.

Q: What is the difference between an interface and an abstract class?

*   An interface defines a contract between an implementing class is and an object that calls the interface.
*   An abstract class pre-defines certain behaviour for classes that will extend it.
*   To a certain degree this can also be considered a contract, since it guarantees certain methods to exist.

What is the difference between an interface and an class?

*   Interfaces do not contain business logic, only method signatures that define a template that any classes implementing the interface must contain.
*   Lets take an auto mobile for example. If we were to create and interface for a car we would want to define a few methods like drive, stop, turn left , turn right. This mean that any vehicle that is a car (aka implements the interface car) must have methods for these things, If they do not PHP will throw an error. So if your car is an BMW , Honda or Ford it must be able to stop. How it stops is up to each car (or PHP class) but it must be able to stop. Technically we can decide not to use an interface for cars, but then some types of cars are not forced to have a "stop" method.


## Predefined Interfaces and Classes

[http://php.net/manual/en/reserved.interfaces.php](http://php.net/manual/en/reserved.interfaces.php)


### Explain usage of iteratorAggregate?

Let's say we have a book, and that books contains chapters. As a good-guy OO programmer, you probably will define a Book class and a separate Chapter class to deal with this. So is our Book-class an iterator? Well, it would be nice to be able to iterate over its chapters,  so yeah, it could be an iterator because we could use something like this:

**foreach **($book **as **$chapter) { $chapter->getTitle();

But in order to achieve all this, we must implement the "iterator" interface and deal with the iterator's key(), current(), valid(), rewind() and next() methods inside our book class. We don't really want this because this is not really the concern of the Book-class. The Book-class should just deal with the book and it should not contain methods for iterating chapters. We really like to move that somewhere else but on the other hand, we cannot really move it to our Chapter-class since it would mean the Chapter would need to deal with multiple chapters. The Chapter-class just deals with a single chapter.

So we will be introducing the "iteratorAggregate" which sounds far more complex than it actually is: The iteratorAggregate interface lets us deal with "aggregating" objects. The interface defines a single method called "getIterator()", who's only job is to return an iterator. One of the advantages is that an object that implemented IteratorAggregate, itself can be used through foreach(), just the same way a normal iterator can. Foreach() takes care of finding the actual iterator by calling the object's getIterator() method.

```php
class Book implements IteratorAggregate {
    protected $title;
    protected $author;
    protected $chapters;
    
    function __construct($title, $author) {
        $this->title = $title;
        $this->author = $author;
    }
    
    function getIterator() {
        // This will return the articles. Since they are inside an array, we
        // can use the standard array-iterator.
        return new ArrayIterator($this->chapters);
    }
    
    function addChapter(Chapter $chapter) {
        $this->chapters[] = $chapter;
    }
    
    function getTitle() {
        return $this->title;
    }
    
    function getAuthor() {
        return $this->author;
    }
}
```

```php
class Chapter {
    protected $_title;
    protected $_content;

    function __construct($title, $content) {
        $this->_title = $title;
        $this->_content = $content;
    }

    function getTitle() {
        return $this->_title;
    }

    function getContent() {
        return $this->_content;
    }
}
```

```php

// Create a new book and add chapters to it
$book = new Book("About the iteratorAggregate", "Joshua Thijssen");

$book->addChapter(new Chapter("Foreword", "This is the introduction"));

$book->addChapter(new Chapter("Chapter 1", "Content"));

$book->addChapter(new Chapter("Chapter 2", "Content"));

print "All chapters from the book '" . $book->getTitle() . "', written by ".$book->getAuthor()." :" . PHP_EOL;

foreach ($book as $chapter) {
    print "- " . $chapter->getTitle() . PHP_EOL;
}

// Add another chapter to our book
$book->addChapter(new Chapter("Epilogue", "Famous last words"));

print "All chapters from the book '" . $book->getTitle() . "', written by ".$book->getAuthor()." :" . PHP_EOL;

foreach ($book as $chapter) {
    print "- " . $chapter->getTitle() . PHP_EOL;
}
```

At this point we have separated the concerns of the classes and have a "clean" Book and Chapter class. Makes things easier, testable and in the end, more maintainable.

Further improvements

Where should we add a method on how many chapters there are inside the book? We could add a "getChapterCount()" method to our Book class, or even let it implement "countable" so we could use PHP's standard count() function for it:

```php
class Book implements IteratorAggregate, Countable {
    function count() {
        return count($this->_chapters);
    }
}
```

```php
print "Number of chapters in our book: " . count(new Book());
```

Since we now have an iterator, it's easy to add our own filtering, for instance, limiting the foreach() to the first 2 chapters:

```php
// "change" our iteratoraggregate into a iterator.
$it = new IteratorIterator($book);
// Display the first two chapters only
$it = new LimitIterator($it, 0, 2);

foreach ($it as $chapter) {
   print "- " . $chapter->getTitle() . PHP_EOL;
}
```

It's easy to extend functionality this way without even having to touch chapters or book objects. It's easy to setup, it's maintainable and we can test our code without any problem.


### Predefined iterators

[http://php.net/manual/en/spl.iterators.php](http://php.net/manual/en/spl.iterators.php)


#### When to use iterator_to_array

Mostly I find this useful when I'm working with collections of data as these often present themselves as an object that you can foreach() over, but you can't dump it directly. If the object in question implements the Traversable interface, you can instead pass it into iterator_to_array to get the data as an array.

Another thing to note about iterator_to_array. If you have a RecursiveIterator, on possibly a multi-dimensional array, it will flatten your data.
