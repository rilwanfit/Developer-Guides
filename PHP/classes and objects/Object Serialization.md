### Serializing Objects

Serialization is the conversion of an object to a series of bytes, so that the object can be easily saved to persistent storage or streamed across a communication link. The byte stream can then be deserialized - converted into a replica of the original object.

PHP is able to automatically serialize most of its variables to strings - Letting you save them into storage like $_SESSION. However, there are some tweaks to avoid exploding scripts and performance problems.


#### Primitives

The **serialize()** primitive function takes a PHP variable as its argument and returns a string from which this variable can be reconstructed: **unserialize($string)** performs the opposite job:

| ```$ php -r 'var_dump(serialize(42));'```        | string(5) "i:42;" |
| ------------------------------------------------ | ----------------- |
| ```$ php -r 'var_dump(unserialize("i:42;"));'``` | int(42)           |


The serialization process is automatic, and you do not have to implement any marker interface. It works on scalars, arrays and objects alike

However, it follows field references:


```bash
$ php -r '$object = new stdClass; $object->field = new stdClass; var_dump(serialize($object));'

string(50) "O:8:"stdClass":1:{s:5:"field";O:8:"stdClass":0:{}}"
```

so if you're referencing an ORM or framework objects from your own serialized ones, you're likely to grab the whole library with it. A simple way to avoid this serialization dependency is to pass the collaborator on the stack instead of injecting it into the object's field:


```php
class MyObject
{
   public function doSomeWork(Zend_View $view)
       // refer to $view instead of $this->view
   }
}
```

#### Forbidden types

Some variable types cannot be easily ported from one OS process to another, and as such are unsuitable for serialization:

*   variables of type _resource_ (open files, streams, old-style connections)
*   objects that store resources inside them (a PDO instance representing a database connection)
*   closures (for some reason probably related to their references established with the _use_ statement)

Every object that composes one of these variables encounter the same problem too: an exception thrown during serialization. Only when this happens you have to resort to custom solutions like the two that follow.


#### __sleep() and __wakeup()

This pair of optional magic methods can tell PHP to serialize just part of an object, resulting in an implementation of the Memento pattern.

__sleep() should return a list of strings, which correspond to the field names representing the state of the object we want to store.

```php
class Connection
{
   protected $connection;
   private $server, $username, $password, $db;

   public function __sleep()
   {
       return array('server', 'username', 'password', 'db');
   }
}
```

This method will be called during serialization to determine what to insert in the representation (excluding for example the closures you defined and stored on $this.)

__wakeup() is called instead of the constructor after deserialization, in order to allow the object to reestablish a link to the current process. It has no arguments, so if you want to define it you'll have to grab the collaborators from some global state (singleton, static, global variable) or recreate them by yourself.

The alternative to a __wakeup() method is a Repository object that passes in collaborators during reconstitution, before returning a valid object.


#### Serializable interface

This interface is an alternative to __sleep(). It allows you to specify what to serialize programmatically, by calling serialize() on a subset of the object's fields.


```php
class Obj implements Serializable
{
   private $data;
   public function __construct() {
       $this->data = "My private data";
   }
   public function serialize() {
       return serialize($this->data);
   }
   public function unserialize($data) {
       $this->data = unserialize($data);
   }
}
```


The interface is an alternative to __sleep() in the sense that you can only use one or the other method, not both.

serialize()/unserialize()

*   __sleep() is executed with serialization, IF available
*   Allows you to specify which properties should be stored (serialized) and which should not be stored.
    *   Can also create/change properties for serialization
*   __wakeup() is executed with unserialization, If available
    *   Ex: to open a database connection unique to the object.

[http://stackoverflow.com/questions/804045/preferred-method-to-store-php-arrays-json-encode-vs-serialize](http://stackoverflow.com/questions/804045/preferred-method-to-store-php-arrays-json-encode-vs-serialize)

[http://jmsyst.com/libs/serializer](http://jmsyst.com/libs/serializer)

[http://jmsyst.com/libs/serializer/master/cookbook](http://jmsyst.com/libs/serializer/master/cookbook)
