---
id: security
title: Security
---
## Authentication
methodologies for authentication

- Login form
- Token based API
- Two-Factor
- Single sign-on-server
- and more... name it.

## How to create an authentication system with login form.
why its important? It will allow user to login :)
Following step will allow to create an authentication system
### 1. Create a `User` class
There is a maker bundle to help us creating a `User` class

```bash
bin/console make:user
```

NOTE: 
Running above command requires you to have following packages installed
- `composer require maker --dev`
- `composer require security --dev`
- `composer require orm --dev`

#### What `make:user` command does?
1. creates `User` Entity which implement `UserInterface`
2. creates a `UserRepository` if we want to store the data
3. creates `security.yaml` inside config/packages

- `getUsername()` This method should just return a visual identifier that represents the user, And actually, this method is only used by Symfony to display who is currently logged in on the web debug toolbar
- `getRoles()` This method related to user permission
- `getPassword(), getSalt() and eraseCredentials()`.  These are only needed if your app is responsible for storing and checking user passwords.
- `security.yaml` file updated with new provider.
```yaml
app_user_provider:
    entity:
        class: App\Entity\User
        property: username
```

### 2. Connect to database
- Create `.env.local`
```bash
DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name
```
- Create mysql container https://gist.github.com/rilwanfit/a12e8d3f0dcc2b3b0d7f21c5e836095c
#### How to run migration?
```bash
 bin/console make:migration
 bin/console doctrine:migrations:migrate
```

### Add some fixtures
[Read more](doctrine-database#data-fixtures)

### 3. Login - the form
1. Create SecurityController
    ```bash
    bin/console make:controller
    ```
    Running above command requires you to have following packages installed
    - `composer require twig`
    
2. Update the action
    ```php
    /**
     * @Route("/login", name="app_login")
     */
    public function index()
    {
        return $this->render('security/login.html.twig');
    }
    ```
3. Add template
    ```twig
    {% extends 'base.html.twig' %}
    
    {% block title %}Login!{% endblock %}
    
    {% block body %}
        <h1>Login to the App!</h1>
    {% endblock %}
    ```
4. Fill the gab https://symfony.com/doc/current/security/form_login_setup.html

### 4. Login - firewall and authenticator/Authentication listeners

## What are Authentication Listeners / Authenticators?
At the beginning of every request, Symfony calls a set of "authentication listeners", or "authenticators".
The job of each authenticator is to look at the request to see if there is any authentication info on it - like a submitted email & password or maybe an API token that's stored on a header. If an authenticator finds some info, it then tries to use it to find the user, check the password if there is one, and log in the user!

## Firewall
- The whole job of the firewall is to authenticate you: to figure out who you are
- The name of the firewall is totally meaningless.
- When you define more than one firewall it tries to match the request in order and fallback to the next firewall if needed

### anonymous: true ?
This allows anonymous requests to pass through this firewall so that users can access your public pages, without needing to login. Even if you want to require authentication on every page of your site, keep this. There's a different place `access_control` - where we can do this better:

1. Create Authentication
    ```bash
   bin/console make:auth
    ```
    What is does?
    If you choose, Empty authenticator and provide name as LoginFormAuthenticator
     - Creates `src/Security/LoginFormAuthenticator.php` 
 2. Change Instead of extends `AbstractGuardAuthenticator` use extends `AbtractFormLoginAuthenticator`
 Thanks to this, we no longer need `onAuthenticationFailure(), start() or supportsRememberMe()` they're all handled for us:
    
 4. Implement required methods on authenticator
    - `support`: Return true if user submit login from, and it POST to /login endpoint
    ```php
    return $request->attributes->get('_route') === 'app_login'
        && $request->isMethod('POST');
    ```
    - `getCredentials`: Return authentication credentials off of the request
    ```php
    return [
        'email' => $request->request->get('email'),
        'password' => $request->request->get('password'),
    ];
    ```
    - `getUser`: use the credentials to return a User object or null if the user isn't found
    ```php
    return $this->userRepository->findOneBy(['email' => $credentials['email']]);
    ```
    
    This requires you to inject the UserRepository 
    
    - `checkCredentials`: Check to see if the user's password is correct or any other last security checks.
    - `onAuthenticationSuccess`:  Redirect to home page on success
    ```php
    return new RedirectResponse($this->router->generate('app_homepage'));
    ```
    This requires you to inject the `RouterInterface` 
    
    What if you want to redirect to previously visited page instead of homepage?
    
    To do this, 
    1. at the top of your authenticator, use a trait `TargetPathTrait`
    2. add following code:
    ```php
    if ($targetPath = $this->getTargetPath($request->getSession(), $providerKey)) {
        return new RedirectResponse($targetPath);
    }
    ```
    
    - `getLoginUrl`: Redirect to login page in failure
    ```php
    return $this->router->generate('app_login');
    ```
 3. Activate it, its done by the make:auth command
 ```yaml
firewalls:
    main:
        guard:
            authenticators:
                - App\Security\LoginFormAuthenticator
```
> The important part is that, as soon as we add this, at the beginning of every request, Symfony will call the supports() method on our authenticator.

> Is that user authentication info is stored to the session. At the beginning of every request, that info is loaded from the session and we're logged in. Cool!

### How Authentication Errors are Stored
In reality, when authentication fails, this `AbstractFormLoginAuthenticator::onAuthenticationFailure() `method is called. when authentication fails, internally, it's because something threw an `AuthenticationException`, which is passed to this method. And, ah: this method stores that exception onto a special key in the session! Then, back in the controller, the `lastAuthenticationError()` method is just a shortcut to read that key off of the session!

### How to customize error messages
Method 1: Throw CustomUserMessageAuthenticationException with custom message in authenticator

Method 2: Use translations
- in your `translations/` directory, create a `security.en.yaml` file,

The name security comes from the domain part `{{ error.messageKey|trans(error.messageData, 'security') }}`
- add following content
    ```yaml
    "Username could not be found.": "Oh no! It doesn't look like that email exists!"
    ```
- clear cache `bin/console cache:clear`

### How to fill the last email
1. Make sure that the email input has value set as `value="{{ last_username }}"`
2. Store last username in the session. `LoginFormAuthenticator::getCredentials` 
    ```php
    $request->getSession()->set(
        Security::LAST_USERNAME,
        $credentials['email']
    );
    ```
  
### 5. Logout
Step 1. Create new route in SecurityController
    ```php
    /**
     * @Route("/logout", name="app_logout")
     */
    public function logout()
    {
    ```
Step 2. Configure the path
    ```yaml
    firewalls:
        main:
            logout:
                path: app_logout
    ```

### 6. CSRF 
Step 1: Add an `<input type="hidden">` field to our form.
```twig
<input type="hidden" name="_csrf_token"
       value="{{ csrf_token('authenticate') }}" >
```
Step 2: Verify the token in authenticator
    
- Return token from the request in `getCredentials`
```php
$credentials = [
    'csrf_token' => $request->request->get('_csrf_token')
```
- Check the token in `getUser`
```php
$token = new CsrfToken('authenticate', $credentials['csrf_token']);
if (!$this->csrfTokenManager->isTokenValid($token)) {
    throw new InvalidCsrfTokenException();
}
```
This requires you to inject the `CsrfTokenManagerInterface`

### 6. Remember me
Step 1: make sure that your checkbox has no value and that its name is `_remember_me`:
```twig
<div class="checkbox mb-3">
    <label>
        <input type="checkbox" name="_remember_me"> Remember me
    </label>
</div>
```

Step 2. Configure it
```yaml
firewalls:
    main:
        remember_me:
            secret:   '%kernel.secret%'
            lifetime: 2592000 # 30 days in seconds, default to 1 year
```

### 7. Use password
Step 1: make sure user entity has `password` field, getter and setter
Step 2: The `getSalt` not always necessary (not needed when using the "bcrypt" or argon)
Step 3: Configure the encoder
```yaml
security:
    encoders:
        App\Entity\User:
            algorithm: bcrypt
```
Step 4: Update fixtures if needed
Step 5: Checking password in authenticator
```php
public function checkCredentials($credentials, UserInterface $user)
{
    return $this->passwordEncoder->isPasswordValid($user, $credentials['password']);
}
```
This requires you to inject the `UserPasswordEncoderInterface`


## What is UserProvider?
a class that helps with a few things, 
- like reloading the User data from the session
- like rem ember me https://symfony.com/doc/master/security/remember_me.html
- impersonation. https://symfony.com/doc/master/security/impersonating_user.html

## Do we need to create UserProvider class when we use User Entity?
No, Fortunately, the make:user command already configured one for you in your security.yaml file under the providers key.

> If your User class is an entity, you don't need to do anything else. But if your class is not an entity, then make:user will also have generated a UserProvider class that you need to finish.

https://symfony.com/doc/master/security/user_provider.html

## How to create an authentication system with basic auth

### How to access the endpoint which under the basic auth
`http://user:pass@localhost`

This will set the username and password for the basic auth request

### Functional tests

To setup functional tests [Check here](automated-tests#functional-test)
```php
$client = static::createClient([],[
    'PHP_AUTH_USER' => 'user',
    'PHP_AUTH_PW'   => 'pass',
]);

$clawler = $client->request(
    Request::METHOD_POST,
    '/'
);
```

### How to create an authentication system with API token
An API token authentication system has two, quite unrelated parts. The first is how your app processes an existing API token and logs in the user. The second is how those API tokens are created and distributed.

1. Create `ApiToken` entity
    ```bash
    bin/console make:entity ApiToken
    ``` 
    - token, string, not null
    - expiresAt, datetime
    - user, ManytoOne relation to `User`, not null
2. Update the `TokenApi`
    ```php
    public function __construct(User $user)
    {
        $this->token = bin2hex(random_bytes(60));
        $this->user = $user;
        $this->expiresAt = new \DateTime('+1 hour');
    }
    ```
    NOTE: our token class is now immutable, which wins us major hipster points. Immutable just means that, once it's instantiated, this object's data can never be changed. Some developers think that making immutable objects like this is super important. I don't fully agree with that. But, it definitely makes sense to be thoughtful about your entity classes. Sometimes having setter methods makes sense. But sometimes, it makes more sense to setup some things in the constructor and remove the setter methods if you don't need them.
3. Run migrations
    ```bash
     bin/console make:migration
     bin/console doctrine:migrations:migrate
    ```
4. Update fixtures
    ```php
    $this->createMany(10, 'main_users', function ($i) use ($manager) {
        $user = new User();
        $user->setEmail(sprintf('rilwan%d@example.com', $i));
        $user->setPassword('rilwan');

        $apiToken1 = new ApiToken($user);
        $manager->persist($apiToken1);

        return $user;
    });
    ```
5. Create ApiTokenAuthenticator
   ```bash
    bin/console make:auth
   ```
What is does?
If you choose, Empty authenticator and provide name as `ApiTokenAuthenticator`
- Creates `src/Security/ApiTokenAuthenticator.php` 
2. Change Instead of extends `AbstractGuardAuthenticator` use extends `AbtractFormLoginAuthenticator`
Thanks to this, we no longer need `onAuthenticationFailure(), start() or supportsRememberMe()` they're all handled for us:
   
4. Implement required methods on authenticator
   - `support`: Return true if user submit login from, and it POST to /login endpoint
   ```php
   return $request->attributes->get('_route') === 'app_login'
       && $request->isMethod('POST');
   ```
   - `getCredentials`: Return authentication credentials off of the request
   ```php
   return [
       'email' => $request->request->get('email'),
       'password' => $request->request->get('password'),
   ];
   ```
   - `getUser`: use the credentials to return a User object or null if the user isn't found
   ```php
   return $this->userRepository->findOneBy(['email' => $credentials['email']]);
   ```
   
   This requires you to inject the UserRepository 
   
   - `checkCredentials`: Check to see if the user's password is correct or any other last security checks.
   - `onAuthenticationSuccess`:  Redirect to home page on success
   ```php
   return new RedirectResponse($this->router->generate('app_homepage'));
   ```
   This requires you to inject the `RouterInterface` 
   
   What if you want to redirect to previously visited page instead of homepage?
   
   To do this, 
   1. at the top of your authenticator, use a trait `TargetPathTrait`
   2. add following code:
   ```php
   if ($targetPath = $this->getTargetPath($request->getSession(), $providerKey)) {
       return new RedirectResponse($targetPath);
   }
   ```
   
   - `getLoginUrl`: Redirect to login page in failure
   ```php
   return $this->router->generate('app_login');
   ```
3. Activate it, its done by the make:auth command
```yaml
firewalls:
   main:
       guard:
           authenticators:
               - App\Security\LoginFormAuthenticator
```
NOTE: The important part is that, as soon as we add this, at the beginning of every request, Symfony will call the supports() method on our authenticator.

NOTE: is that user authentication info is stored to the session. At the beginning of every request, that info is loaded from the session and we're logged in. Cool!

https://symfony.com/doc/3.4/security/api_key_authentication.html


## Authorization
deciding whether or not a user should have access to something. it involves the following

- roles
- access controls
- denying access in your controller

### How to restrict access to some section of the application (Autherization)
There are two ways
first, `access_control` and second, denying access in your controller.

### access_control
```yaml
security:
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
```
    - The path is a regular expression
    - The roles, every user at least has this one role: ROLE_USER
    
### Denying access in controller
```php
$this->denyAccessUnlessGranted('ROLE_USER');
```

or use `@IsGranted("ROLE_ADMIN")` annotation

this requires you to have following packages
- `composer require annotations`

Annotation can be place in class level or action level

### Checking role in twig
```twig
% if is_granted('ROLE_USER') %}
```

- Checking for `ROLE_USER` means that user is logged in regardless of his role
- Checking for `IS_AUTHENTICATED_FULLY` also the same meaning as above

### Requiring login on every page
following configuration will be allowing to require login on every page.
```yaml
security:
    access_control:
        # definitely allow /login to be accessible anonymously
        - { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        # if you wanted to force EVERY URL to be protected
        - { path: ^/, roles: IS_AUTHENTICATED_FULLY }
```

It's possible to use `IS_AUTHENTICATED_FULLY` or `IS_AUTHENTICATED_REMEMBERED`

`IS_AUTHENTICATED_FULLY` means that you have authenticated during this session.

 