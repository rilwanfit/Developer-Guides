---
id: messenger
title: Messenger
---

## Install messenger 
`composer req messenger`

## Command Bus Pattern
Most people will use Messenger as a "command bus"... which is sort of a design pattern. Here's the idea. Right now, we're doing all of our work in the controller. Well, ok, we've organized things into services, but our controller calls those methods directly. It's nicely-organized, but it's still basically procedural: you can read the code from top to bottom.

With a command bus, you separate what you want to happen - called a "command" - from the code that does that work. Imagine you're working as a waiter or waitress at a restaurant and someone wants a pizza margherita... with extra fresh basil! Mmm. Do you... run back to the kitchen and cook it yourself? Probably not... Instead, you write down the order. But... let's say instead, you write down a "command": cook a pizza, margherita style with extra fresh basil. Next, you "send" that command to the kitchen. And finally, some chef does all the magic to get that pizza ready. Meanwhile, you're able to take more orders and send more "commands" back to the kitchen.

This is a command bus: you create a simple, informational command "cook a pizza", give it to some central "system"... which is given that fancy word "bus", and it makes sure that something sees that command and "handles" it... in this case, a "chef" cooks the pizza. And that central "bus" is probably smart enough to have different people "handle" different commands: the chef cooks the pizza, but the bar tender prepares the drink orders.

### Creating the Command Class
```php
namespace App\Message;
class AddPonkaToImage
{
}
```

### Creating the Handler Class
```php
namespace App\MessageHandler;
use Symfony\Component\Messenger\Handler\MessageHandlerInterface;
class AddPonkaToImageHandler implements MessageHandlerInterface
{
}
```

- Must implement `MessageHandlerInterface`
- Must have a public function called `__invoke()` with a single argument that is type-hinted with the message class.

### Connecting the Message and Handler
`php bin/console debug:messenger`

### Dispatching the Message
1. In the controller action add a new argument with the `MessageBusInterface` type-hint.
2. Dispatch
```php
$messageBus->dispatch(new AddPonkaToImage());
```

- It's possible to remove logic from a controller action and add it in the Handler. Handler can be treated as a service;

## Transports
### Transport Types
- amqp - RabbitMQ
- redis
- doctrine

### Activating the transport
1. update the environment variable in `.env`
2. Update the config
```yaml
framework:
    messenger:
        transports:
            # https://symfony.com/doc/current/messenger.html#transports
            async: '%env(MESSENGER_TRANSPORT_DSN)%'
```
3. Setup the routing

Without routing the message will be handled immediately.  
```yaml
framework:
    messenger:
        routing:
            # Route your messages to the transports
            'App\Message\AddPonkaToImage': async
```

### Routing with Interfaces & Base Classes
Oh, and by the way: if you look at the routing section in messenger.yaml, you'll usually route thing by their exact class name: App\Message\AddPonkaToImage goes to async. But you can also route via interfaces or base classes. For example, if you have a bunch of classes that should go to the async transport, you could create your very own interface - maybe AsyncMessageInterface - make your messages implement that, then only need to route that interface to async here. But be careful because, if a class matches multiple routing lines, it will be sent to all those transports. Oh, and last thing - in case you have a use-case, each routing entry can send to multiple transports.

## Worker
`php bin/console messenger:consume -vv`
And... that's it! This messenger:consume command is something that you'll have running on production all the time. Heck, you might decide to run multiple worker processes. Or, you could even deploy your app to a totally different server - one that's not handling web requests - and run the worker processes there! Then, handling these messages wouldn't use any resources from your web server.

### Restarting the Worker
Find your terminal, press Ctrl+C to stop the worker, then restart it:

`php bin/console messenger:consume`
Why? As you know, workers sit there and run... forever. The problem is that, if you update some of your code, the worker won't see it! Until you restart it, it still has the old code stored in memory! So anytime you make a change to code that a worker uses, be sure to restart it. Later, we'll talk about how to do this safely when you deploy.

## Handling doctrine queries inside the message handler.
1. Pass only the required fields to the message object. e.i: `int $imagePostId` instead of `ImagePost` object.
2. then in the handler use the id to query for an object.

```php
class AddPonkaToImage
{
    private $imagePostId;
    public function __construct(int $imagePostId)
    {
        $this->imagePostId = $imagePostId;
    }
    public function getImagePostId(): int
    {
        return $this->imagePostId;
    }
}
```

```php
class AddPonkaToImageHandler implements MessageHandlerInterface
{
    private $imagePostRepository;
    public function __construct(ImagePostRepository $imagePostRepository)
    {
        $this->imagePostRepository = $imagePostRepository;
    }
    
    public function __invoke(AddPonkaToImage $addPonkaToImage)
    {
        $imagePostId = $addPonkaToImage->getImagePostId();
        $imagePost = $this->imagePostRepository->find($imagePostId);
    }
}
```
3. Logs
```php
class AddPonkaToImageHandler implements MessageHandlerInterface, LoggerAwareInterface
{
    use LoggerAwareTrait;
    public function __invoke(AddPonkaToImage $addPonkaToImage)
    {
        if (!$imagePost) {
            
        }
    }
```
- Starting in Symfony 4.2, there's a little shortcut to getting the main logger service. First, make your service implement LoggerAwareInterface. Then, use a trait called LoggerAwareTrait.

> whenever you see a user's service that implements LoggerAwareInterface, automatically call setLogger() on it and pass the logger.

## How to Fail in your Handler
what should we do if the ImagePost isn't found? We have two options... and the correct choice depends on the situation. First, we could throw an exception - any exception - and that would cause this message to be retried. More retries soon. Or, you could simply "return" and this message will "appear" to have been handled successfully... and will be removed from the queue.

## Dispatching a Message inside a Handler?

## Partial Handler Failures & Advanced Routing

## Envelopes & Stamps
- When you pass your message to the bus, internally, it gets wrapped inside something called an Envelope.
- If you have an Envelope, you can attach extra config to it via stamps. So yes, you literally put a message in an envelope and then attach stamps.

### Put the message into the Envolope and add Stamps
```php
$envelope = new Symfony\Component\Messenger\Envelope($message, [
    new DelayStamp(5000)
]);

$messageBus->dispatch($envelope);
```

> Side note: In Symfony 4.3, the Redis transport doesn't support delays - but it may be added in the future.

## Retrying on Failure

### make random failures
```php
if (rand(0, 10) < 7) {
    throw new \Exception('I failed randomly!!!!');
}
```
 by default, Messenger will retry a message three times. If it still fails, it's finally removed from the transport and the message is lost permanently. Well... that's not totally true... and there's a bit more going on here than it seems at first.

### Failure transport

## Available configurations for messenger
`php bin/console config:dump framework messenger`

## Current configuration for messenger
`php bin/console debug:config framework messenger`


