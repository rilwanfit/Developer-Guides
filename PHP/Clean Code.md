---
id: clean-code
title: DDD
---
<!----- Conversion time: 3.446 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Sun Feb 10 2019 10:32:16 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1wRWSNNte8JYrCYDjQVsvqfivwae-rWoP3OpZxvPOiuA
----->



## Legacy

*   It uses page scripts placed directly in the document root of the web server.
*   It has special index files in some directories to prevent access to those directories.
*   It has special logic at the top of some files to die() or exit() if a certain value is not set. • Its architecture is include-oriented instead of class-oriented or object-oriented. • It has relatively few classes. • Any class structure that exists is disorganized, disjointed, and otherwise inconsistent. • It relies more heavily on functions than on class methods. • Its page scripts, classes, and functions combine the concerns of model, view, and controller¹ into the same scope. • It shows evidence of one or more incomplete attempts at a rewrite, sometimes as a failed framework integration. • It has no automated test suite for the developers to run.
1.  Implement An Autoloader.

[https://docs.google.com/document/d/1E5R806_F0m2B4GNxTWMSA7n9o57k9xJM-Lv-1m2ZUAQ/edit#heading=h.pfbwi9yjfmhn](https://docs.google.com/document/d/1E5R806_F0m2B4GNxTWMSA7n9o57k9xJM-Lv-1m2ZUAQ/edit#heading=h.pfbwi9yjfmhn)

Steps to implement psr-0



*   Pick a Single Location For Classes where it has only class files in it.
*   Add autoloader code and Then we will register it with **spl_autoload_register**() early in our bootstrap or setup code, before any classes are called.
    *   Autoloader code can be either of a static method, an instance method, an anonymous function, or a regular global function.

    [http://php.net/manual/en/function.spl-autoload-register.php](http://php.net/manual/en/function.spl-autoload-register.php)

1.  Consolidate classes and functions.

Remove all the includes (include but also require , include_once , and require_once) in the code.


<table>
  <tr>
   <td><strong>require </strong>'includes/setup.php';
<p>
<strong>require_once </strong>'lib/sub/User.php';
<p>
$user = <strong>new </strong>User();
   </td>
   <td><strong>require </strong>'includes/setup.php';
<p>
$user = <strong>new </strong>User();
   </td>
  </tr>
</table>


RegEx:

^[ \t]*(include|include_once|require|require_once).*User\.php

^             	  	Starting at the beginning of each line,

[ \t]*          	followed by zero or more spaces and/or tabs,

(include|...)   	followed by any of these words,

.*             		 followed by any characters at all,

User\.php      	 followed by User.php, and we don't care what comes after.


### Consolidate Functions Into Class Files

autoloading only works for classes.  It would be good to find a way to automatically load the function files as well as the class files. 



*   Convert that function definition file into a class file of static methods


## Dependency injection container

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)


## REDUCE THE NUMBER OF BRANCHES IN A FUNCTION BODY


### Delegate decisions (:Tell, don't ask")

In some cases the decision made by an if statement should be moved to another object that is better informed anyway. For example:


```
if ($a->somethingIsTrue()) {  
    $a->doSomething();  
}
```


might be changed into: `$a->doSomething();`

where the decision making process has been absorbed by the doSomething() method of object $a itself. We will never have to "think" for it anymore and we can always safely call doSomething(). This approach follows the Tell, don't ask principle nicely. I recommend you to look into it and apply it whenever you're asking an object for some information and making a decision for it based on that information.


### Use a Map

Sometimes the number of if, elseif or else clauses can be reduced by introducing a map. For example, this:


```
    if ($type === 'json') {  
    return $jsonDecoder->decode($body);  
} elseif ($type === 'xml') {  
    return $xmlDecoder->decode($body);  
} else {  
    throw new \LogicException(  
        'Type "' . $type . '" is not supported'  
    );  
} 
```


can be reduced to this:


```
$decoders = ...; // a map of type (string) to corresponding Decoder objects  


if (!isset($decoders[$type])) {  
    throw new \LogicException(  
        'Type "' . $type . '" is not supported'  
    );  
}  


return $decoders[$type]->decode($body); 

Using a map like this makes your code Open for extension, closed for modification too
```



### Enforce types

Many if statements can be removed if we take types more seriously. For example:


```
if ($a instanceof A) {  
    // happy path  
    return $a->someInformation();  
} elseif ($a === null) {  
    // alternative path  
    return 'default information';  
}  
```


can be simplified a lot by enforcing $a to be an instance of A:


```
return $a->someInformation();
```



### Return early

Often a branch in a function body isn't really a branch, but a pre- or sometimes a post-condition, like this:


```
// pre-condition  
if (!$a instanceof A) {  
    throw new \InvalidArgumentException(...);  
}  


// happy path  
return $a->someInformation(); 
```


The if statement isn't a functional branch for the execution of this function body. It's merely a check for a pre-condition. In some cases we can delegate checking pre-conditions to PHP itself (i.e. by using proper type hinting). However, PHP won't be able to check all imaginable pre-conditions, so we still need to have some of them in our code. To reduce complexity we should try to return as early as possible if we know that it's not going to work out, if we don't know how to handle the given input, or if we know the answer already.

The visual effect of returning early will be that the "happy path" of your code is not indented anymore:

Following this template for function bodies will make it much easier to read and understand code.


```
    // check precondition  
    if (...) {  
        throw new ...();  
    }  


    // return early  
    if (...) {  
        return ...;  
    }  


    // happy path  
    ...  


    return ...;  
```



## **Clean Code**

Example code:


```
public function getJobs($date)
{
  $ret = array();
  $res = $this->db->query(
      'SELECT * FROM jobs WHERE year=' . date('Y', $date) . ' AND month='
      . date('m', $date)
  );
  $res = $res->fetchAll();
  foreach ($res as $data) {
      $j = new Job(/* $data['...'] */);
      $j->rev = $j->time * $j->euro;
      $ret[] = $j;
  }
  return $ret;
}
```


Summary:

First it fetches some data from a database, then it prepares job objects, calculates some additional data and returns the objects.

Improvements:



1.  Rename local variables to improve readability
1.  Break the code up into logical pieces that belong together.

```
$statement = $this->db->query(
  'SELECT * FROM jobs WHERE year=' . date('Y', $date) . ' AND month='
  . date('m', $date)
);
$rows = $statement->fetchAll();


$jobs = array();
foreach ($rows as $row) {
  $job = new Job(/* $row['...'] */);
  $job->rev = $job->time * $job->euro;
  $jobs[] = $job;
}
return $jobs;
```



Commit each changes (mitigate by squashing the commits of a refactoring into a single one before pushing upstream.)


## Use collections

1.  Consider the following code which loop through an item and check for a condition and if it satisfied return a value;
```php
public function getCheckerFor($file) {
    foreach ($this->checkers as $checker) {
        if ($checker->canCheck($file)) {
            return $checker;
        }
    }
 }
```
It can be improved 
```php
return $this->checkers->first(function($i, $checker) use ($file) {
    return $checker->canCheck($file);
});
```

2.
```php
public function hasCheckerFor($file)
{
    foreach ($this->checkers as $checker) {
        if ($checker->canCheck($file)) {
            return true;
        }
    }
    
    return false;
}
```  

```php
    return $this->checkers->contains(function($i, $checker) use ($file) {
    return $checker->canCheck($file);
    
    });
```

#### Code reviewers guide

[http://misko.hevery.com/code-reviewers-guide/flaw-constructor-does-real-work/](http://misko.hevery.com/code-reviewers-guide/flaw-constructor-does-real-work/)

Commenting code

Readable code will actually require less commenting 


```
// data
$a = [
   ['n'=>'John Smith', 'dob'=>'1988-02-03'],
   ['n'=>'Jane Jones', 'dob'=>'2014-07-08']
];

// iterate the array
for($x=0;$x<sizeof($a);$x++){/*calculate difference*/$a[$x]['a']=(new DateTime())->diff(new DateTime($a[$x]['dob']))->format("%y");}
```


This terse and confusing code calculates the age, in years, of each user record within an array.  Their age is added to the record in the array for later use.


#### Code Style



*   Indentation and spacing
*   [PSR-1](http://www.php-fig.org/psr/psr-1/) and [PSR-2](http://www.php-fig.org/psr/psr-2).

[http://akrabat.com/checking-your-code-for-psr-2/](http://akrabat.com/checking-your-code-for-psr-2/)


#### Code Structure

Code structure includes everything from the directory tree in which the source is stored to the methodologies and libraries used. In the context of readability the important factors are abstraction, choice of control structures, data structures and use of temporary variables.



*   **for loop to a foreach loop** 
*   **splitting the age calculation** into a few steps. 

The value in the foreach loop has been set to assign by reference so that the array will be updated when the value is changed. \



<table>
  <tr>
   <td>foreach($users as &$user) {
<p>
     $u['age'] = calculateAgeOfUser($user);
<p>
}
<p>
unset($u);
   </td>
   <td>function calculateAgeOfUser(array $user)
<p>
{
<p>
   $today = new DateTime();
<p>
   $birthday = new DateTime($user['birthday']);
<p>
   return $today->diff($birthday)->format("%y");
<p>
}
   </td>
  </tr>
  <tr>
   <td><code>$users = [ \
    new User('John Smith', new DateTime('1988-02-03')), \
    new User('Jane Jones', new DateTime('2014-07-08')) \
]; \
 \
foreach ($users as $user) {  \
    echo $user->calculateAge(); \
}</code>
   </td>
   <td>class User
<p>
{
<p>
   private $name;
<p>
   private $birthday;
<p>
   public function __construct($name, DateTime $birthday)
<p>
   {
<p>
       $this->name = $name;
<p>
       $this->birthday = $birthday;
<p>
   }
<p>
   public function calculateAge()
<p>
   {
<p>
       return $this->birthday->diff(new DateTime)->format("%y");
<p>
   }
<p>
}
   </td>
  </tr>
</table>

#### Naming 

*   self documenting code

#### Comments

Comments should state in high level terms what the expected result of the code is. Never what the code is doing in low level terms. They should also document any side-effects that may not be obvious at first glance.

[https://github.com/FriendsOfPHP/PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)


## 6 Principles for Writing Maintainable Code

[http://culttt.com/2014/12/03/6-principles-writing-maintainable-code/](http://culttt.com/2014/12/03/6-principles-writing-maintainable-code/)

*   Teachings to help writing maintainable code DRY, SOLID, YAGNI
*   In addition to those three following are the patterns from DDD blue book. \

1.  Intention-Revealing Interfaces
1.  Side-Effect-Free Functions 
1.  Assertions
1.  Conceptual Contours
1.  Standalone Classes
1.  Closure of Operations


## Don't Repeat Yourself (DRY)

A good example of this is when two different but similar classes require some of the same functionality. Instead of duplicating the code across two different classes, you could create an abstract class which holds the common functionality and then make the other two classes extend the abstract class.

[https://deliciousbrains.com/refactoring-php-code-better-readability/](https://deliciousbrains.com/refactoring-php-code-better-readability/)

[https://medium.com/swlh/writing-highly-readable-code-94da94d5d636#.ld82q9p27](https://medium.com/swlh/writing-highly-readable-code-94da94d5d636#.ld82q9p27)

[http://loige.co/6-rules-of-thumb-to-build-blazing-fast-web-applications/](http://loige.co/6-rules-of-thumb-to-build-blazing-fast-web-applications/)

[https://mattstauffer.co/blog/using-php-cs-fixer-to-fix-up-your-php-code](https://mattstauffer.co/blog/using-php-cs-fixer-to-fix-up-your-php-code)

[http://chrismm.com/blog/4-forgotten-code-constructs-time-to-revisit-the-past/](http://chrismm.com/blog/4-forgotten-code-constructs-time-to-revisit-the-past/)


# Boost Your E-Commerce Shop

[https://polcode.com/en/boost-your-e-commerce-shop-with-elasticsearch/](https://polcode.com/en/boost-your-e-commerce-shop-with-elasticsearch/)


# LEGACY CODE
## Remove Dead Code
   - Dead code in a project makes it much harder to find bugs, as it can be confusing to know if the code is relevant or not.
   - it may also mean removing old dependencies, as the dead code may relu on libraries we would no longer needed
 
```php
function displayAvatar($user) {
   if ($user->gravatar) {
      return getGravatarLink($user->email);
   } elseif ($user->libavatar) {
      return getLibavatarImage($user->email);
   } else {
      return getAvatarLink($user);
   }
}
```  
