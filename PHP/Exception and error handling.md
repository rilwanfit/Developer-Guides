## Errors and Exceptions


###### print the call stack in PHP


```
$e = new \Exception;
var_dump($e->getTraceAsString());
```


[http://php.net/manual/en/function.debug-backtrace.php](http://php.net/manual/en/function.debug-backtrace.php)

[http://php.net/manual/en/function.debug-print-backtrace.php](http://php.net/manual/en/function.debug-print-backtrace.php)


######  Get the last occurred error



1.  echo $x;
1.    print_r(error_get_last());


### Exceptions


#### What is Exception?

Exception is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions. When you **throw** (create an exception object and hand it to the runtime system) an exception, the system will **catch** it by searching for the appropriate handler and returning the proper message.

There are several key differences between "regular" PHP errors and exceptions



*   Exceptions are objects, created (or "thrown") when an error occurs
*   Exceptions can be handled at different points in a script's execution,

and different types of exceptions can be handled by separate portions

of a script's code



*   All unhandled exceptions are fatal
*   Exceptions can be thrown from the __construct method on failure
*   Exceptions change the flow of the application

Exception handle handled and user can continue the application without an issue But Error will kill the entire application and stop user to continue.


#### When Do we need to use Exceptions?

Use exceptions when your system is faced with exceptional circumstances that prevent the system from taking over. We use exceptions

only when the system is not able to determine what has happened. Martin Fowler believes that "**if a failure is expected behavior, then you **

**shouldn't be using exceptions**". This means you should use exceptions where you are not able to determine the error. Exceptions should

only be used in exceptional circumstances.

Note: Exceptions are not good for handling logic operations.

For a system like validating input, using exceptions is wrong. First of all, the input text will be determined, and in an application like this

we should report a collection of errors instead of a single error. I believe in eliminating the use of exceptions in any circumstance where

we might expect validation failures.

Catching an exception is very important, because if you do not catch an exception the system will return an error. The catching operation

must be as close to the point of failure as possible.


#### Throwing Exceptions

Exceptions are usually created and thrown when an error occurs by using the **throw** construct:

Although it is common practice to do so, you don't need to create the Exception object directly in the throw expression. You may

instantiate the exception object at any time and assign it to a variable to throw later.


```
if ($error) {
   throw new Exception("This is my error");
}
```


Exceptions then "bubble up" until they are either handled by the script or cause a fatal exception. The handling of exceptions is performed

using a try...catch block:


```
try {
   if ($error) {
       throw new \Exception("This is my error");
   }
} catch (\Exception $e) {
   // Handle exception
}
```

Note how the catch() portion of the statement requires us to hint the type of Exception that we want to catch; one of the best features of exceptions is the fact that you can decide which kind of exception to trap.

Note: You will need to fully qualify your exception hints if you are using namespaces!

Since you are free to extend the base \Exception class, this means that different nested try..catch blocks can be used to trap and deal with different types of errors:

```
namespace myCode;
class Exception extends \Exception { }
try {
   try {
       try {
           new PDO("mysql:dbname=zce");
           throw new myException("An unknown error occurred.");
       } catch (\PDOException $e) {
           echo $e->getMessage();
       }
   } catch(\myCode\Exception $e) {
       echo $e->getMessage();
   }
} catch (\Exception $e) {
   echo $e->getMessage();
}
```

we have three nested try... catch blocks; the innermost one will only catch \PDOException objects, while the next will catch the custom \myCode\Exception objects, and the outermost will catch any other exceptions that we might have missed. Rather than nesting the try...catch blocks like we did above, you can also chain just the catch blocks:

```
try {
   new PDO("mysql:dbname=zce");
   throw new myException("An unknown error occurred.");
} catch (\PDOException $e) {
   echo $e->getMessage();
} catch (\myCode\Exception $e) {
   echo $e->getMessage();
} catch (\Exception $e) {
   echo $e->getMessage();
}
```


Once an exception has been caught, execution of the script will follow from directly after the last catch block.

To avoid fatal errors from uncaught exceptions, you could wrap your entire application in a try... catch block, but this would be rather inconvenient. Luckily, there is a better solution: PHP allows us to define a "catch-all" function that is automatically called whenever an exception is not handled. This function is set up by calling **set_exception_handler**():


```
function handleUncaughtException($e) {
   echo $e->getMessage();
}
set_exception_handler("handleUncaughtException");
throw new Exception("You caught me!");
echo "This is never displayed";
```


Note that, because the catch-all exception handler is only called after the exception has bubbled up through the entire script, it, just like an all-encompassing try... catch block, is the end of the line for your code.

In other words, the exception has already caused a fatal error, and you are given the opportunity to handle it, but are not given the opportunity to recover from it. For example, the code above will never output You caught me!, because the exception thrown will bubble up and cause **handleUncaughtException**() to be executed; the script will then terminate.

If you wish to restore the previously used exception handler, be it the default of a fatal error or another user defined callback, you can use restore_exception_handler().

New in PHP 5.5: With PHP 5.5 you can pass NULL to set_exception_handler() to return to the default PHP behavior.

When an Exception thrown, the next line of code will never get executed, unless we handle the exception ex:


```
throw new Exception("Not Handled Yet!");
return "Something";
```


Second line of code will not get executed.


```
try {
   throw new myException("An unknown error occurred.");
} catch (Exception $e) { }
return "Something";
```


Now the return statement will return something.


#### When to throw exception.

Never fail silently instead throw an exception that will describe exactly what went wrong.


#### Finally

With PHP 5.5, a finally block was added. Code within the finally block will be executed regardless of whether an exception has been thrown or not, making it useful for

things like cleanup.


```
try {
   // Try something
} catch (\Exception $exception) {
   // Handle exception
} finally {
   // Whatever happened, do this
}
```



#### Throwing Your Own Exceptions

we can throw our own exceptions** by extending the Exception class**:


```
class ModelNotFoundException extends Exception {}

// something wrong, throw our custom exception
throw new ModelNotFoundException('some message');
```


That's easy but one might ask why throw custom exceptions?



*   It makes it easy to recognize which thing (library, class, extension, etc) generated exception in code hierarchy
*   This helps developer of original library easily spot problems and handle it in their code
*   It brands your library exceptions like PDO, DOM, etc do.

NOTE:


### **Changes in Errors and Exception Handling**

In PHP 7 changes have been made to default error and exception handling. Therefore, we can' not use custom error handlers as before as now exceptions thrown for those errors.


### **Exception handlers will no longer receive just Exception objects**

You can configure an exception handler to handle regular exceptions and exceptions that used to be errors. So the handler function may receive either objects of type Exception or Error.

Since both objects implement the Throwable interface, if you want to use type hinting it is better to declare your handler like this, otherwise it may cause fatal errors and not work as you intended.

void handler( Throwable $ex);

[http://www.phpclasses.org/blog/post/357-PHP-7-Migration-Guide-Part-1-10-Backwards-Incompatibile-Changes.html](http://www.phpclasses.org/blog/post/357-PHP-7-Migration-Guide-Part-1-10-Backwards-Incompatibile-Changes.html)


### Formatting Exception messages

[http://rosstuck.com/formatting-exception-messages/](http://rosstuck.com/formatting-exception-messages/)

[https://github.com/M6Web/ApiExceptionBundle](https://github.com/M6Web/ApiExceptionBundle)


### Errors

[http://php.net/manual/en/language.errors.php7.php](http://php.net/manual/en/language.errors.php7.php)

[https://blog.feryn.eu/type-errors-php-7/](https://blog.feryn.eu/type-errors-php-7/)


#### API Errors

[https://akrabat.com/api-errors-are-first-class-citizens/](https://akrabat.com/api-errors-are-first-class-citizens/)


#### Display pretty errors with Whoops

[https://murze.be/2016/01/display-pretty-errors-with-whoops/](https://murze.be/2016/01/display-pretty-errors-with-whoops/)

[https://daveyshafik.com/archives/69237-an-exceptional-change-in-php-7-0.html](https://daveyshafik.com/archives/69237-an-exceptional-change-in-php-7-0.html)


## Logging



*   Most ubiquitous task
*   Logs to track error messages, record important events, and debug problem with our code.
*   Scattering code with full of logging calls make the code dependent on the availability of logging library, it is a clear violation of dependency inversion principle.


### PSR-3



*   provides a common interface for logger objects.
*   This interface exposes **eight** methods for handling messages of different severity, and a generic log() method which can accept an arbitrary severity level
    *   Emergency – the system is unusable
    *   Alert – immediate action is required
    *   Critical – critical conditions
    *   Error – errors that do not require immediate attention but should be monitored
    *   Warning – unusual or undesirable occurrences that are not errors
    *   Notice – normal but significant events
    *   Info – interesting events
    *   Debug – detailed information for debugging purposes

```composer require psr/log```





### **How Logging Can Limit Code Reuse**

There are many different logging libraries for PHP, each with its own approach to collecting and recording data. While there are some common ideas among them, each library has its own unique set of logging methods. This means that switching between loggers can be challenging, often requiring code changes wherever logging is used. This works against the idea of code reuse and the SOLID principles of object-oriented design. We're left with a situation that requires us to either declare a dependency on a specific logging library, or avoid logging entirely.

To illustrate this problem more clearly, a concrete example is needed. Let's say we're creating a simple Mailer object to handle sending emails. We want the Mailer to log a message whenever an email is sent, and we decide to use the excellent Monolog library to handle our logging needs.


<table>
  <tr>
   <td><strong>namespace </strong>Email;
<p>
<strong>class </strong>Mailer
<p>
{
<p>
   <strong>private </strong>$logger;
<p>
   <strong>public function </strong>__construct($logger)
<p>
   {
<p>
       $this->logger = $logger;
<p>
   }
<p>
   <strong>public function </strong>sendEmail($emailAddress)
<p>
   {
<p>
       // code to send an email...
<p>
       // log a message
<p>
       $this->logger->addInfo("Email sent to $emailAddress");
<p>
   }
<p>
}
   </td>
   <td>$logger = <strong>new </strong>\Monolog\Logger("Mail");
<p>
$logger->pushHandler(<strong>new </strong>\Monolog\Handler\StreamHandler("mail.log"));
<p>
// create the mailer and send an email
<p>
$mailer = <strong>new </strong>Mailer($logger);
<p>
$mailer->sendEmail("rilwanfit@gmail.com");
   </td>
  </tr>
</table>


Running this code will result in a new entry in the mail.log file, recording that the email was sent.

At this point, we might be tempted to say that we've written a reusable Mailer object. We're making the logger available to the Mailer with dependency injection so we can swap out different logger configurations without having to touch our Mailer code. It appears that we've successfully followed the SOLID principles and avoided creating any hard dependencies.

But suppose we want to reuse our Mailer class in a different project which uses Analog to handle logging interactions. Now we run into a problem, because Analog doesn't have an addInfo() method. To log an info level message with Analog, we call Analog::log($message, Analog::INFO).

We could modify our Mailer class to use Analog's methods, as seen below. **composer require analog/analog**


<table>
  <tr>
   <td><strong>public function </strong>sendEmail($emailAddress)
<p>
{
<p>
   // code to send an email...
<p>
   // log a message
<p>
   Analog::<em>log</em>("Email sent to $emailAddress", Analog::<em>INFO</em>);
<p>
}
   </td>
   <td>// set up Analog
<p>
Analog::<em>handler</em>(AnalogHandlerFile::<em>init</em>("mail.log"));
<p>
// create the mailer and send an email
<p>
$mailer = <strong>new </strong>Mailer();
<p>
$mailer->sendEmail("rilwanfit@gmail.com");
   </td>
  </tr>
</table>


While this will work, it is far from ideal. We've run into Mailer's dependency on a specific logging implementation, **which requires the class to change whenever a new logger is introduced**. This makes the class less reusable and forces us to choose between being dependent on the availability of a specific logger or abandoning logging from within our class entirely.


### Using PSR-3 to Avoid the Logger Dependency

the Dependency Inversion Principle tells us that we should depend upon abstractions rather than concretions. In the case of logging, our problem so far has been a lack of a decent abstraction on which to depend.

This is where PSR-3 comes in. PSR-3 is designed to overcome the problem of incompatible logging implementations by providing a universal interface for loggers, aptly named LoggerInterface. By providing an interface which is not bound to any particular implementation, PSR-3 frees us from having to rely on a specific logger – we can instead type-hint against LoggerInterface to acquire a PSR-3 compliant logger. I have updated the Mailer class below to demonstrate this:


```
namespace Email;

class Mailer

{

    private $logger;


    public function __construct(PsrLogLoggerInterface $logger)

    {

        $this->logger = $logger;

    }


    public function sendEmail($emailAddress)

    {

        // code to send an email...

        // log a message

        $this->logger->info("Email sent to $emailAddress");

    }

}

The constructor has been modified to accept an implementor of LoggerInterface, and the sendEmail() method now calls the info() method specified in PSR-3.

Monolog is already PSR-3 compliant, and Analog supplies a wrapper object that implements LoggerInterface, so we can now use both loggers without modifying our Mailer class.

Here's how you would call the class with Monolog:


```

```


<?php

// create a Monolog object

$logger = new MonologLogger("Mail");

$logger->pushHandler(new MonologHandlerStreamHandler("mail.log"));



// create the mailer and send an email

$mailer = new EmailMailer($logger);

$mailer->sendEmail("email@example.com");

And with Analog:

<?php

// create a PSR-3 compatible Analog wrapper

$logger = new AnalogLogger();

$logger->handler(AnalogHandlerFile::init("mail.log"));



// create the mailer and send an email

$mailer = new EmailMailer($logger);

$mailer->sendEmail("email@example.com");

Now we're able to use our Mailer object with either library without having to edit the Mailer class or change the way we consume it.


### **Using the Adapter Pattern for Loggers that Don't Support PSR-3**

So far we've successfully decoupled the Mailer object from any particular logging implementation by asking for an implementor of LoggerInterface. But what about loggers that have yet to add support for PSR-3? For example, the popular KLogger library has not been updated for some time and is currently not compatible with PSR-3.

Luckily, it's simple for us to map the methods exposed by KLogger to those defined by LoggerInterface by harnessing the power of the Adapter Pattern. The supporting files in the Psr/Log repository make it easy to create adapter classes by providing us with an AbstractLogger class that we can extend. The abstract class simply forwards the eight level-specific logging methods defined in LoggerInterface to a generic log() method. By extending the AbstractLogger class and defining our own log() method, we can easily create PSR-3 compliant adapters for loggers that do not natively support PSR-3. I'll demonstrate this below by creating a simple adapter for KLogger:


```

```


<?php

namespace Logger;



class KloggerAdapter extends PsrLogAbstractLogger implements PsrLogLoggerInterface

{

    private $logger;



    public function __construct($logger)

    {

        $this->logger = $logger;

    }



    public function log($level, $message, array $context = array())

    {

        // PSR-3 states that $message should be a string

        $message = (string)$message;



        // map $level to the relevant KLogger method

        switch ($level) {

            case PsrLogLogLevel::EMERGENCY:

                $this->logger->logEmerg($message, $context);

                break;

            case PsrLogLogLevel::ALERT:

                $this->logger->logAlert($message, $context);

                break;

            case PsrLogLogLevel::CRITICAL:

                $this->logger->logCrit($message, $context);

                break;

            case PsrLogLogLevel::ERROR:

                $this->logger->logError($message, $context);

                break;

            case PsrLogLogLevel::WARNING:

                $this->logger->logWarn($message, $context);

                break;

            case PsrLogLogLevel::NOTICE:

                $this->logger->logNotice($message, $context);

                break;

            case PsrLogLogLevel::INFO:

                $this->logger->logInfo($message, $context);

                break;

            case PsrLogLogLevel::DEBUG:

                // KLogger logDebug() method does not accept a second

                // argument

                $this->logger->logDebug($message);

                break;

            default:

                // PSR-3 states that we must throw a

                // PsrLogInvalidArgumentException if we don't

                // recognize the level

                throw new PsrLogInvalidArgumentException(

                    "Unknown severity level"

                );

        }

    }

}

The log() method simply maps the LoggerInterface methods to the respective KLogger methods, and KLogger handles the actual logging activity. By wrapping the KLogger class in this way, we're able to use it without breaking the LoggerInterface contract.

We can now use the KLogger adapter to work with the Mailer class:


```

```


<?php

// create a new KLogger object which will log to the "logs" directory

$klogger = KLogger::instance("logs");



// inject KLoggger to a PSR-3 compatible KloggerAdapter

$logger = new LoggerKloggerAdapter($klogger);



// send an email

$mailer = new EmailMailer($logger);

$mailer->sendEmail("email@example.com");

Using the adapter class, we're able to use KLogger without modifying the Mailer class and still adhering to LoggerInterface.

KLogger doesn't accept a second argument for debug level messages, so it's not technically PSR-3 compliant even with an adapter. Extending KLogger to make it fully compatible with PSR-3 would be a trivial task, but one which falls outside the scope of this article. However, it is safe to say that using our adapter class gets us very close to full PSR-3 compliance and allows us to use LoggerInterface with the KLogger class.


### How to connect ELK with Monolog

ELK is an awesome stack for logging.

Monolog is an awesome PHP library for managing logs. [https://github.com/Seldaek/monolog](https://github.com/Seldaek/monolog)

In a nutshell, Monolog offers you a logger, where you send your logs. This logger has multiple handlers, which send these logs wherever you need them. Monolog has many handlers, which enable you to to simply send logs to many destinations, e.g. files, e-mails, slack, logstash, and many more.

ELK stack (now known as Elastic stack) stands for [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), [Kibana](https://www.elastic.co/products/kibana) stack.

**Logstash** is used to manage logs. It collects, parses and stores them. There is a lot of existing inputs, filters and outputs.

**Elasticsearch** is powerful, distributed NoSQL database with REST API. In ELK stack, Elasticsearch is used as persistent storage for our logs.

**Kibana** visualizes logs from Elasticsearch and lets you create many handy visualizations and dashboards which allow you to see all important metrics in one place.


#### Install ELK with docker

[https://pehapkari.cz/blog/2017/10/22/connecting-monolog-with-ELK/](https://pehapkari.cz/blog/2017/10/22/connecting-monolog-with-ELK/)

[https://www.elastic.co/webinars/introduction-elk-stack](https://www.elastic.co/webinars/introduction-elk-stack)
