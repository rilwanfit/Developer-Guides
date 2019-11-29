---
id: mailer
title: Mailer
---

## Welcome email after registration
1. Make sure you have a controller to handle the user registration. if not run `bin/console make:registration-form`
2. Right after new user persisted to database we can send an email
3. 
```php
$mail = new Symfony\Component\Mime\Email();
```
The Mailer component in Symfony, we're actually talking about two components: Mailer and Mime. The Mime component is all about creating & configuring the email itself and Mailer is all about sending that email.

4. Configure the email
```php
$email = (new Email())
    ->from('alienmailcarrier@example.com')
    ->to($user->getEmail())
    ->subject('Welcome to the Space Bar!')
    ->text("Nice to meet you {$user->getFirstName()}! ❤️");
```

5. Send the email
 - Let's add that as one of the arguments to our controller method: `MailerInterface $mailer`
 - `$mailer->send($email);`
 
### Configuring MAILER_DSN
It is default to `MAILER_DSN=smtp://localhost` so, we need to setup a SMTP server locally. Its not recommended.
Still there is a better solution.  Instead of sending real emails to a real email server, you can send them to a "fake" mailbox. 
 - MailCatcher
 - MailHog
 - MailTrap.io hosted solution for above two options.
 
### How to configure mailtrap DSN
`smtp://username:password@smtp.mailtrap.io:2525`

### Email content with HTML
```php
$email = (new Email())
        ->text("Nice to meet you {$user->getFirstName()}! ❤️")
        ->html("<h1>Nice to meet you {$user->getFirstName()}! ❤️</h1>");
```

#### Use Twig for html content
1. create `template/email` directory and add a `welcome.html.twig` file
2. any content
3. Change `Email` class into `TemplatedEmail` class
4. replace the `->text()` and `->html()` by `->htmlTemplate('email/welcome.html.twig')`

> Paths must always be absolute in the email content
> The style tag doesn't work in gmail.  Mailer CSS Inlining will be helping us.

#### update the mail content
- `{{ url('app_homepage') }}` to generate url to homepage. note that we can not use `path()` because it will generate relative url i.e: /
- use inline styling
- for images use `asset()` as `{{ absolute_url(asset('build/images/email/logo.png')) }}`
- asset will generate relative url

#### Passing variables
```php
->context([
    'user' => $user,
]);
```
- The Built-in "app" and "email" Variables are available in templates
    -  "email" is an instance of `WrappedTemplatedEmail`
    - `{{ email.toName }}`

### NamedAddress and email.toName()
- `->to(new Symfony\Component\Mime\Address($user->getEmail(), $user->getFirstName()));`
- `->from(new NamedAddress('alienmailcarrier@example.com', 'The Space Bar'))`

### Html to markdown
it helps Mailer to better handling HTML emails
`composer req league/html-to-markdown`
- As soon as you install it, Mailer will automatically use it to transform the HTML email into text.
- the html-to-markdown library was smart enough to get rid of the CSS styles code

## Image handling
There are two ways to put an image into an email. 1. a normal, boring `img` tag that links to your site. 2. embed the image inside the email itself.

### how we can embed the logo?
1. The source logo image is located at assets/images/email/logo.png.
2. Adding a Twig Path to Images
```twig
twig:
    paths:
        'assets/images': images
```
When you render a template with Twig, by default, it knows to look for that file in the templates/ directory.
If you have template files that live somewhere else, that is where "paths" are handy. 
For example, pretend that, for some crazy reason, we decided to put a template inside the `assets/images/` directory called `dark-energy.html.twig`. 
Thanks to the item we added under paths, we could render that template by using a special path `@images/dark-energy.html.twig`.

3. Embedding an Image
```twig
<div class="body">
    <div class="container">
        <div class="header text-center">
            <a href="{{ url('homepage') }}">
                <img src="{{ email.image('@images/email/logo.png') }}" class="logo" alt="SpaceBar Logo">
            </a>
        </div>
```

### What is happening when we embed the image?
It hides the image Url. Woh! Instead of the image src being a URL that points to our site... it's some weird `cid` then a long string.

Check out the "Raw" tab. We already know that the content of the email has multiple parts: here's the text version, below is the text/html version and... below that, there is now a third part of the email content: the logo image! It has a Content-ID header - this long cfdf933 string - and then the image contents below.

The Content-Id is the key. Inside the message itself, that is what the cid is referring to. This tells the mail client to go find that "part" of the original message and display it here.

So it's kind of like an email attachment, except that it's displayed within the email.

## Automatic CSS Inlining
https://litmus.com/
There are two big rules you should follow if you want your emails to display consistently across all mail clients. 
First, use a table-based layout instead of floating or Flex-box. 
The second rule is that you can't use CSS files or even add a `<style>` tag. These will not work in gmail. then you literally need to add style="" to every HTML element. that's insane! There is a tool to help us so, we are not going to do that manually.

### Checking for the twig-pack
To get this all working, we need to check that a certain bundle is installed. If you started your project after October 2019, you can skip this because you will already have it.

For older projects, first make sure you have Twig 2.12 or higher: you can find your version by running: `composer show twig/twig`

If you find it old, Update it by running: `composer update twig/twig`

now run `composer require twig`

After October 16th, 2019 - to be exact - the twig alias will download symfony/twig-pack. The twig-pack will install the normal TwigBundle and a new twig/extra-bundle, which is a library that will help us use some new Twig features.

### The inline_css Filter
1. apply `{% apply inline_css %}.` on top of the email template
```twig
{% apply inline_css %}
...
{% endapply %}
```
> in Twig, you normally use a filter with the | symbol - like foo|inline_css. But if you want to run a lot of stuff through a filter, you can do it with this handy apply tag.

This requires you to have some other package installed. `composer require twig/cssinliner-extra`

## Inlining CSS Files
Lets refactor the email template.
1. Copy all of the styles and delete them. Inside the assets/css directory, let's create a new email.css file. Paste!
2. Namespace the styles path in twig
```twig
twig:
    paths:
        'assets/css': styles
```

3. Update the email template inline_css `{% apply inline_css(source('@styles/email.css')) %}`

> The source() function is a standard Twig function. It tells Twig to go find the file - which could be a CSS file or another Twig template - and return its contents. It's basically a file_get_contents() for Twig. That's perfect, because inline_css() doesn't want the path to a CSS file, it wants the string styles it should use.

## Ink: Automatic CSS Email Framework
https://foundation.zurb.com/emails/docs/css-guide.html

If you want to write an email that's going to look consistently good in every email client, the best practice is actually to use tables for your layout. Mailer has another trick up its sleeve.

### Hello Ink / Foundation for Emails
So, Ink, or Foundation for Emails is a CSS framework for responsive HTML emails that works on any device.
Foundation for emails is basically two parts. First, it's a CSS file that defines useful CSS classes and a grid structure for designing emails. Again... it's just like Bootstrap CSS for emails.

1. install `composer require twig/inky-extra`
2. apply `{% apply inky_to_html %}.` on top of the email template
   ```twig
   {% apply inky_to_html %}
   ...
   {% endapply %}
   ```
   
### Inlining the foundation-emails CSS
1. Go back to the documentation, click on the "CSS Version" link and click download. When you unzip this, you'll find a foundation-emails.css file inside. Copy that... and paste it into, how about, the assets/css directory.
2. Make sure the style path is namespaced [check here](mailer#how-we-can-embed-the-logo)
3. apply
```twig
{% apply inky_to_html|inline_css(source('@styles/foundation-emails.css'), source('@styles/email.css')) %}
```
4. change the styling on email.css as per your requirement

## Command to send and email
Send a weekly email to newsletter subscribers.

> One of the fields on User is called `$subscribeToNewsletter`. If this field is set to true for an author - someone that writes content on our site - once a week, via a CRON job, we'll run a command that will email them an update on what they published during the last 7 days.

1. Create a command call `AuthorWeeklyReportSendCommand`  by running `bin/console make:command app:author-weekly-report:send`
2. This command doesn't require any arguments or options. remove them and update the description
```php
protected function configure()
{
    $this
        ->setDescription('Send weekly reports to authors')
    ;
}
```
3. find all users that have this `$subscribeToNewsletter` property set to true in the database.
```php
class UserRepository extends ServiceEntityRepository
{
     /** @return User[] */
    public function findAllSubscribedToNewsletter(): array
    {
        return $this->createQueryBuilder('u')
            ->andWhere('u.subscribeToNewsletter = 1')
            ->getQuery()
            ->getResult();
    }
    }
```
4. Autowiring services into the command
```php
private $userRepository;
public function __construct(UserRepository $userRepository)
{
    parent::__construct(null);
    $this->userRepository = $userRepository;
}
```

```php
protected function execute(InputInterface $input, OutputInterface $output)
{
    $io = new SymfonyStyle($input, $output);
    $authors = $this->userRepository
        ->findAllSubscribedToNewsletter();
    $io->progressStart(count($authors));
    foreach ($authors as $author) {
        $io->progressAdvance();
    }
    $io->progressFinish();
    $io->success('Weekly reports were sent to authors!');
 
    return 0;
}
```

5. Find all the articles this user published
```php
class ArticleRepository extends ServiceEntityRepository
{
    /**
     * @return Article[]
     */
    public function findAllPublishedLastWeekByAuthor(User $author): array
    {
        return $this->createQueryBuilder('a')
            ->andWhere('a.author = :author')
            ->andWhere('a.publishedAt > :week_ago')
            ->setParameter('author', $author)
            ->setParameter('week_ago', new \DateTime('-1 week'))
            ->getQuery()
            ->getResult();
    }
}
```

6. Update the command construct and execute
```php
public function __construct(UserRepository $userRepository, ArticleRepository $articleRepository)
{
    $this->articleRepository = $articleRepository;
}
```

```php
protected function execute(InputInterface $input, OutputInterface $output)
{
    foreach ($authors as $author) {
        $io->progressAdvance();
        $articles = $this->articleRepository
            ->findAllPublishedLastWeekByAuthor($author);
        // Skip authors who do not have published articles for the last week
        if (count($articles) === 0) {
            continue;
        }
    }
}
```
7. Create email template in templates/email/author-weekly-report.html.twig
6. use `TemplatedEmail` to send an email.

### Base Email Template
1. create a base template `emailBase.html.twig` in templates/email directory [Check here](https://gist.github.com/rilwanfit/e719336bba9120a242b650d9c0416fd7)
2. use the base template in actual email template as `{% extends 'email/emailBase.html.twig' %}`
3. place the content inside `{% block content %}` and `{% endblock %}`
4. Loop the list of article
```twig
{% for article in articles %}
<tr>
    <td>{{ loop.index }}</td>
    <td>{{ article.title }}</td>
    <td>{{ article.comments|length }}</td>
</tr>
{% endfor %}
```
- `{{ loop.index }}` to number the list, 1, 2, 3, 4, etc

5. Fix the URL in the email
If you're sending emails from the command line - or rendering templates for any reason that contain paths - you need to help Symfony: you need to tell it what domain to use.
 - Update .env
    ```bash
   SITE_BASE_SCHEME=https
   SITE_BASE_HOST=localhost:8000
   SITE_BASE_URL=$SITE_BASE_SCHEME://$SITE_BASE_HOST
   ```
 - Update services.yaml
 ```yaml
  parameters:
      router.request_context.scheme: '%env(SITE_BASE_SCHEME)%'
      router.request_context.host: '%env(SITE_BASE_HOST)%'
  ```

## Attachment in email
1. Install `composer require knplabs/knp-snappy-bundle`
Snappy makes working with `wkhtmltopdf` pretty easy, but you'll need to make sure it's installed on your system. I installed it on my Mac via brew.
`wkhtmltopdf --version` and `which wkhtmltopdf`
2. Create PDF template
- in the templates/email directory, I'll create a new file called `_report-table.html.twig` 
```twig
<table class="table table-striped">
<tr>
    <th>#</th>
    <th>Title</th>
    <th>Comments</th>
</tr>
{% for article in articles %}
    <tr>
        <td>{{ loop.index }}</td>
        <td>{{ article.title }}</td>
        <td>{{ article.comments|length }}</td>
    </tr>
{% endfor %}
</table>
```
- Update the email template `templates/email/author-weekly-report.html.twig`
```twig
{% block content %}
    <row>
        <columns>
            {{ include('email/_report-table.html.twig') }}
        </columns>
    </row>
{% endblock %}
```
    
- Create in templates/email/, a new file: `author-weekly-report-pdf.html.twig`. To add some basic HTML, I'll use a PhpStorm shortcut that I just learned! Add an exclamation point then hit "tab". Boom! Thanks Victor!
    and then, 
```twig
<head>
    {{ encore_entry_link_tags('app') }}
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-sm-12">
                <h1>Weekly report {{ 'now'|date('Y-m-d') }}</h1>
                {{ include('email/_report-table.html.twig') }}
            </div>
        </div>
    </div>
</body>
```

### Generate PDF
1. Autowiring required services 
```php
class AuthorWeeklyReportSendCommand extends Command
{
    public function __construct(Environment $twig, Pdf $pdf)
    {
```
2. Get HTML from template
```php
$html = $this->twig->render('email/author-weekly-report-pdf.html.twig', [
    'articles' => $articles,
]);
```   
3. Turn that HTML into PDF content.
```php
$pdf = $this->pdf->getOutputFromHtml($html);
```
Behind the scenes, this simple method does a lot: it takes the HTML content, saves it to a temporary file, then executes `wkhtmltopdf` and points it at that file.

4. Adding an Attachment to the email
```php
$email = (new TemplatedEmail())
    ->context([
    ])
    ->attach($pdf, sprintf('weekly-report-%s.pdf', date('Y-m-d')));
```

### Debugging PDF
https://symfonycasts.com/screencast/mailer/pdf-styles#play

## Email service
```php
namespace App\Service;
class Mailer
{
    public function __construct(Symfony\Component\Mailer\MailerInterface $mailer,
        Twig\Environment $twig,
        Knp\Snappy\Pdf $pdf,
        Symfony\WebpackEncoreBundle\Asset\EntrypointLookupInterface $entrypointLookup
    ) {
        $this->mailer = $mailer;
    }
    
    public function sendWelcomeMessage(User $user)
    {
        $email = (new TemplatedEmail())
            ->from(new NamedAddress('alienmailcarrier@example.com', 'The Space Bar'))
            ->to(new NamedAddress($user->getEmail(), $user->getFirstName()))
            ->subject('Welcome to the Space Bar!')
            ->htmlTemplate('email/welcome.html.twig')
            ->context([
                // You can pass whatever data you want
                //'user' => $user,
            ]);
        $this->mailer->send($email);
    }
    
    public function sendAuthorWeeklyReportMessage(User $author, array $articles)
    {
        $html = $this->twig->render('email/author-weekly-report-pdf.html.twig', [
            'articles' => $articles,
        ]);
        
// This is subtle... but it makes sure that the Encore stuff is reset after we render our template. If some other part of our app calls this methods and then renders its own template, Encore will now be ready to do work correctly for them.
        $this->entrypointLookup->reset();

        $pdf = $this->pdf->getOutputFromHtml($html);
        $email = (new TemplatedEmail())
            ->from(new NamedAddress('alienmailcarrier@example.com', 'The Space Bar'))
            ->to(new NamedAddress($author->getEmail(), $author->getFirstName()))
            ->subject('Your weekly report on the Space Bar!')
            ->htmlTemplate('email/author-weekly-report.html.twig')
            ->context([
                'author' => $author,
                'articles' => $articles,
            ])
            ->attach($pdf, sprintf('weekly-report-%s.pdf', date('Y-m-d')));
        $this->mailer->send($email);
    }

}
```

## Unittesting the email
1. `php bin/console make:unit-test MailerTest`

### Emails during Development - Disabling Sending
```yaml
# config/packages/test/swiftmailer.yaml
swiftmailer:
    disable_delivery: true
```

```xml
<!-- config/packages/test/swiftmailer.xml -->
<?xml version="1.0" encoding="UTF-8" ?>
<container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:swiftmailer="http://symfony.com/schema/dic/swiftmailer"
    xsi:schemaLocation="http://symfony.com/schema/dic/services
        http://symfony.com/schema/dic/services/services-1.0.xsd
        http://symfony.com/schema/dic/swiftmailer http://symfony.com/schema/dic/swiftmailer/swiftmailer-1.0.xsd">

    <swiftmailer:config disable-delivery="true" />
</container>
```

```php
// config/packages/test/swiftmailer.php
$container->loadFromExtension('swiftmailer', array(
    'disable_delivery' => "true",
));
```

> which is the default value used by Symfony in the `test` environment

### Emails during development - Sending to a Specified Address(es)
```yaml
delivery_addresses: ['dev@example.com']
```

### how to send email in controller
```php
public function index($name, \Swift_Mailer $mailer)
{
    $message = (new \Swift_Message('Hello Email'))
        ->setFrom('send@example.com')
        ->setTo('recipient@example.com')
        ->setBody(
            $this->renderView(
                'HelloBundle:Hello:email.txt.twig',
                array('name' => $name)
            )
        )
    ;
    $mailer->send($message);

    return $this->render(...);
}
```

### What is the header will be used by Swift Mailer when we use `delivery_addresses`?
X-Swift-To, containing the replaced address, so you can still see who it would have been sent to

### What will be happening for CC and BCC when we use `delivery_addresses`?
this will also stop the email being sent to any CC and BCC addresses set for it. Swift Mailer will add additional 
headers to the email with the overridden addresses in them. These are X-Swift-Cc and X-Swift-Bcc for the CC and BCC addresses respectively.

### Emails during development - Sending to a Specified Address but with Exceptions
```yaml
delivery_addresses: ['dev@example.com']
    delivery_whitelist:
       # all email addresses matching these regexes will be delivered
       # like normal, as well as being sent to dev@example.com
       - '/@specialdomain\.com$/'
       - '/^admin@mydomain\.com$/'
```

```xml
<swiftmailer:config>
        <!-- all email addresses matching these regexes will be delivered
             like normal, as well as being sent to dev@example.com -->
        <swiftmailer:delivery-whitelist-pattern>/@specialdomain\.com$/</swiftmailer:delivery-whitelist-pattern>
        <swiftmailer:delivery-whitelist-pattern>/^admin@mydomain\.com$/</swiftmailer:delivery-whitelist-pattern>
        <swiftmailer:delivery-address>dev@example.com</swiftmailer:delivery-address>
    </swiftmailer:config>
```

> The delivery_whitelist option is ignored unless the delivery_addresses option is defined.

### What is Spool Emails?
Do not send email immediately instead saves the message to somewhere such as a file and another process to read from the spool and take care of sending the emails in the spool.

### What are the Swift Mailer supported spooling methods?
file or memory

### What is the default behavior of SwiftmailerBundle?
it will send the email immediately.

### What could be the problem by sending email immediately?
performance hit of the communication between Swift Mailer and the email transport, which could cause the user to wait for the next page to load while the email is sending

### Spool Using Memory
The email only gets sent if the whole request got executed without any unhandled exception or any errors.

```yaml
spool: { type: memory }
```

### Spool Using Files?
 Symfony creates a folder in the given path for each mail service (e.g. "default" for the default service). This folder will contain files for each email in the spool. So make sure this directory is writable by Symfony (or your webserver/php)!
 
```yaml
spool:
    type: file
    path: '%kernel.root_dir%/spool'
```

### How to send messages in the spool?
```bash
app/console swiftmailer:spool:send --env=prod --message-limit=10 --time-limit=10
``` 

> When you create a message with SwiftMailer, it generates a Swift_Message class. If the swiftmailer service is lazy loaded, it generates instead a proxy class named Swift_Message_<someRandomCharacters>.
  
  If you use the memory spool, this change is transparent and has no impact. But when using the filesystem spool, the message class is serialized in a file with the randomized class name. The problem is that this random class name changes on every cache clear. So if you send a mail and then you clear the cache, the message will not be unserializable.
  
  On the next execution of swiftmailer:spool:send an error will raise because the class Swift_Message_<someRandomCharacters> doesn't exist (anymore).
  
  The solutions are either to use the memory spool or to load the swiftmailer service without the lazy option
