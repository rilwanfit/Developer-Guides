<!----- Conversion time: 31.49 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Mon Feb 11 2019 10:24:41 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1M1HVlskMI8AGF1BwOxvtB1xfdLYihDYwXHbA-5doP3c

ERROR:
undefined internal link to this URL: "#heading=h.6hvmjs35rzu".link text:  encapsulate behaviour in Value Objects
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.f26muux1zymo".link text: Anemic Domain Model
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.el4uke74nlel".link text: Domain Events
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.el4uke74nlel".link text: Domain Events
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.lf8zz8lykkoe".link text: already
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.qgurkma4qh6g".link text: HasEvents trait
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.pkwp8r6sfqvx".link text:  using the ubiquitous language
?Did you generate a TOC?


ERROR:
undefined internal link to this URL: "#heading=h.5qykk1lkiv0".link text: benefits of using Repositories in web applications.
?Did you generate a TOC?

* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server.

WARNING:
You have 4 H1 headings. You may want to use the "H1 -> H2" option to demote all headings by one level.

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 8; WARNINGs: 1; ALERTS: 14.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>
<a href="#gdcalert8">alert8</a>
<a href="#gdcalert9">alert9</a>
<a href="#gdcalert10">alert10</a>
<a href="#gdcalert11">alert11</a>
<a href="#gdcalert12">alert12</a>
<a href="#gdcalert13">alert13</a>
<a href="#gdcalert14">alert14</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



# Domain Driven Design

It is an approach to help us succeed in understanding and building software model designs.

It is about developing knowledge around the business and the technology to provide value.


## Why?



1.  Software should **not make sense only for coders** but also for the business, DDD empathises making sure everybody talks the same language.
1.  Software **priorities** are aligned with business priorities.
1.  Everybody learns and contributes discovering the business domain. And the **Knowledge shared** with everyone and everybody knows what is going on with the business.
1.  There are **no translations** between domain experts, meaning no information loss or tedious syncing. Everyone talks the same language.
1.  The design is the code and the code is the design, the only implemented truth for the common language. Focused on delivering software continuously through agile discovery processes.
1.  DDD provides a framework for strategic and tactical design. Strategic to pin-point the most important areas to develop based on business value and tactical about battle-tested building blocks and patterns.


## How DDD helps?

DDD is an approach for delivering software, focused on three pillars:

1. Ubiquitous Language: 

2. Strategic Design: 

DDD addresses the strategy behind the direction of the business, not just the technical aspects. It helps define the internal relationships and early-warning feedback systems. On the technical side, strategic design protects each business service by providing the motivation for how service-oriented architecture should be achieved.

3. Tactical Design:

DDD provides the tools and the building blocks for iterative software deliverables. Tactical design tools produces software that is correct as maps domain expert's mental model, is testable and less error prone.


### Ubiquitous Language

How to capture this special language?



*   Label with names for actions physical and conceptual domain concepts.
*   Create a glossary of terms and definitions.
*   Capture important software concepts with some kind of documentation.
*   Share and evolve the knowledge collected with the rest of the team.


### When it is needed.

As a rule of thumb, use it to simplify your domain, never to add more complexity.



1.  If your application is data-centric and use-cases evolve around data manipulation and CRUD operations - you do not need DDD.
1.  If your application has less than 30 use-cases, it might be simpler to use a framework.
1.  If more than 30 use-cases, consider using DDD.

DDD helps on growing and is likely changing applications to manage complexity and refactor your model over time.


### Main challenges of applying DDD.

It will require more time and effort to think about the business domain, terminology, research and collaboration with domain experts.


## Architectural Styles.




## Building blocks of DDD

An important part of the beauty of DDD is the fact that we have a language to describe certain concepts and components within our application. Terms like Aggregate Root, Value Object and Entity have concrete meanings that describe how our applications are formed.

Whilst these terms form part of the structure of DDD, and not our application's domain model, it is still important to represent these concepts explicitly in the code we write.

By using the same terminology in how we describe the application, as well as how we write the code, we dispel a lot of the confusion that can arise from application development.


### What is the benefit and why is this important?

In Object Oriented Programming, we represent concepts of our application as objects. Within the scope of a Domain Driven Design project, the objects of the project fit into a number of explicitly defined types.

For example, how we can

<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: undefined internal link (link text: " encapsulate behaviour in Value Objects"). Did you generate a TOC? </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

[ encapsulate behaviour in Value Objects](#heading=h.6hvmjs35rzu). Value Objects are immutable and very important when modelling a domain. On the other hand, objects in which identity is important are represented by[ Entities](http://culttt.com/2014/09/01/user-entity-ubiquitous-language/).

Value Objects and Entities have a number of characteristics that they must possess in order to meet the requirements of that object type.

For example, the equality of Value Objects is based upon the attributes of the object, whereas Entities must have a unique identifier that ensures continuity as the properties of the object change over time.

To ensure that the objects of our application meet these requirements we can define interfaces, extend abstract classes or use traits to ensure that the functionality is implemented.

This also makes our code much more readable because you can simply open up a class and see exactly what type of object it represents and what it is capable of.

There are many Open Source repositories that define these types of interfaces or Value Objects for you. Personally I think it is better to write this type of code for each project as it means the code will actually live inside of your project. It also means you don't have to compromise with generic interfaces or lose some of the semantic meaning from your domain.

[http://culttt.com/2014/04/30/difference-entities-value-objects](http://culttt.com/2014/04/30/difference-entities-value-objects)


## Domain Model


## Encapsulating your application's business rules

Doctrine 2 is an implementation of[ The Data Mapper pattern](http://martinfowler.com/eaaCatalog/dataMapper.html). The Data Mapper pattern promotes the separation of your application's business rules from how the data is persisted to the database. This is in contrast to the Active Record pattern ([What's the difference between Active Record and Data Mapper?](http://culttt.com/2014/06/18/whats-difference-active-record-data-mapper)), where the two are coupled together.

Whilst getting a grasp of how Doctrine works and how it is different to Eloquent is important, we haven't really touched upon the main reason why you would want to choose Doctrine 2 in the first place.

That reason is how you encapsulate the business rules of your application.


## Business Rule

The Business rules of an application are essentially what **makes that application unique** and also business rules **defines how the company operates**. 

Example 01: If you were developing an application to manage the internal company projects, you might have to adhere to the following business rules:



1.  A project must have an assigned client
1.  A project must have a status.

Example 02: registration system



1.  User must have an Email, Username and Password
1.  Email should be valid and unique
1.  A username should only contain alphanumeric, as well as dashes and underscores,  and it should be unique
1.  A password should be more than 8 characters.

Example 03: Time



1.  Should be able to create from time
1.  Should be able to create from hours and minutes
1.  Should be able to create from MinutesSinceMidnight

Each of these business rules are extremely important to adhere to because they are critical to how the business operates.

Each different application will have unique set of business rules. Having a good idea of business rules of a proposed application will be a good starting point to develop a new application.

Business rules should be encapsulated in **entity and Value Objects** that have no dependency or knowledge of the outside world.


### Where should you define business rules?

This is the tricky to get your head around. 



*   For many types of application, defining business rules in Active Record model is fine.
*   For simple application business rules in controller is probably fine too. 
*   However for bigger and more complex application you should be defining your business rules within your project domain.


### Sample application:


#### Folder structure.



*   Domain -> Model
*   Infrastructure
*   Application


#### Encapsulating business rules in Objects -> Value Objects

An Email, Username and Password could all be considered Value Objects within our application. By creating Value Objects out of each of these user requirements we can encapsulate rules around each item within the confides of a dedicated PHP object that has the sole responsibility of being an instance of that thing.

This means whenever we need an instance of an email address, we can create a new EMAIL object

The Email object has the responsibility to ensure that the email address is valid. This means we encapsulate the business rules of our application within the Email class.

The reason why we do not have the email as a string, so you can't be sure that it is a valid email address without running it through some kind of validation.


#### Email Value object - tests


```
class Mhr\Tests\Domain\Model\Identity\ValueObject\EmailTest;
```




1.  Ensure that the Email object will only accept valid email addresses
1.  Simply validate that everything works as expected by passing in an email address that is valid and asserting that the Email object is instantiated correctly.


#### Implementing Email Value object


```
class Mhr\Domain\Model\Identity\ValueObject\Email;
```


NOTE: 

The[ Assert](https://github.com/beberlei/assert) package is used to prevent you from having to litter your code with **if blocks.** It is specifically design for libraries and application low-level code and so it has no dependencies.

[https://github.com/webmozart/assert](https://github.com/webmozart/assert)


## Specification Pattern



*   Not all the validation can be implemented inside Value Object: Unique Email validation - requires DB check.
*   coupling the Value Object to the storage mechanism is **bad** practise.
*   Specification pattern is rescue here.

The Specification Pattern is a way of encapsulating business rules to return a boolean value. By encapsulating a business rule within a Specification object, we can create a class that has a single responsibility, but can be combined with other specification objects to create maintainable rules that can be combined to satisfy complex requirements.

The Specification object has a single public **isSatisfiedBy**() method that will return a boolean value:


<table>
  <tr>
   <td>class UsernameIsUnique implements UsernameSpecification {
<p>
      <strong>public function </strong>isSatisfiedBy(Username $username) : bool {
   </td>
   <td>class UsernameIsUniqueTest extends \PHPUnit_Framework_TestCase {
<p>
   public function setUp()
<p>
   {
<p>
       $this->repository = m::<em>mock</em>(UserRepository::<em>class</em>);
<p>
       $this->spec = new UsernameIsUnique($this->repository);
<p>
   }
   </td>
  </tr>
  <tr>
   <td>public function testShouldReturnTrueWhenUnique()
<p>
{
<p>
   $this->repository->shouldReceive('userByUsername')->andReturn(null);
<p>
   $this->assertTrue($this->specification->isSatisfiedBy(new Username('72391bdiadg7sad')));
<p>
}
   </td>
   <td>public function testShouldReturnFalseWhenNotUnique()
<p>
{
<p>
   $this->repository->shouldReceive('userByUsername')->andReturn(['id' => 1]);
<p>
   $this->assertFalse($this->specification->isSatisfiedBy(new Username('hello')));
<p>
}
   </td>
  </tr>
</table>


The **UsernameIsUnique** Specification object can be implemented using whatever means necessary. In this example you would likely inject a repository to query the database to check if the username was indeed unique.


#### Why would you choose to use The Specification Pattern

The Specification Pattern is powerful because it allows you to encapsulate the business rule inside of the class whilst providing an easy to use API that can be consumed within your application.

By using The Specification Pattern, you can use the Specification Object anywhere that is necessary in your application. Your application does not need to know how the business rule is enforced because it is internal to the Specification object. If the business rules is changed, you only have to change that single source of truth.

Using The Specification Pattern also makes having alternative, or multiple rules easier to work with. By encapsulating each rule as a Specification object you are free to use many instances together to satisfy the complex requirements of the organisation without getting bogged down in complex or unmaintainable code.

[http://marcaube.ca/2015/05/specifications/](http://marcaube.ca/2015/05/specifications/)


#### Implementing Specification Pattern


<table>
  <tr>
   <td>namespace Mhr\Domain\Model\Users;
<p>
class UsernameIsUnique implements UsernameSpecification {
<p>
   private $repository;
<p>
   public function __construct(UserRepository $repository)
<p>
   {
<p>
       $this->repository = $repository;
<p>
   }
<p>
   public function isSatisfiedBy(Username $username) : bool
<p>
   {
<p>
       if ($this->repository->userByUsername($username)) {
<p>
           return false;
<p>
       }
<p>
       return true;
<p>
   }
<p>
}
   </td>
   <td>namespace Cribbb\Domain\Model\Users;
<p>
interface UsernameSpecification {
<p>
    public function isSatisfiedBy(Username $username) : bool;
<p>
}
   </td>
  </tr>
</table>


 the isSatisfiedBy() method by searching the database for a user that has that username.


## The User Entity and Ubiquitous Language

Let's have a look at how we going to create User Entity by using Value objects and Specification Objects.


#### Creating new User.

Legitimate ways of creating new user.


<table>
  <tr>
   <td>$user = new User;
<p>
$user->save();
   </td>
   <td>$user = new User;
<p>
$user->name = 'Philip Brown';
<p>
$user->email = 'name@domain.com';
<p>
$user->save();
   </td>
   <td>$user = User::<em>create</em>([
<p>
   'name' => 'Philip Brown',
<p>
   'email' => 'name@domain.com'
<p>
]);
   </td>
  </tr>
</table>


one of our business rules is "**users must have a email, username and password**". How to enforce this rule? Well, the best way of doing that is also simplest. Use the __constract()


<table>
  <tr>
   <td>class User {
<p>
   public function __construct(Email $email, Username $username, Password $password)
<p>
   {
<p>
       $this->email = $email;
<p>
       $this->username = $username;
<p>
       $this->password = $password;
<p>
   }
<p>
}
   </td>
   <td>$user = new User(
<p>
   new Email('name@domain.com'),
<p>
   new Username('username'),
<p>
   new Password('qwerty')
<p>
);
   </td>
  </tr>
</table>



####  \
What is the Ubiquitous Language and how can we use it?

What martinfowler says: http://martinfowler.com/bliki/UbiquitousLanguage.html

Basically it means the language that is used by the "business experts" to describe the application.

When there are two distinct groups working on an application, developers on one side and business people on the other, a lot of the meaning and nuance of an application can get lost in translation.

However, when everyone agrees on a Ubiquitous Language to talk about the project, and developers use that language in their work, the project is better equipped to stay on track.

In the example above we are saying _we create a new user_: However, do we really _create_ a user? No, a user registers with our application. Instead of "creating" a user we should be using the terms from our Ubiquitous Language:

simply make the __construct method private and created a static register method.

To read more on this topic I highly recommend reading[ Named Constructors in PHP](http://verraes.net/2014/06/named-constructors-in-php/).


```
$user = User::register(
   new Email('name@domain.com'),
   new Username('username'),
   new Password('qwerty')
);
```



#### Using UUIDs instead of Auto-incrementing IDs

You are probably already aware of what auto-incrementing ids are. Basically an auto-incrementing id is the primary key of the record that is set when you create a new record in the database.

A UUID (Universally unique identifier) is an id that is generated by the application, rather than the database.

UUIDs are beneficial for a number of reasons. Firstly, you are not relying on the infrastructure to generate ids for you. The infrastructure shouldn't really leak into your application as the database is just an implementation detail.

Secondly, UUIDs are great if you plan to distribute your infrastructure. Using UUIDs instead of auto-incrementing ids will make it dramatically easier to partition your databases if you are lucky in enough to reach that type of scale.

The downsides to using UUIDs are, you can't easily determine the order of records based on their id, and UUIDs are going to be bigger than auto-incrementing ids. Jeff Atwood has a nice post on using [IDs versus GUIDs](http://blog.codinghorror.com/primary-keys-ids-versus-guids/)

PHP does not have a native UUID class, and so I'm going to be using [this](https://github.com/ramsey/uuid) package. As I mentioned in Encapsulating your application's business rules, I don't mind using these types of packages in my domain related code because if PHP had a native class I would use it, and the package has no third-party dependencies to worry about.


#### UserId Class

Tests: In the tests I'm simply making sure the UserId class is being instantiated correctly. I've also wrote a fromString() method to take string UUIDs and convert them into instances of UserId. This will be useful for when I'm getting records from the database.


<table>
  <tr>
   <td>namespace Mhr\Domain\Model\Identity\ValueObject;
<p>
use Mhr\Domain\Model\UuidIdentifier;
<p>
use Rhumsaa\Uuid\Uuid;
<p>
class UserId extends UuidIdentifier
<p>
{
<p>
   protected $value;
<p>
   public function __construct(Uuid $value)
<p>
   {
<p>
       $this->value = $value;
<p>
   }
<p>
<strong>public static function </strong>fromString(string $userId) : UserId
<p>
{
<p>
   <strong>return new </strong>UserId(Uuid::<em>fromString</em>($userId));
<p>
}
<p>
<em>/**</em>
<p>
<em>* Return the object as a string</em>
<p>
<em>*/</em>
<p>
<strong>public function </strong>__toString() : string
<p>
{
<p>
   <strong>return </strong>$this->value->toString();
<p>
}
<p>
}
   </td>
   <td><?<em>php <strong>namespace </strong></em>Cribbb\Domain\Model\Users;
<p>
<strong>use </strong>Rhumsaa\Uuid\Uuid;
<p>
<strong>class </strong>UserIdTest <strong>extends </strong>\PHPUnit_Framework_TestCase {
<p>
   <em>/** <strong>@test </strong>*/</em>
<p>
<em>   <strong>public function </strong></em>should_require_instance_of_uuid()
<p>
   {
<p>
       $this->setExpectedException('Exception');
<p>
       $id = <strong>new </strong>UserId;
<p>
   }
<p>
   <em>/** <strong>@test </strong>*/</em>
<p>
<em>   <strong>public function </strong></em>should_create_new_user_id()
<p>
   {
<p>
       $id = <strong>new </strong>UserId(Uuid::<em>uuid4</em>());
<p>
       $this->assertInstanceOf('Cribbb\Domain\Model\Users\UserId', $id);
<p>
   }
<p>
   <em>/** <strong>@test </strong>*/</em>
<p>
<em>   <strong>public function </strong></em>should_create_user_id_from_string()
<p>
   {
<p>
       $id = UserId::<em>fromString</em>('d16f9fe7-e947-460e-99f6-2d64d65f46bc');
<p>
       $this->assertInstanceOf('Cribbb\Domain\Model\Users\UserId', $id);
<p>
   }
<p>
}
   </td>
  </tr>
</table>



#### User Entity

#### The User entity


## Repositories


### Value Objects


## Entity

## Domain Events

## Creating and Using a Command Bus

**layers to DDD applications**, each with their own roles and responsibilities within the application stack. 



1.  Domain
1.  Infrastructure
1.  Application


### How do you implement the Application Layer?

Two methods for implementing the Application layer.


#### Application Services

An Application Service is basically just a class with methods for the various tasks your application can perform.


```
class IdentityApplicationService::register($email, $username, $password)
```


The register() method on this Application Service would accept the raw strings from the request and use the internal **RegisterUserService** to create a new User object.

The Application Service would be injected with the various services it requires to complete the task. Application Service would generally separated by functional area of the application and would group together related methods.


#### Command Bus

A Command Bus accepts a Command object and delegates it to a Command Handler.

For example, you might have a **RegisterUserCommand** that is sent to the Command Bus and delegated to a **RegisterUserHandler**.

The **RegisterUserHandler** would be able to handle the process for registering a new user in the application.


### What are the differences between Application Services and The Command Bus?

There are many differences between Application Services and The Command Bus and both have their positives and negatives.



*   Application Services tend to be more complicated than using a Command Bus. This is because grouping together many related tasks means you end up with a large class that requires a number of dependencies and has a lot of responsibility.
*   Using a Command Bus is far more simpler because each Command has a single Command Handler. Each Handler only needs to be concerned with satisfying a single Command, and so your code ends up much closer to The Single Responsibility Principle.

However on the other hand, using a Command Bus can be more complicated than using an Application Service in some situations. A Command Bus should not return a value after a Command has been handled.

For example, if you were creating a new resource in your application, you would not be returned the id of the new resource so you could redirect the user to it's view page.

Now arguably, this is probably only going to make using a Command Bus more difficult if you are trying to use a Command Bus for the wrong job. If you face this situation, you should probably use an Application Service.

However testing A Command Bus is more difficult because it's harder to test code when there is no return value from the method call. 

When you begin writing The Application Layer of your application, you don't need to choose either Application Services or a Command Bus. You can very easily use one type of implementation for certain functionality of your application and another for other aspects. It's about choosing the right tool for the job, and not trying to fit a square peg in a round hole.


### Building the Command Bus

[Creating your own Domain Event Dispatcher](http://culttt.com/2014/11/03/creating-domain-event-dispatcher), you have the option of either using a ready made Command Bus package or rolling your own. Using Open Source packages is a fantastic way of not wasting time by trying to reinvent the wheel.

However as I advocated last week, I do believe that certain components of your application should sit within your walled garden, even if you end up building a generic tool for a generic problem. I think having complete control of the API and future evolution of application critical components is worth the time investment of writing your own code.

The Command Bus infrastructure will be made up of three main components:



1.  **Commands** 

    Commands are the data objects that are passed into The Command Bus.


    It's a strictly defined message, it is immutable. The Command is not more than a Data Transfer Object which can be used by the Command Handler. It represents the outside request structured in a well formalized way.

1.  **Handlers** deal with accepting a Command and completing the required task
1.  **The Command Bus** is responsible for mapping Commands to Command Handlers and instantiating the required objects

Note: Commands only have one Handler, whereas Events have many Listeners.


#### The Command Interface

The Command interface doesn't need to have any method definitions because Commands are basically just plain old PHP objects that hold data.

You might be thinking, what's the point in the interface then? I think there is still a lot of value to implementing interfaces even if the interface does not contain any methods. This is because it makes your code a lot more readable and easier to understand for new developers.


#### The Handler Interface


<table>
  <tr>
   <td><strong>interface </strong>CommandHandler
<p>
{
<p>
<em>   <strong>public function </strong></em>handle(<strong>Command </strong>$command);
<p>
}
   </td>
   <td><strong>interface Command {}</strong>
   </td>
  </tr>
</table>


Each Handler will require a _handle_() method that should accept a _Command_. We don't really care how the Handler actually handles the command, we only care about telling it what to do.


#### The CommandBus Interface

Dispatches command objects to the subscribed command handlers.


<table>
  <tr>
   <td><strong>interface </strong>CommandBus
<p>
{
<p>
   <em>/**</em>
<p>
<em>    * Dispatches the command $command to the proper CommandHandler.</em>
<p>
<em>    *</em>
<p>
<em>    * <strong>@param </strong>mixed $command</em>
<p>
<em>    */</em>
<p>
<em>   <strong>public function </strong></em>dispatch($command);
<p>
   <em>/**</em>
<p>
<em>    * Subscribes the command handler to this CommandBus.</em>
<p>
<em>    */</em>
<p>
<em>   //    public function subscribe(CommandHandler $handler);</em>
<p>
}
   </td>
   <td rowspan="2" ><strong>final class </strong>StandardCommandBus <strong>implements </strong>CommandBus
<p>
{
<p>
   <em>/** <strong>@var </strong>CommandHandlerLocator */</em>
<p>
<em>   <strong>private </strong></em>$commandHandlerLocator;
<p>
   <strong>public function </strong>__construct(CommandHandlerLocator $commandHandlerLocator)
<p>
   {
<p>
       $this->commandHandlerLocator = $commandHandlerLocator;
<p>
   }
<p>
   <strong>public function </strong>dispatch(Command $command)
<p>
   {
<p>
       ($this
<p>
           ->commandHandlerLocator
<p>
           ->getHandler($command)
<p>
       )->handle($command);
<p>
   }
<p>
}
   </td>
  </tr>
  <tr>
   <td><strong>interface </strong>CommandHandlerLocator
<p>
{
<p>
   <strong>public function </strong>getHandler(Command $command): CommandHandler;
<p>
}
<p>
<strong>interface </strong>CommandHandlerMap
<p>
{
<p>
   <strong>public function </strong>getCommand(string $command): CommandHandler;
<p>
}
   </td>
  </tr>
</table>


Now we need a data structure where we can store the relationships between Commands and Command Handlers. For each Command we need a specific Command Handler. We're going to use a **simple immutable collection**

[http://tactician.thephpleague.com/](http://tactician.thephpleague.com/)

[http://simplebus.github.io/MessageBus/](http://simplebus.github.io/MessageBus/)


#### The Container

The Command Bus will be responsible for instantiating a new Handler instance when given a particular Command. This means the Command Bus needs a way of creating other objects.

Check Laravel  IoC Container implementation. [exploring Laravel's IoC Container](http://culttt.com/2014/03/24/exploring-laravels-ioc-container).

We can define a Container interface that we can rely on instead. This means we can effectively replace the IoC Container with a different implementation that also satisfies the interface contract:

Method make Return a new instance of an object.


```
interface Container
{
   public function make(string $class);
}
```



##### The Laravel Container implementation


<table>
  <tr>
   <td><strong>use </strong>Illuminate\Container\Container <strong>as </strong>IoC;
<p>
<strong>class </strong>ContainerUsesLaravel <strong>implements </strong>Container
<p>
{
<p>
   <em>/** <strong>@var </strong>IoC */</em>
<p>
<em>   <strong>private </strong></em>$container;
<p>
   <strong>public function </strong>__construct(IoC $container)
<p>
   {
<p>
       $this->container = $container;
<p>
   }
<p>
   <strong>public function </strong>make(string $class)
<p>
   {
<p>
       <strong>return </strong>$this->container->make($class);
<p>
   }
<p>
}
   </td>
   <td><strong>public function </strong>setUp()
<p>
{
<p>
   $container = <strong>new </strong>Container;
<p>
   $container->bind('Cat', <strong>function </strong>() {
<p>
       $cat = <strong>new </strong>stdClass;
<p>
       $cat->name = 'foo';
<p>
       <strong>return </strong>$cat;
<p>
   });
<p>
   $this->container = <strong>new </strong>ContainerUsesLaravel($container);
<p>
}
<p>
<em>/** <strong>@test </strong>*/</em>
<p>
<strong>public function </strong>should_make_class_from_container()
<p>
{
<p>
   $class = $this->container->make('Cat');
<p>
   $this->assertInstanceOf(\stdClass::<em>class</em>, $class);
<p>
   $this->assertEquals('foo', $class->name);
<p>
}
   </td>
  </tr>
</table>



#### The Inflector

The Command Bus should also be responsible for mapping Commands to Handlers. Instead of lumping this responsibility into the Command Bus, we can instead write a dedicated Inflector that can do the job for us.

As time goes by we might want to change the organisation of our code and so I think it makes sense to create an Inflector interface:

A simple string replace Inflector implementation could be as follows


#### The Command Bus Implementation.

The Command Bus should be injected with instances of Container and Inflector and it should have an execute() method that accepts a Command:



<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/DDD4.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/DDD4.png "image_tooltip")


[https://github.com/rilwanfit/command-bus](https://github.com/rilwanfit/command-bus)


## The Command Pattern - Behavioral

The command pattern is a behavioral design pattern in which an object is used to represent and encapsulate all the information needed to call a method at a later time. This information includes the method name, the object that owns the method and values for the method parameters

[https://code.tutsplus.com/tutorials/design-patterns-the-command-pattern--cms-22942](https://code.tutsplus.com/tutorials/design-patterns-the-command-pattern--cms-22942)

[http://php-and-symfony.matthiasnoback.nl/2015/01/some-questions-about-the-command-bus/](http://php-and-symfony.matthiasnoback.nl/2015/01/some-questions-about-the-command-bus/)


## Different between command and event

Command 


## Domain Service




# Building a Password Reminder Domain Service

[Last week](http://culttt.com/2014/10/20/creating-user-registration-domain-service/) we looked at writing the registration component of Cribbb as a Domain Service. The registration process is going to be a very important part of Cribbb and so I decided to model it as a Domain Service.

Registering new users into a new application is fairly straight forward and fits nicely into the Identity namespace we're currently working on.

Another important aspect of _Identity_ is the functionality to reset a password. When developing an application it is very easy to forget about implementing the ability for your users to reset their passwords. But if you launch without it, your new users will soon let you know about it with support requests!

In this week's tutorial we're going to be looking at implementing the functionality to reset passwords as a Domain Service.


## Is it a Domain Service?

In last week's tutorial we analysed whether the registration functionality should be a Domain Service or not.

I decided that I would write the registration functionality as a Domain Service for the following 3 reasons:



1.  The User object would end up with too much responsibility if it knew about the database and the ability to hash passwords.
1.  The process involves the co-ordination of multiple services
1.  The registration is an important part of the application's business logic

When it comes to providing password reminder functionality, I think you could argue that the three points above are equally as valid.

Firstly, the functionality does not fit on any existing Domain Object. The User object will require a resetPassword() method, but it should not have the responsibility to query the database or hash a new password. We need to tell the User object about the new password, not expect it to create it itself.

Secondly, resetting a user's password will require multiple Domain Objects and Services. We will need a Reminder Entity, and the ability to generate a ReminderCode Value Object and assert whether it is valid or not. We need a way of encapsulating this logic or it will leak out into the application.

And thirdly, whilst you could argue that resetting a password should be an Application Service, I tend to prefer to keep this kind of functionality insulated in the Domain, and then allow the Application layer to act as thin layer of co-ordination on top. I'm still figuring out this structure though, so if you don't feel like it is right for your application, feel free to implement it as an Application Service.


## How do Password Reminders work?

Before I jump into the code, first I'll briefly explain how the process for allowing a user to reset their password will work.


### 1. Requesting

When a user has forgotten their password, they will be directed to a form where they can submit their registered email address.

At this point we will check to ensure that the email is registered with the application.

If the email is valid we can create a new Reminder that will expire in 60 minutes.

We can then send the user an email with a link to reset their password.


### 2. Reseting

The user will receive an email with a URL back to Cribbb that contains a unique code.

When the user clicks on the link they will be taken to a page that will allow them to reset the password.

The unique code in the URL will be validated against the current valid Reminder codes to ensure it is valid.

If the code is valid, the user will be able to reset their password.

I'm sure you've probably reset your password hundreds of times using web applications, but if you've never built a password reminder service, I think it can useful to know what's going on under the hood.


## Speccing what we need to build

So now that we have the theory down, we can start to think about what code we need to write to actually make the functionality work.


### Domain Objects

The Domain is the most important part of Domain Driven Design and so naturally the bulk of the logic should be encapsulated in Domain Objects. If the bulk of the logic was captured in Services or Infrastructure we would know that something had gone wrong.



1.  When a user requests to reset their password, we need to create a Reminder. A Reminder is stored in the database and has a lifecycle, so it should be an Entity. The Reminder object should be responsible for determining if it is valid or not.
1.  Each Reminder will need a ReminderCode. The ReminderCode can be a Value Object and so we can model it separately.
1.  We will need a Repository to add, find and delete reminders
1.  We will also need Domain Events so we can send the email to the user on the request and a confirmation email when the password was successfully reset. Using Domain Events means we don't have to couple the sending of the email to the Reminder Service.
1.  And finally we need to add a resetPassword() method to the existing User class.


### Domain Services



1.  We will need a ReminderService that will co-ordinate the various Domain Objects and Services and provide an API to the Application layer of the application


### Infrastructure



1.  We will need a Doctrine Repository implementation for the ReminderRepository.


## The Reminder Code Value Object

Value Objects are always the easiest bit of developing functionality like this so we'll start by creating the ReminderCode class:

[?](http://culttt.com/2014/10/27/building-password-reminder-domain-service/#)


<table>
  <tr>
   <td>1
<p>
2
<p>
3
<p>
4
<p>
5
<p>
6
<p>
7
<p>
8
<p>
9
<p>
10
<p>
11
<p>
12
<p>
13
<p>
14
<p>
15
<p>
16
<p>
17
<p>
18
<p>
19
<p>
20
<p>
21
<p>
22
<p>
23
<p>
24
<p>
25
<p>
26
<p>
27
<p>
28
<p>
29
<p>
30
<p>
31
<p>
32
<p>
33
<p>
34
<p>
35
<p>
36
<p>
37
<p>
38
<p>
39
<p>
40
<p>
41
<p>
42
<p>
43
<p>
44
<p>
45
<p>
46
<p>
47
<p>
48
<p>
49
<p>
50
<p>
51
<p>
52
<p>
53
<p>
54
<p>
55
<p>
56
<p>
57
<p>
58
<p>
59
<p>
60
<p>
61
<p>
62
<p>
63
<p>
64
<p>
65
<p>
66
<p>
67
<p>
68
<p>
69
<p>
70
<p>
71
<p>
72
<p>
73
<p>
74
<p>
75
<p>
76
<p>
77
<p>
78
<p>
79
<p>
80
<p>
81
<p>
82
   </td>
   <td><?php namespace Cribbb\Domain\Model\Identity;
<p>
 
<p>
use RuntimeException;
<p>
use Cribbb\Domain\ValueObject;
<p>
 
<p>
class ReminderCode implements ValueObject
<p>
{
<p>
    /**
<p>
     * @var string
<p>
     */
<p>
    private $value;
<p>
 
<p>
    /**
<p>
     * Create a new ReminderCode
<p>
     *
<p>
     * @param string $value
<p>
     * @return void
<p>
     */
<p>
    private function __construct($value)
<p>
    {
<p>
        $this->value = $value;
<p>
    }
<p>
 
<p>
    /**
<p>
     * Generate a new ReminderCode
<p>
     *
<p>
     * @return ReminderCode
<p>
     */
<p>
    public static function generate($length = 40)
<p>
    {
<p>
        $bytes = openssl_random_pseudo_bytes($length * 2);
<p>
 
<p>
        if (! $bytes)
<p>
        {
<p>
            throw new RuntimeException('Failed to generate token');
<p>
        }
<p>
 
<p>
        return new static(substr(str_replace(['+', '=', '/'], '', base64_encode($bytes)), 0, $length));
<p>
    }
<p>
 
<p>
    /**
<p>
     * Create a new instance from a native form
<p>
     *
<p>
     * @param mixed $native
<p>
     * @return ValueObject
<p>
     */
<p>
    public static function fromNative($native)
<p>
    {
<p>
        return new ReminderCode($native);
<p>
    }
<p>
 
<p>
    /**
<p>
     * Determine equality with another Value Object
<p>
     *
<p>
     * @param ValueObject $object
<p>
     * @return bool
<p>
     */
<p>
    public function equals(ValueObject $object)
<p>
    {
<p>
        return $this == $object;
<p>
    }
<p>
 
<p>
    /**
<p>
     * Return the object as a string
<p>
     *
<p>
     * @return string
<p>
     */
<p>
    public function toString()
<p>
    {
<p>
        return $this->value;
<p>
    }
<p>
 
<p>
    /**
<p>
     * Return the object as a string
<p>
     *
<p>
     * @return string
<p>
     */
<p>
    public function __toString()
<p>
    {
<p>
        return $this->value;
<p>
    }
<p>
}
   </td>
  </tr>
</table>


The ReminderCode requires a single generate() method that can be called statically. We can also make the __construct() method private because the code should be generated internally.

This class should also implement the ValueObject interface and we can provide a fromNative() method so we can rehydrate data from the database.

The tests for this class are basically the same as the other Value Object tests:

[?](http://culttt.com/2014/10/27/building-password-reminder-domain-service/#)


<table>
  <tr>
   <td>1
<p>
2
<p>
3
<p>
4
<p>
5
<p>
6
<p>
7
<p>
8
<p>
9
<p>
10
<p>
11
<p>
12
<p>
13
<p>
14
<p>
15
<p>
16
<p>
17
<p>
18
<p>
19
<p>
20
<p>
21
<p>
22
<p>
23
<p>
24
<p>
25
<p>
26
<p>
27
<p>
28
<p>
29
<p>
30
<p>
31
<p>
32
<p>
33
<p>
34
<p>
35
<p>
36
<p>
37
<p>
38
<p>
39
<p>
40
<p>
41
<p>
42
<p>
43
<p>
44
<p>
45
<p>
46
<p>
47
<p>
48
<p>
49
<p>
50
   </td>
   <td><?php namespace Cribbb\Tests\Domain\Model\Identity;
<p>
 
<p>
use Cribbb\Domain\Model\Identity\ReminderCode;
<p>
 
<p>
class ReminderCodeTest extends \PHPUnit_Framework_TestCase
<p>
{
<p>
    /** @test */
<p>
    public function should_generate_new_code()
<p>
    {
<p>
        $code = ReminderCode::generate();
<p>
 
<p>
        $this->assertInstanceOf('Cribbb\Domain\Model\Identity\ReminderCode', $code);
<p>
    }
<p>
 
<p>
    /** @test */
<p>
    public function should_throw_exception_if_code_generation_fails()
<p>
    {
<p>
        $this->setExpectedException('RuntimeException');
<p>
 
<p>
        $code = ReminderCode::generate(null);
<p>
    }
<p>
 
<p>
    /** @test */
<p>
    public function should_create_a_code_from_a_string()
<p>
    {
<p>
        $code = ReminderCode::fromNative('D1zcA5ncaEHzmjvCGjJIt3Kd8sGxTTtE7DkathqB');
<p>
 
<p>
        $this->assertInstanceOf('Cribbb\Domain\Model\Identity\ReminderCode', $code);
<p>
    }
<p>
 
<p>
    /** @test */
<p>
    public function should_test_equality()
<p>
    {
<p>
        $one   = ReminderCode::fromNative('D1zcA5ncaEHzmjvCGjJIt3Kd8sGxTTtE7DkathqB');
<p>
        $two   = ReminderCode::fromNative('D1zcA5ncaEHzmjvCGjJIt3Kd8sGxTTtE7DkathqB');
<p>
        $three = ReminderCode::fromNative('sdffpweofpojwepfnpowefopwfpowejfpopopfep');
<p>
 
<p>
        $this->assertTrue($one->equals($two));
<p>
        $this->assertFalse($one->equals($three));
<p>
    }
<p>
 
<p>
    /** @test */
<p>
    public function should_return_as_string()
<p>
    {
<p>
        $code = ReminderCode::fromNative('D1zcA5ncaEHzmjvCGjJIt3Kd8sGxTTtE7DkathqB');
<p>
 
<p>
        $this->assertEquals('D1zcA5ncaEHzmjvCGjJIt3Kd8sGxTTtE7DkathqB', $code->toString());
<p>
        $this->assertEquals('D1zcA5ncaEHzmjvCGjJIt3Kd8sGxTTtE7DkathqB', (string) $code);
<p>
    }
<p>
}
   </td>
  </tr>
</table>


These tests are asserting that we can create a new ReminderCode instance and the equals() method can correctly identify equality.


### The Reminder Id Value Object

The Reminder class will be an Entity and so it will require a unique Id. In this project I'm using UUIDs as unique identifiers instead of auto-incrementing database ids: **use **Mhr\Domain\Model\Identity\ReminderId;


### The Reminder Entity

The Reminder Entity is fairly similar to the User Entity that we created a[ couple of weeks ago](http://culttt.com/2014/09/01/user-entity-ubiquitous-language/).

However I'll walk through it step-by-step because it's quite a big class.

Firstly, the Entity should implement the AggregateRoot interface and it should use the RecordsEvents trait (I renamed HasEvents to RecordsEvents):  **use **Mhr\Domain\Model\Identity\Reminder;

**Note:** I'm using Doctrine annotations because this is an Entity. If you are unfamiliar with Doctrine annotations, take a look at[ this tutorial](http://culttt.com/2014/07/07/doctrine-2-different-eloquent).



*   We don't need to associate a Reminder code to a user because that would be an unnecessary association. Instead we can simply record the email address.
*   all setters as **private** unless they are explicitly required by the Domain. 
*   _isValid_() method ensures that the logic behind whether a reminder is valid or not is encapsulated inside the Reminder Entity.


## The Reminder Repository Interface

The next thing we need to do is to define the ReminderRepository interface. As we saw a[ couple of weeks ago](http://culttt.com/2014/10/13/creating-testing-doctrine-repositories), we should define Repository interfaces as part of the Domain of our application, and leave the implementation up to the Infrastructure layer of the application.

The first method we will need will be to return a new instance of ReminderId:



*   As I mentioned a[ couple of weeks ago](http://culttt.com/2014/09/08/benefits-using-repositories), the repository should be conceptionally responsible for managing the unique id's of the collection of Entities.
*   As with the UserRepository, this method should accept a whole instance of Reminder. The add() method's responsibility is simply to add the entity to the collection.
*   findReminderByEmailAndCode  This will be used when the user clicks on their email link. We need to ensure that the reminder code exists and that it is associated with the correct email address.


## The Reminder Repository Implementation

Next we need to create the Doctrine implementation of the Reminder Repository. This will be a new file called ReminderDoctrineORMRepository under the Repositories namespace in the Infrastructure directory:



*   The EntityManager is the real power behind Doctrine, and so we need an instance of it to talk to the database.
*   nextIdentity() his method we need to return a new instance of ReminderId and so we can use the static generate() method to do just that.
*   findReminderByEmailAndCode   In this method I'm using Doctrine's DQL. If you are unfamiliar with DQL you should read my previous tutorial[ Using Doctrine Query Language](http://culttt.com/2014/07/28/using-doctrine-query-language).
*   add   If you read[ last week's posts](http://culttt.com/2014/10/27/building-password-reminder-domain-service/%E2%80%9Chttp://culttt.com/2014/10/20/creating-user-registration-domain-service%E2%80%9C), the code above should look familiar. Here we are passing a whole Reminder instance into the Repository for it to be persisted.
*   Delete methods - This will be useful for cleaning up after a reminder has successfully been used to reset a password.
*   deleteExistingRemindersForEmail   If the user doesn't receive their password reset email straight away, they might request another email. This method will clean up an previously requested reminders from the database so we don't have multiple reminder codes hanging around.
*   deleteExpiredReminders  Once a reminder has expired there is no point in keeping it in the database. This method will simply delete any reminders that have expired. This could be run as an action in the Reminder service or perhaps an automated cron job.


## Reminder Fixtures

If you read[ last week's post](http://culttt.com/2014/10/20/creating-user-registration-domain-service) you will remember that we introduced the concept of Doctrine fixtures. Fixtures allow you to seed the database with data so you can run your database tests with existing data. This saves you from having to write code in each test to set up the database.

Create a new class under the Fixtures namespace

**use **Mhr\Tests\Infrastructure\Repositories\Fixtures\ReminderFixtures;

In this fixture class I'm creating two existing reminders, one that is valid and one that has expired. This will allow us to test each of the Repository methods by querying for actual data.

When it comes to writing fixtures, I prefer to write the absolute minimum. Fixtures can end up getting out of control if you aren't strict with them, and you don't want to end up with false positives in your tests because you don't fully understand what is going on in your database set up.


## The Repository Tests

Next we can write the Repository tests. Here is the initial test file set up:

As with the Repository tests[ from last week](http://culttt.com/2014/10/20/creating-user-registration-domain-service), here I'm using a setUp() method to get everything in place for the tests. This includes migrating the database and loading the fixtures.

The first test should simply assert that an instance of ReminderId is returned from the nextIdentity() method:


### The Reminder Service

Now that we have the Entities, Value Objects and Repositories in places, we can look at using these individual components to build the ReminderService. **use **Mhr\Domain\Services\Identity\ReminderService;



*   define the __construct() method and inject the class dependencies : __construct(ReminderRepository $reminders, UserRepository $users, HashingService $hasher)
*   _UserRepository_ - so we can search for existing users 
*   _HashingService_ so we can hash the user's new password.


#### Request method



*   check to ensure that the email address is an actual registered email address in our application
*   delete any existing reminders for this user
*   Finally we can create a new Reminder instance, add it to the Repository and then return the object from the method


#### Check method

This method will be called when the user clicks on the link in the reminder email



*   ensure that the reminder code exists, is valid and belongs to the correct email address


#### reset method

accept the request from the user to update their password



*   ensure nothing has been tampered with email and code.
*   ensure that the email address is an actual registered email address in our application
*   pass the password into the HashingService to receive an instance of HashedPassword in return
*   pass the HashedPassword to the resetPassword() method on the User object.
    *   Additionally, we can also record a PasswordWasReset Domain Event. This will be used to send a confirmation email to the user that they successfully changed their password.
*   pass the user to the UserRepository to persist the user to the database
*   Now that the reminder has been successfully used, we can delete it from the database using the deleteReminderByCode() method.
*   finally, we can return the $user from the method. The $user object is loaded with the PasswordWasReset Domain Event ready to be fired.

Testing - there is no need to hit the database during these tests. However, we will need to create some in-memory fixtures so we don't have to repeat the same set up code for each test.


## Domain Model

Meaning of it? Why it is important? How to use it?


### The Domain is the problem

Domain Driven Design is predicated around the idea of solving the problems organisations face through code. This is achieved by focusing the investment of resources into the heart of the business logic of the application.

The domain therefore is, the world of the business. Whenever you hear the phrase "Domain Driven Design", you should think of it as "Business Problem Driven Design".

The domain is the world of the business you are working with and the problems they want to solve. This will typically involve rules, processes and existing systems that need to be integrated as part of your solution.

The domain is the ideas, knowledge and data of the problem you are trying to solve. Most businesses will have terms that have specific meaning within the context of their organisation. They will also likely have metrics, goals and objectives that are unique to their business.

All of the knowledge around the company and how it operates will form the domain of your Domain Driven Designed project.


### The Model is your solution

The Model of an Domain Driven Designed project is your solution to the problem.

The Model usually represents an aspect of reality or something of interest. The Model is also often a simplification of the bigger picture and so the important aspects of the solution are concentrated on whilst everything else is ignored.

This means your Model should be focused knowledge around a specific problem that is simplified and structured to provide a solution.


### The Domain Model

So if the Domain is the world of the business, and the Model is your solution, what is the Domain Model?

The Domain Model is your **organised and structured knowledge / focused knowledge** of the problem. The Domain Model should represent the vocabulary and key concepts of the problem domain and it should identify the relationships among all of the entities within the scope of the domain.

The Domain Model itself could be a diagram, code examples or even written documentation of the problem. The important thing is, the Domain Model should be accessible and understandable by everyone who is involved with the project.

The Domain Model should also define the vocabulary around the project and should act as a communication tool for everyone involved. The Ubiquitous Language is an extremely important concept in Domain Driven Design and so it should be directly derived from the Domain Model.

One of the downfalls of many software development projects is the misunderstanding of terms, objectives and proposed solutions that are scoped at the beginning of development.

The Domain Model should act as a clear depiction of the problem that is being solved and the proposed solution. It is extremely important that all stakeholders of the project contribute to the Domain Model so that everyone understands the key concepts and definitions of the vocabulary of the project and how the problem is being tackled and solved.

The Domain Model includes the Entities, Value Objects and relationships between the things of importance that are specific to that particular problem.


### Why is the Domain Model important?

So the Domain is the world of the business, the Model is your solution and the Domain Model is the structured knowledge of the problem.

Why is this important to Domain Driven Design? Well, I think there are three reasons why a Domain Model is important.


#### The Domain Model and the heart of the design shape each other

The code that you write should be intimately linked to the model of the problem you are solving. Anyone on the team should be able to look at your code and understand how it applies to the problem you are solving.

When developers deviate from the model, they write code that is built around their mental model of the problem. The code that you write should be directly derived from the agreed Domain Model to ensure that your solution meets the requirements of the business.


#### The Domain Model is the backbone of the language used by all team members

Every single person on the project team should use the Ubiquitous Language. This means that technical and non-technical people have a common language to communicate so there is no loss of understanding between parties.

The Ubiquitous Language should be directly derived from the Domain Model and so without a Domain Model you don't have a Ubiquitous Language.

Without a Ubiquitous Language you will start to feel the friction of communication and the loss of understanding between technical and non-technical members of the team. If the code that is written starts to deviate away from the requirements of the business experts, the end solution won't satisfy the goals of the project.


#### The Domain Model is distilled knowledge

The Domain Model should be the outcome of an iterative discovering process where everyone on the team meets to discuss the problem you are facing and how it should be solved.

This early collaboration between domain experts and the development team is critical to the success of the project.

The Domain Model should capture how you think about the project and all of the distilled knowledge that has been derived from those collaboration sessions.

Whenever a decision needs to be made during the course of the project everyone should refer to the Domain Model to look for the answer or to try to iteratively evolve the design to discover a hidden truth that has not previously been exposed.


### How do you arrive at a Domain Model?

Arriving at a Domain Model is an iterative approach that attempts to discover the real problem that is faced and the correct solution that is required.

It's important to focus a Domain Model on one important problem. Trying to capture the entire scope of a business inside of a single Domain Model will be far too overcomplicated and most likely contradictive as concepts and ideas move around the organisation.

The problem should be mapped out through talking to business experts to discover the problems they face. This should form the conceptual problem that you are looking to tackle.

Business experts won't talk in the terms of technical solutions, and so it is your job to correctly interpret their problems into technical solutions.

During this process the important aspects of the problem should be picked out from the language and given concrete definitions to form the Ubiquitous Language.

It doesn't really matter whether the Domain Model is created in code, as a diagram or in prose. However there should be a tight feedback loop where everyone on the project discusses the proposed Domain Model to get closer to the solution. This is an iterative approach that will likely encompass code, diagrams and prose to really understand the problem and to discover the correct solution.


### What is an example of a Domain Model?

It's difficult to show an example of a Domain Model as it should really be taken from a real world business problem.

However, for the benefit of this article, let's pretend we have the following business problem we need to solve:



1.  A business wants to create software to manage its projects
1.  Projects are derived from a product roadmap
1.  The project is picked due to certain criteria
1.  The project is assigned to a project manager
1.  The project manager organises the team
1.  The sprint is planned

The 6 steps that I've outlined above is the manually process that managers at a fictitious organisation walk through to assign tasks to employees. This is the Domain because it is solely focused on the world of the business.

The Model is focused on this one aspect of project management within the scope of the bigger organisation. In large organisations there are probably numerous ways that work is chosen and allocated to teams of employees. However in order to stay on track, we only care about this one, structured problem.

The Domain Model should take the Domain and the Model to form a conceptual solution to the problem. For example, we can see that we will probably need entities for _Roadmap_, _Project Managers_, _Employees_, _Team_ and _Sprint_.

We will need a way of _selecting projects_ from some kind of persistence storage.

We will need a way of capturing the business logic of _Criteria_ to make decisions of priority based upon what is important to the business.

And finally we will need a way of _planning_ a sprint and allocating times and resources to ensure that the work is completed.

I'm sure if this was a real world problem we would need to explore the concept of assigning projects in much greater detail, but hopefully you see what I'm getting at.


## Failing Domain Rules and Validating User Input

An interesting predicament that all applications will face is, how do you validate incoming data, and how do you communicate errors back to the user interface?

we can encapsulate Domain related rules and logic inside of the Entities, Value Objects and Specifications of our application.

These Domain Objects protect against inputs and operations that do not meet the business rules of the application. By encapsulating the business rules inside of the object, the consumer of the object does not need to be concerned with how the rule is enforced.

But in the grand scheme of an application, there is going to be lots of incoming data. Should you rely on the logic of your Domain code to act as the single source of truth?


### The problem of incoming data.

Simply never trust user input.


### The Domain layer as the single source of truth

We encapsulate the business rules of the application using Domain Objects. For example, we created Value Objects for Email and Username that would ensure the input matched our criteria for being valid. And also There is Specification objects that could select objects from the database to check for uniqueness. This means we can ensure that a user's chosen username and email have not already been registered.

Domain Driven Design is all about modelling the business rules of the application in code. Whilst this particular application is fairly simple, we now have a rich Domain layer that will enforce the rules of the application.

However, should the Domain layer act as a single source of truth? When a request comes in from the outside world, should we rely on the Email Value Object to determine if the user's email address is valid?

Whilst everyone knows you shouldn't repeat yourself, I think there is a time and place to "violate" this rule. Instead of having a single source of truth when it comes to validation, I think you need multiple layers of validation.


### Multiple layers of validation

When building a web application, you usually have a set of basic rules for the requirements of incoming data. For example, perhaps you require the user to enter their first name, last name and a valid email address.

Typically when building out the PHP side of the application you will enforce these rules in one of many possible places. In Cribbb I'm enforcing the rules using Value Objects, but you might instead be running your validation in the model.

If you follow the "rules" of good programming, you will know that you shouldn't repeat the same logic. For example, validating data in the controller tends to lead to duplicate code because you end up needing that same validation logic else where in your application. Most developers are pretty emphatic about not repeating this type of logic.

However if you have any type of Javascript validation, you are going to end up repeating the "domain logic" validation. You will probably have both Javascript and a PHP code for validating that the fields were completed correctly and the email address is valid.

Typically copying and pasting code is usually a bad thing, but I think when code is doing two very different jobs, it's fine to repeat the same logic.

Therefore, I believe there should be multiple layers of validation in your application to deal with the different problems that can arise.

Duplicating validation rules is usually fine because as long as your Domain logic is correct your application won't start allowing invalid data. Discrepancies between the additional higher layers of validation can usually be picked up through Quality Assurance testing.

Your Domain rules should therefore be the final guard against invalid data, but certainly not the only one.


### The different layers of validation

I think the vast majority of web application will have fairly similar validation requirements and so the following validation layers are probably applicable to most projects.


#### Client side validation

The first layer of validation is simple format checking written in Javascript. By this I mean simply checking that what the user input entered matches the correct type and format of data you require.

For example, first names and last names should probably not contain numbers or symbols, and email addresses follow a fairly predictable pattern.

Instead of reinventing the wheel, a good choice for this type of validation is[ jQuery Validation](http://jqueryvalidation.org).

If the user has entered data that is obviously incorrect we don't need to make a roundtrip to the server in order to let them know. These types of errors should be communicated immediately and so using Javascript is the perfect choice.


#### Application Service requests

When you need to check input data against the business rules of your application there is no avoiding making a server request. However allowing the user to submit the form before telling them that their chosen username has already been taken is really annoying.

Instead you should be able to send an Ajax HTTP request to the server to query against the database.

This can be implemented by writing an Application Service that exposes an interface to accept these types of requests and return the appropriate response. This means the user can be notified almost instantly if they have entered data that violates the application's business rules.

These Application Services can be really simple because all they have to do is receive an HTTP request and then return a response in JSON.


#### Framework validation library

When the user submits the form, the request will typically be routed to a Controller who's job it is to route the request through the application to the correct destination.

A lot of application framework's ship with validation libraries. Laravel for example has an excellent[ Validation Library](http://laravel.com/docs/4.2/validation) that allows you to validate data in a number of useful ways.

This layer of validation is important because you can't assume that Javascript will be enabled in the user's browser. Javascript is something that you can just turn off, and so relying only on Javascript validation is not a great solution.

The depth of validation you implement at this level is usually specific to the type of request that is being made. In a lot of instances you can simple check for the correct data types to make sure the required fields have been completed and the data looks like it's in the correct format. If these validation checks fail we can very easily abort the HTTP request and redirect back to the form.

Controllers also typically handle requests that don't match the shape of the Domain layer. For example, if you were accepting a new order request in an ecommerce application, you are probably mixing together Order, Customer and Product validation rules. This means your Controller validation can act as a light layer of protection against invalid data across multiple related components of your application.

Having a layer of validation in the controller serves the purpose of handling any obvious errors before the request makes it into your application.


#### Domain validation

Finally we have the Domain validation that is encapsulated in Entities, Value Objects, Specification as well as any other Domain Object that enforce the business rules of the application.

Domain Objects can signal a problem in one of two ways. Either returning null or false from the method, or by throwing an Exception that should be caught at a higher level.

As I covered in[ When should you use an Exception?](http://culttt.com/2014/04/09/use-exception), I believe Exceptions should only be thrown in exceptional circumstances. If null or false is an acceptable response from a method, avoiding using Exceptions in these instances is usually the right choice.

So for example, if we had a UserRegistrationService, we might return null if the user registration process failed. We could then expose an errors() method that would pass the errors back up to the User Interface.

On the other hand, a lot of the time it does make sense to throw Exceptions if something goes wrong. You could argue that this makes more sense in the Domain layer because this is our last form of defence against the outside world.

The Domain layer could throw a Domain Exception if one of the business rules had not been satisfied. This Exception could then be caught in the Controller layer in order to return the feedback to the user.

If you have a pretty robust layer of validation before the Domain layer you might instead want these Exceptions to bubble all the way back to the surface to halt the application's processing. The Domain layer should protect against malicious attacks and so if you believe that the request must be malicious because it made it's way through your additional layers of validation, it might be better to just bail.

[http://highscalability.com/blog/2015/10/12/making-the-case-for-building-scalable-stateful-services-in-t.html](http://highscalability.com/blog/2015/10/12/making-the-case-for-building-scalable-stateful-services-in-t.html)

Bounded Contexts and Context Maps


### Domain Models

The theoretical idea of a Domain Model makes a lot of sense in the world of programming. A Domain Model is the focused knowledge of a problem and so having a clear depiction of the problem and the solution makes your life as a programmer a lot easier.

However, in the real world of building applications for businesses, there is never just a single Domain Model. If you were building an application that touches many parts of an existing business, you will likely encounter multiple coexisting Domain Models that will often contradict one another.

For example, in a typical ecommerce web application, the term Product will likely mean something different in the stock system than it does in the catalogue system. This might mean that the two different systems have two different ideas of responsibility for that object, or that the object should have two completely different sets of capabilities.

When objects have multiple different responsibilities for different parts of an application they can become monolithic and difficult to work with. 

What's more, when there are multiple developers working on different problems using the same objects, there will inevitably be conflicting requirements and the introduction of unforeseen bugs and inconsistencies.

A Domain Model should be the focused knowledge around a particular problem. This constrains the scope of the Domain Model to a very specific context. When you inevitably encounter these conflicting ideas, it's time to take a step back from the Domain Model.


### What is a Bounded Context?

A Bounded Context is the **boundary that surrounds a particular model**. This keeps the knowledge inside the boundary consistent whilst ignoring the noise from the outside world.

This is beneficial/important for a number of reasons.



1.   you can model the aspects of the problem without having to be concerned with other parts of the business. 

Example: a Product object within the stock system only needs to be concerned with the methods and properties of that single system, rather than any other business concern that happens to fit on an object called Product.



1.  Protects the internal consistency of the model. 

The terminology within a Bounded Context can have single, clear definition that accurately describe the problem at hand. Different departments across a company will usually have slightly different ideas and definitions of similar terms, but this can often derail a project through a lack of clarity and understanding when terms are mixed.



1.  Each of the objects within a Bounded Context have a clear scope and set of responsibilities.

In large applications you will often find that certain objects attract responsibility from all over the application. These monolithic objects quickly become difficult to use, maintain and test. A Bounded Context will constrain the scope of an object to the focused responsibility of that single aspect of the project. This not only keeps responsibility from leaking in, but it should also stop responsibility from leaking out.

The language, names of objects and ideas within the Bounded Context should form a unified model of the problem at hand. The Bounded Context shields the internal model from the complexity of the outside world. Each Bounded Context should have an internal model that is clearly understood by all members of the team. This is important because in most organisations, certain terms will have different meanings across departments or areas of the business.

By using different Bounded Contexts, we keep different aspects of the application decoupled.

When working on a large application it is important to split large models into Bounded Contexts and then to be explicit with how each Bounded Context is related to other Bounded Context of the application.

Ex: 


<table>
  <tr>
   <td><ol>

<li><strong>Identity Bounded Context.</strong> <ol>

 <li>New user registration
 <li>Password reset
 <li>User follow up </li> </ol>
</li> </ol>

   </td>
   <td><ol>

<li><strong>Groups</strong> <ol>

 <li>Regulates contents and discussions
 <li>Allow users to join and become member or admin 
 <li>Contains members and Admin</li> </ol>
</li> </ol>

   </td>
  </tr>
</table>



### What is a Context Map?

When you start to think about the various subsystems of an application as individual Bounded Contexts, you can lose sight of the global view of the business as a whole.

It is inevitable that the various Bounded Contexts of an application will need to communicate or share data between each other.

A Context Map is the **global view of the application as a whole**. Each Bounded Context fits within the Context Map to show how they should communicate amongst each other and how data should be shared.

It's better to have a true internal model for each Bounded Context with a layer of translation, rather than having monolithic objects that try to fill the role of different, often conflicting jobs.


### Strategies for Integrating Bounded Contexts

The vast majority of all Domain Driven Design applications will have multiple Bounded Contexts. This could mean integration with third-party services, but it is also often the case that you need to integrate with existing legacy applications or simply other models of the same application.

There are many technical ways to integrate Bounded Contexts, third-party services and legacy applications.

However, choosing the correct integration pattern is extremely important because it will have a big impact on the design of your application and the future of your project as a whole.


#### The Reality of software projects

In an ideal world, every software project would start with a blank slate, a clean git repository and no legacy headaches. However, in the real world, these types of projects are very rare.

The vast majority of all non-trivial application development projects will require multiple Bounded Contexts.

When working with an existing organisation, it is usually the case that you will be required to integrate with legacy applications or third-party services.

It is therefore your job to integrate these legacy applications and third-party systems with the new project you are working on.

The problem with these types of integrations is, there are an unlimited number of situations you can find yourself in. For example, you could find yourself at the mercy of a third-party service, or perhaps you are responsible for providing an interface to an existing legacy system.

There are a number of different integration strategies that I will discuss in this article. Each strategy has it positives and negatives.


#### Example

Imagine that we've been brought into an existing offline retailer as Consultants to create a new online Ecommerce website for the company.

The company has been trading on the high street for a number of years now and so it already has existing stock management, distribution and financial systems.

Our assignment is to create an Ecommerce website that can interface with the company's existing systems to provide a seamless new channel for sales.

Our new development project is clearly a new Bounded Context within the scope of the Context Map of the Organisation as a whole. We aren't going to be extending any of the current systems, but we must consume and return data from each of the existing systems in order to make the new channel seamlessly integrate into the current architecture.

We will need to be able to request data from the stock management system in order to know what should be made available to purchase online.

We will also need to send data to the distribution and financial systems so orders are processed correctly and the company's accountancy liabilities are dealt with.

If this was a real life situation, we would probably have to interface with a whole load of other existing systems and third-party services. However, hopefully you can see the requirement for being able to integrate with existing systems through a layer of translation.

Whilst in a theoretical idea world, it would be wonderful if Ecommerce sales, stock management, distribution and finances were all part of a single unified model. However, the complexity of such a system would quickly get out of control.

Instead we need to find ways of integrating through a layer of translation. The following are 7 ways of doing just that.



1.  Shared Kernel

The first integration strategy is to use a Shared Kernel, where a part of the Domain Model is shared between different teams working on the same application.

The Shared Kernel integration strategy is beneficial because it reduces the amount of duplicated code and the overhead of translation layers.

However a Shared Kernel will only work if all of the development teams work to share and communicate the requirements of the shared code. This means that the design of the Shared Kernel can't evolve as quickly as other aspects of the application, and any changes must be agreed upon by members of each development team.

Each development team will also have to take equal responsibility for maintaining unit tests that ensure the functionality of the Shared Kernel remains consistent.

When there is a shared requirement of a certain aspect of the application, and relatively high levels of communication and low levels of political unrest, the Shared Kernel integration strategy can be easier to implement than many of the other integration strategies we will look at further.

Example: in our Ecommerce analogy might be that the **Customer Model **is shared between the Transaction team and the Online Marketing team.

The Transaction team requires the Customer Model to associate transactions and request payments, whereas the online marketing team requires the Customer Model to send marketing information to stimulate new purchases and keep the customer informed of new products and offers.

Instead of both teams writing their own Customer Model and translating back and forth, they could have a shared Customer Model that they both rely on to satisfy their requirements. This means that the teams need to agree on the specification of the Customer Model, and any future changes must be signed off by both teams.


#####    2. Customer / Supplier

A common relationship between two software applications is where a downstream application requires data from an upstream application, but the upstream application is not dependent on the downstream application.

This relationship can play out in a number of different ways.

Firstly the downstream team can be at the mercy of the upstream team if the upstream team make changes without thinking about the requirements of the downstream team.

Alternatively the upstream team can feel restricted on the design and implementation of their application if the downstream team has control over how their application evolves.

The downstream team is dependent on the upstream team, but the upstream team is not responsible for the deliverables of the downstream team. In order to make this situation work, the two teams need to have a formal relationship where the requirements of the downstream team are considered, as would occur in any Customer / Supplier relationship.

This means future development priorities, tasks and requests need to be agreed upon by members of both teams.

Both teams should also agree upon a series of Acceptance tests that will ensure the interface of the boundary remains consistent. This means the upstream team can evolve their application as long as the interface remains consistent, and the downstream team can be assured that they won't wake up one day to find their application broken because of the upstream team's changes.

The Customer / Supplier relationship works best when both teams' goals are aligned or they both report to the same layer of management. When the goals of the two teams are not aligned, the relationship will often break down.

An example of the Customer / Supplier relationship from our Ecommerce analogy could be where a separate Analysis application must take data from the Ecommerce application in order to generate customer recommendations and predict new trends.

The Analysis application is in a different Bounded Context because it is likely to be written in a different programming language and use very different tools and persistent storage than that of the Ecommerce application.

The Analysis application is relying on the Ecommerce application to send data of the transactions, customer profiles and tracking events in order to run the analysis.

The two teams should agree upon on the type, format and method of the data and how it should be transferred downstream. If both teams are aligned in increasing the success and profitability of the company as a whole, this relationship is more likely to work.


#####   3. Conformist

When the relationship between the Customer and the Supplier is not mutually aligned, the downstream team can end up in a situation where it is at the mercy of the upstream team.

This can occur in the same company where each team reports to a very different layer of management with different goals, or where the Supplier has many small Customers and so each individual Customer is not particularly important to the Supplier.

This means that the upstream team has no motivation to provide any kind of priority or even consistency to the downstream team. The downstream team just has to accept the fact that they cannot rely on the upstream team and a consistent interface at the boundary of the relationship.

If the value of the upstream application is critically important to the downstream application, the only way to continue is to adhere to the whims of the upstream team.

This will mean that the upstream team will be completely in control of the integration of the two applications and so the downstream will have to just make it work.

This will cause the downstream team to have a deep dependency on the upstream team and so any future development will be constrained by the situation.

An example of the Conformist relationship in our Ecommerce analogy could be where we rely on a third-party delivery service to send packages to our customers. In order to update our customers on the location of their progress we need to request updates from the third-party delivery service.

However, we are only one of thousands of customers of the delivery service, and so they have no motivation to provide the data we require. The delivery service is also free to change their API at any point, potentially breaking our integration.

In this situation we are completely at the mercy of the delivery service and so we have to take responsibility for accepting their data and the interface they provide for requesting it. If the interface changes, it is our responsible to conform to those changes and evolve our integration to meet those requirements.


#####   4. Anti-Corruption Layer

When working with existing legacy applications or third-party services, the integration requirements can often be complex. Whilst it might initially seem easier to avoid the integration all together, if the other system is critically important, there is probably more value in the integration.

Integrating other systems is difficult because the other model can leak through the integration and begin to affect the new system's own model. 

By adapting to the existing systems too much, we can end up with a new system that has an inconsistent or unsuitable model to solve it's own problem.

In order to prevent the model from an external system leaking internally to a new system, we need a way to translate the data between the two models. This will often mean ensuring the context of how the data is interpreted is consistent in the new model, but also preventing unnecessary data from leaking through the integration.

To achieve this we need to create an isolating layer that can communicate with the existing interfaces of the legacy and third-party systems, and then translate that back forth between our internal model as requests come and go.

An example of using an Anti-Corruption Layer in our Ecommerce analogy could be linking an existing customer loyalty scheme from the offline retail system with a customer loyalty scheme for our new online retail system.

If an existing offline customer has an existing loyalty balance with the retailer and she starts to shop online, those two systems should know about each other so the customer does not lose out on benefits or discounts.

However the two systems are likely going to have different models and different supporting data that is not relevant between the two systems.

In order to keep the two systems consistent we can set up Web Hooks ([What are Webhooks?](http://culttt.com/2014/01/22/webhooks)) that will send a request whenever the customer loyalty balance is updated on one side of the relationship.

When a request is sent or received, it will travel through the Anti-Corruption Layer to be translated into the appropriate form for the receiving application. This means data can flow between the two systems without having to change the existing offline system and without having to conform the new system to the model of the existing system.


##### 5. Open Host

When your application will need to integrate with another system, you will typically provide a layer of translation to make this integration easier.

However when your application needs to integrate with many other existing systems, having all of these layers of translation can start to become unwieldily.

Instead of providing one off translation layers for each integration, instead provide a set of Services that can be consumed by any other Bounded Context.

By providing your application as a series of Services you reduce the overhead required to maintain multiple layers of translation.

These Services are likely to evolve as new functionality is created internally, or requested from external consumers. However for one off integration requirements, a single layer of translation will be better than compromising the generic Service interface.

An example of using the Open Host strategy in our Ecommerce application could be providing data about the customers and transaction as a series of Services.

Many of the existing applications inside the company will likely need to request data from our application in order to fulfil orders or restock products, and so instead of providing layers of translation for each system, we can provide a set of Services to reduce this overhead.


##### Published Language

Integrating the model from an external system into a new system is difficult because you don't want your new system to be influenced by the design of the external system.

The external system's model is likely going to be incompatible with your internal model and so accepting their model as a data exchange language can mean your model is dependent and influenced by the external system.

Instead of relying on the model as a data exchange language, we should use common published languages such as JSON or XML that allow data to be translated between different systems using a common format.

An example of using a Published Language in our Ecommerce analogy could be updating an Messaging application whenever a new transaction occurs. The Messaging application will send emails to the customer to notify them of product updates, discounts and related products.

We can ensure that the data is compatible with the Messaging application by sending details of the request as JSON or XML. By sending the data as a common format we aren't forcing the Messaging system to conform to our internal model or make compromises in how they design their internal application.


##### Separate Ways

In many situations, integration can be more costly than it is worth. This could be due a Conformist situation, or perhaps the other team is just too difficult to work with.

If the functionality or data between the two system is not inherently linked, just because it is related, does not mean that it should be integrated.

Instead of trying to force an integration, allowing two systems to go their Separate Ways is another attractive option.

An example of Separate Ways in our Ecommerce analogy could be that of reporting financial statistic to the retailer's existing finance department. The finance department uses an archaic system that would take a long time to integrate with our new ecommerce application.

The Financial liabilities of the company need to be reported weekly, monthly, quarterly and yearly. Investing time to build integration between the two systems for real-time reporting is superfluous to our needs.

Instead we can simply ensure that financial reports can be exported in the required format. This means the two systems don't need to be integrated at all and so we lose the overhead of trying to make the integration work.


## Conclusion

It's almost inevitable that you will need to integrate with existing applications, third-party services or multiple Bounded Context in any non-trivial application.

Many different types of companies around the world can reap huge productivity benefits from integrating new and existing systems within their organisation.

Understanding the common patterns around integration will be a huge asset when you are tasked to integrate two very different systems.

Integration is also something important to keep in mind when developing a totally new application. By providing your application as a series of services, you will make any future integration requirements with your application a whole lot easier!

The power of software is really magnified when distributed systems can be integrated and leveraged as a whole. Understanding how to integrate applications under different circumstances is very valuable knowledge to possess.


## Moving to a new Bounded Context

Consider the functionalities such as: register new user, password reminders, forgot passwords, and followers modules for social interaction between users. All of these functionalities fits neatly into the **Identity** namespace as unified model. Which is helps to easily understand and maintain because all of the related code in the same place. 

**Identity ** namespace is in its own bounded context.


### Why choose a separate Bounded Context?



*   If we aren't strict about the scope of Bounded Contexts we can quickly find ourselves in a position where certain aspects of the application have too much responsibility. At some point we have to draw the line as to what is allowed in and what is kept out.
*   The context of a User is different in the Identity Bounded Context than it is in the Groups Bounded Context. In the Identity Bounded Context, the User is the focal point of the functionality and so basically everything will flow through that key Entity. In the Group Bounded Context, it is the Group Entity that is the most important.
*   we don't have the concept of Users in the Group Bounded Context, instead we have a more granular definition of _Members_ and _Admins_. Members and admins will still be both "Users", but they will likely have different roles and responsibilities within this Bounded Context.


## A Pattern for Sharing Data Across Bounded Context

There are a number of different ways to leverage common data across bounded contexts.



*   mirroring data from one system to another where the first system is designed for editing that data and the second just needs read-only access to a bit of that data.

The implementation involves a number of working parts, including Inversion of Control (IoC) containers and message queues.

The Sample Scenario: Sharing a Customer List.

System A is dedicated to customer maintenance. Here, users can maintain customer data, along with plenty of other data.  This system interacts with a data storage mechanism, but that isn't important to the sample. The second system B is designed for taking orders. In that system, users need access to customers, but really only to identify the customer making the order. That means this bounded context needs just a read-only list of customer names and identifiers. Therefore, the database connected to the second system needs an up-to-date list of customer names and IDs based on the customers that are maintained in the first system.


### Mirroring Data: High Level

At a very high level, the solution I'll apply is that every time a customer is inserted into System A, the ID and name of that customer should be added to System B's data store. If I change an existing customer's name in System A, then System B needs to have the correct name as well, so a name change should cause an update in the System B data store. This domain doesn't delete data, but a future enhancement could be to remove inactive customers from the System B data store.

So, I care about only two events in System A to which I need to respond:



*   A customer was created
*   An existing customer's name was changed

In a connected system, System B could expose methods for System A to call, such as CreateACustomer or ChangeCustomerName. Or System A could raise events, such as CustomerWasCreated and CustomerNameChanged, for other systems, including System B, to catch and respond to.

In response to each event, System B needs to do something in its database.

Because these systems are disconnected, a better approach is to employ a **publish-subscribe pattern**. System A will publish one or more events to some type of operator. And one or more systems then subscribe to that same operator, waiting for particular events and performing their own actions in response to those events.

Publish-subscribe aligns with the principles of DDD that require these two systems be unaware of each other and, therefore, not talk directly to one another. So, instead, I'll use a concept called an anti-corruption layer. Each system will communicate through the operator that will shuffle messages between the two.

This operator is a message queue. System A will send messages to the queue. System B will retrieve messages from the queue. In my example, I'll have only one subscriber—System B—but it's possible to have many subscribers.


### What is in the Event Message?

When the event being published is the CustomerWasCreated event, System A will send a message that says, "A customer has been created. Here's the customer's identity and name." That's the full message except that it's represented in data, not in nice English sentences. The interesting part about publishing an event to a message queue is that the publisher doesn't care what systems are retrieving the message or what they'll do in response.

System B will respond to that message by inserting or updating the customer in its database. In reality, System B doesn't even perform this task; I'll let a service do the job. Moreover, I'll let the database determine how an update is to be performed. In this case, the System B database will execute a customer "update" using a stored procedure whose logic deletes the original customer record and inserts a new one. Because I'll be using GUIDs as identity keys, the identity of the customer will be maintained properly. I don't want to worry about database-generated keys in my DDD software. Pre-created GUIDs vs. database-incremented keys is a sticky topic. You'll need to define your logic to align with your company's database practices.

In the end, System B, the ordering system, will have a complete list of customers it can use. Further along in the workflow, if the ordering system needs more information about a particular customer, for example credit card information or current shipping address, I can leverage other mechanisms, such as calling a service, to retrieve that data as needed. I won't be addressing that workflow here, however.


### Communicating with the Message Queue

A system that allows you to communicate messages in an asynchronous way is called an event bus. An event bus contains the infrastructure to store messages and provide them to whoever needs to retrieve them. It also provides an API for interacting with it.  I'll focus on one particular implementation that I found to be an easy way to get started with this style of communication: a message queue. There are a number of message queues from which to choose. 

In writing this article, I decided it was time for me to learn to use one of the more popular open source message queues, RabbitMQ. This meant installing the RabbitMQ server (and Erlang!) onto my computer, as well as pulling in the RabbitMQ .NET Client so I could easily code against it in my application. You can learn more about RabbitMQ at[ rabbitmq.com](http://rabbitmq.com/). 

System A, therefore, has a mechanism for sending messages to the RabbitMQ server. But System B, the Order system, has nothing to do with any of this interaction. System B simply expects the customer list to be in the database and doesn't care how it gets there. A separate small Windows service will handle checking the RabbitMQ message queue for messages and updating the Order system's database accordingly. **Figure 1** shows a visual workflow of the entire process.



<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/DDD5.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/DDD5.png "image_tooltip")


**Figure 1 A Message Queue Allows Unacquainted Systems to Share Messages, in This Case to Update the System B Database**


### Sending Messages to the Queue

I'll start with the Customer class in System A, For the sake of a simple example, it comprises only a few properties—ID, Name, the source of the customer and some logging dates. Following DDD patterns, the object has built-in constraints to prevent random editing. You create a new customer using the Create factory method. If you need to fix the name, you use the FixName method.

 \


Notice that the constructor and the FixName method both call the PublishEvent method, which, in turn, creates a simple CustomerDto (which has only an Id and Name property) and then uses the DomainEvents class from Udi Dahan's 2009 MSDN Magazine article, "Employing the Domain Model Pattern" ([msdn.microsoft.com/magazine/ee236415](https://msdn.microsoft.com/magazine/ee236415)) to raise a new Customer­UpdatedEvent (see **Figure 3**). In my example, I'm publishing the event in response to simple actions. In a real implementation, you might prefer to publish these events after the data has been successfully persisted into the System A database.

Figure 3 A Class That Encapsulates an Event When a Customer Is Updated

public class CustomerUpdatedEvent : IApplicationEvent{ \
  public CustomerUpdatedEvent(CustomerDto customer,  \
   bool isNew) : this(){ \
	Customer = customer; \
	IsNew = isNew; \
  } \
  public CustomerUpdatedEvent() \
  { \
	DateTimeEventOccurred = DateTime.Now; \
  } \
  public CustomerDto Customer { get; private set; } \
  public bool IsNew { get; private set; } \
  public DateTime DateTimeEventOccurred { get; set; } \
  public string EventType{ \
	get { return "CustomerUpdatedEvent"; } \
 }} \


A CustomerUpdatedEvent wrap's all I need to know about this event: the CustomerDto along with a flag that indicates whether the customer is new. There's also metadata that will be needed by a generic event handler.

The CustomerUpdatedEvent can then be handled by one or more handlers I define in my application. I've defined only one handler, a service called CustomerUpdatedService:

public class CustomerUpdatedService : IHandle<CustomerUpdatedEvent> \
{ \
  private readonly IMessagePublisher _messagePublisher; \
  public CustomerUpdatedService(IMessagePublisher messagePublisher){ \
	_messagePublisher = messagePublisher; \
  } \
  public void Handle(CustomerUpdatedEvent customerUpdatedEvent){ \
	_messagePublisher.Publish(customerUpdatedEvent); \
}} \


The service will handle all CustomerUpdatedEvent instances my code raises by using the specified message publisher to publish the event. I haven't specified the publisher here; I've only referenced an abstraction, IMessagePublisher. I'm employing an IoC pattern that lets me loosely couple my logic. I'm a fickle gal. Today I may want one message publisher. Tomorrow, I might prefer another. In the background, I used StructureMap (structuremap.net), a popular tool among .NET developers for managing IoC in .NET applications. StructureMap lets me indicate where to find the classes that handle events raised by DomainEvents.Raise. StructureMap's author, Jeremy Miller, wrote an excellent series in MSDN Magazine called "Patterns in Practice" that's relevant to the patterns applied in this sample ([bit.ly/1ltTgTw](http://bit.ly/1ltTgTw)). With StructureMap, I configured my application to know that when it sees IMessagePublisher, it should use the concrete class, RabbitMQMessagePublisher, whose logic is shown here:

public class RabbitMqMessagePublisher : IMessagePublisher{ \
  public void Publish(Shared.Interfaces.IApplicationEvent applicationEvent) { \
	var factory = new ConnectionFactory(); \
	IConnection conn = factory.CreateConnection(); \
	using (IModel channel = conn.CreateModel()) { \
  	[code to define the RabbitMQ channel] \
  	string json = JsonConvert.SerializeObject(applicationEvent, Formatting.None); \
  	byte[] messageBodyBytes = System.Text.Encoding.UTF8.GetBytes(json); \
  	channel.BasicPublish("CustomerUpdate", "", props, messageBodyBytes); \
 }}} \


Note that I've removed a number of lines of code specific to configuring RabbitMQ. You can see the full listing in the download ([msdn.microsoft.com/magazine/msdnmag1014](https://msdn.microsoft.com/magazine/msdnmag1014)).

The meat of this method is that it publishes a JSON representation of the event object into the queue. Here's what that string looks like when I've added a new customer named Julie Lerman:

{ \
"Customer": \
  {"CustomerId":"a9c8b56f-6112-42da-9411-511b1a05d814", \
	"ClientName":"Julie Lerman"}, \
"IsNew":true, \
"DateTimeEventOccurred":"2014-07-22T13:46:09.6661355-04:00", \
"EventType":"CustomerUpdatedEvent" \
} \


When this message has been published, the Customer Maintenance system's involvement is complete.

In my sample application, I use a set of tests to cause messages to be published to the queue, as shown in **Figure 4**. Rather than build tests that will check the queue when they're finished, I simply browse to the RabbitMQ Manager on my computer and use its tooling. Notice in the test constructor I initialize a class called IoC. This is where I've configured StructureMap to wire up the IMessagePublisher and my event handlers.

Figure 4 Publishing to RabbitMq in Tests

[TestClass] \
public class PublishToRabbitMqTests \
{ \
  public PublishToRabbitMqTests() \
  {IoC.Initialize(); \
  } \
  [TestMethod] \
  public void CanInsertNewCustomer() \
  { \
	var customer = Customer.Create("Julie Lerman",  \
      "Friend Referral"); \
	Assert.Inconclusive("Check RabbitMQ Manager for a message re this event"); \
  } \
  [TestMethod] \
  public void CanUpdateCustomer() { \
	var customer = Customer.Create("Julie Lerman",  \
      "Friend Referral"); \
	customer.FixName("Sampson"); \
	Assert.Inconclusive("Check RabbitMQ Manager for 2 messages re these events"); \
}} \



### Retrieving the Message and Updating the Order System's Database

The message sits on the RabbitMQ server waiting to be retrieved. And that task is performed by a Windows service that runs continuously, periodically polling the queue for new messages. When it sees a message, the service retrieves and handles it. The message can also be handled by other subscribers as they come along. For the sake of this sample, I created a simple console application, rather than a service. This allows me to easily run and debug the "service" from Visual Studio while learning. For the next iteration, I might check out Microsoft Azure WebJobs ([bit.ly/1l3PTYH](http://bit.ly/1l3PTYH)), rather than tangling with a Windows service or using my console application hack.

The service employs similar patterns of raising events with Dahan's DomainEvents class, responding to events in a handler class and initializing an IoC class that uses StructureMap to locate the event handlers.

The service listens to RabbitMQ for messages using the RabbitMQ .NET Client Subscription class. You can see the logic for this in the following Poll method, where the _subscription object keeps listening for messages. Every time a message is retrieved, it deserializes the JSON I stored in the queue back into a CustomerUpdatedEvent and then raises the event:

private void Poll() { \
  while (Enabled) { \
	var deliveryArgs = _subscription.Next(); \
	var message = Encoding.Default.GetString(deliveryArgs.Body); \
	var customerUpdatedEvent = \
  	JsonConvert.DeserializeObject<CustomerUpdatedEvent>(message); \
	DomainEvents.Raise(customerUpdatedEvent); \
}} \


The service contains a single class, Customer:

public class Customer{ \
  public Guid CustomerId { get; set; } \
  public string ClientName { get; set; } \
} \


When the CustomerUpdatedEvent is deserialized, its Customer property—originally populated by a CustomerDto in the Customer Management system—deserializes to this service's Customer object.

What happens to the raised event is the most interesting part of the service. Here's the class, CustomerUpdatedHandler, that handles the event:

public class CustomerUpdatedHandler : IHandle<CustomerUpdatedEvent>{ \
  public void Handle(CustomerUpdatedEvent customerUpdatedEvent){ \
	var customer = customerUpdatedEvent.Customer; \
	using (var repo = new SimpleRepo()){ \
  	if (customerUpdatedEvent.IsNew){ \
    	repo.InsertCustomer(customer); \
  	} \
  	else{ \
    	repo.UpdateCustomer(customer); \
 }}}} \


This service uses Entity Framework (EF) to interact with the database. **Figure 5** shows the relevant interaction is encapsulated in two methods—InsertCustomer and UpdateCustomer—in a simple repository. If the event's IsNew property is true, the service calls the repository's InsertCustomer method. Otherwise, it calls the UpdateCustomer method.

Figure 5 The InsertCustomer and UpdateCustomer Methods

public void InsertCustomer(Customer customer){ \
  using (var context = new CustomersContext()){ \
	context.Customers.Add(customer); \
	context.SaveChanges(); \
}} \
public void UpdateCustomer(Customer customer){ \
  using (var context = new CustomersContext()){ \
	var pId = new SqlParameter("@Id", customer.CustomerId); \
	var pName = new SqlParameter("@Name", customer.ClientName); \
	context.Database.ExecuteSqlCommand  	 \
      ("exec ReplaceCustomer {0}, {1}",  \
	    customer.CustomerId, customer.ClientName); \
}} \


Those methods perform the relevant logic using an EF DbContext. For an insert, it adds the customer and then calls SaveChanges. EF will perform the database insert command. For an update, it will send the CustomerID and CustomerName to a stored procedure, which uses whatever logic I—or my trusted DBA—has defined to perform the update.

Therefore, the service performs the necessary work in the database to make sure the Customers list in the Ordering system always has an up-to-date roster of customers as maintained in the Customer Maintenance system.

[https://blog.forma-pro.com/getting-started-with-rabbitmq-in-php-84d331e20a66](https://blog.forma-pro.com/getting-started-with-rabbitmq-in-php-84d331e20a66)

[https://blog.forma-pro.com/getting-started-with-rabbitmq-in-symfony-cb06e0b674f1](https://blog.forma-pro.com/getting-started-with-rabbitmq-in-symfony-cb06e0b674f1)

[https://novemberfive.co/blog/real-world-implementation-of-symfony-events](https://novemberfive.co/blog/real-world-implementation-of-symfony-events)


### Yes, This Is a Lot of Layers and Puzzle Pieces!

Because I used a simple sample to demonstrate this workflow, you might be thinking the solution is an incredible amount of overkill. But remember, the point is that this is how to orchestrate the solution when you're using DDD practices to solve complex software problems. When focusing on the domain of customer maintenance, I don't care about other systems. By using abstractions with the IoC, handlers and message queues, I can to satisfy needs of external systems without muddying up the domain itself. The Customer class simply raises an event. For this demo, this is the easiest place to ensure the workflow makes sense to readers, but it might already be too muddy for your domain. You can certainly raise the event from another place in your application, perhaps from a repository just as it is about to push changes into its own data store.

The sample solution download for this column does employ RabbitMQ and that requires installing its lightweight, open source server on your computer. I've included references in the download's ReadMe file. I'll also post a short video on my blog at[ thedatafarm.com](http://thedatafarm.com/), so you can see me stepping through the code, inspecting the RabbitMQ Manager and the database to see the results.

[https://msdn.microsoft.com/en-us/magazine/dn802601.aspx](https://msdn.microsoft.com/en-us/magazine/dn802601.aspx)

[https://github.com/php-enqueue/enqueue-dev/blob/master/docs/index.md](https://github.com/php-enqueue/enqueue-dev/blob/master/docs/index.md)


## RabbitMQ

[https://www.sitepoint.com/use-rabbitmq-php/](https://www.sitepoint.com/use-rabbitmq-php/)


```

```


**Data Mapper Pattern** promotes the the separation of the application's Business Rules from how the data is persisted to the database. This is in contrast to the **Active Record Pattern**.

[http://culttt.com/2015/01/14/command-query-responsibility-segregation-cqrs/](http://culttt.com/2015/01/14/command-query-responsibility-segregation-cqrs/)

[http://culttt.com/2015/01/07/service-oriented-architecture/](http://culttt.com/2015/01/07/service-oriented-architecture/)

[http://culttt.com/2015/01/05/using-aggregates-gateway-functionality/](http://culttt.com/2015/01/05/using-aggregates-gateway-functionality/)

[http://culttt.com/2014/12/31/hexagonal-architecture/](http://culttt.com/2014/12/31/hexagonal-architecture/)

[http://culttt.com/2014/12/29/enforcing-business-rules-aggregate-instantiation/](http://culttt.com/2014/12/29/enforcing-business-rules-aggregate-instantiation/)

[http://culttt.com/2014/12/24/factories-domain-driven-design/](http://culttt.com/2014/12/24/factories-domain-driven-design/)

[http://culttt.com/2014/12/22/modelling-discussion-forum/](http://culttt.com/2014/12/22/modelling-discussion-forum/)

[http://culttt.com/2014/12/17/aggregates-domain-driven-design/](http://culttt.com/2014/12/17/aggregates-domain-driven-design/)

http://culttt.com/2014/12/15/using-entities-different-bounded-contexts/

[http://culttt.com/2014/12/10/modules-domain-driven-design/](http://culttt.com/2014/12/10/modules-domain-driven-design/)

[http://culttt.com/2014/12/08/creating-domain-objects-recap/](http://culttt.com/2014/12/08/creating-domain-objects-recap/)

[http://culttt.com/2014/12/03/6-principles-writing-maintainable-code/](http://culttt.com/2014/12/03/6-principles-writing-maintainable-code/)

[http://culttt.com/2014/12/01/moving-new-bounded-context/](http://culttt.com/2014/12/01/moving-new-bounded-context/)

[http://rosstuck.com/domain-events-on-creation/](http://rosstuck.com/domain-events-on-creation/)

[http://rosstuck.com/persisting-value-objects-in-doctrine/](http://rosstuck.com/persisting-value-objects-in-doctrine/)


# **REST and DDD**

[https://yuloh.github.io/2016/rest-and-ddd/](https://yuloh.github.io/2016/rest-and-ddd/)

[http://shadowhand.me/the-binary-classification-of-objects/](http://shadowhand.me/the-binary-classification-of-objects/)

[https://www.rabbitmq.com/devtools.html#php-dev](https://www.rabbitmq.com/devtools.html#php-dev)


## Event sourcing with GDPR

As many already know GDPR came into effect since end of May 2018. As per GDPR customer has right to erasure of right to be forgotten. In my event sourcing application I am having customer sensitive data. 

For whole immutability purpose I do not want to delete or update events. I am familiar with one of the approach which is encrypting data for each customer with it's own key. But this approach has couple of disadvantages

1. I cannot perform search in events (I am using Mongodb to store events)

2. Maintaining key

I wanted to know if anyone else has tried any other approach?


<!-- Docs to Markdown version 1.0β14 -->
