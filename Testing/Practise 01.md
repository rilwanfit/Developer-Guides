---
id: practise-01
title: Behat
---
## How would you refactor this code with proper tests?

here is the legacy code https://gist.github.com/rilwanfit/e7693614ce68bdd505111b02b43a9df7

### What could be the problematic area of the code?

1. Global variable `$config`
2. `\Util\Mail` class, which is an external class, and as expected, will send an e-mail
3. Usage of the PDO database connection `$config->db`

### Possible solution

- Refactor code and split logic into an entity (`User`) and a manager (`UserManager`)
- Use integration testing and connect to the database
- Use mocks to create a dummy class that imitates the functionality of the dependent code

1. Write unittest for `User`
    ```php
    private $user;
    public function setUp()
    {
        $this->user = new User([
            'firstName' => 'FirstName',
            'lastName' => 'LastName',
            'email' => 'example@test.com',
            'password' => 'password123'
        ]);
    }
    
    public function testValidInput()
    {
        $this->assertTrue($this->user->isInputValid());
    }
    
    public function testInValidInput()
    {
        $this->user->email = null;
    
        $this->assertFalse($this->user->isInputValid());
    }
    
    public function testCreatedPassword()
    {
        $this->user->createPassword();
    
        $this->assertEquals(sha1('password123' . $this->user->salt), $this->user->password);
    
        $this->assertNotEquals(sha1(null), $this->user->password);
    }
    
    public function testEmptyPassword()
    {
        $this->user->createPassword();
    
        $this->assertNotEquals(sha1(null), $this->user->password);
    }
    ```
    
    Remove all other features from `User` and create `UserManager`
    
2. Write integration test for `UserManager`

Why integration test?
This verifies the interaction with other components (database and email)

If you go down the unit-test-only route, it might be difficult to verify if we can really store/retrieve data from the database. 
In this case, it would be a problem. For e-mails, let's say we are not worried about sending e -mails

```php
class UserManager
        private $db;
        private $email;
        private $config;
        public function __construct(
            \Util\Mail $email,
            \PDO $db,
            $config
        ) {
            $this->email = $email;
            
            $this->db = $db;
            
            $this->config = $config;
        }
        
        private function sendActivationEmail(User $user)
        {
            $this->email->setEmailFrom($this->config->email);
            
            $this->email->setEmailTo($user->email);
            
            $this->email->setTitle('Your account has been activated');
            
            $this->email->setBody("Dear {$user->firstName}\n Your account has been activated\n");
            
            $this->email->send();
        }
        
        public function createUser(User $user)
        {
            if (! $user->isInputValid()) {
                throw new \InvalidArgumentException('Invalid user data');
            }
            
            $user->createPassword();
            
            $sql = "INSERT INTO users(firstname) VALUES (:firstname)";
            
            $statement = $this->db->prepare($sql);
            
            $statement->bindParam(':firstname', $user->firstName);
            
            if ($statement->execute()) {
                $this->userId = $this->db->lastInsertId();
                
                $this->sendActivationEmail($user);
                
                return true;
            
            } else {
                throw new \Exception(implode(':', $statement->errorInfo()));
            }
            
            return false;
        }
```

```php
class UserManagerTest extends TestCase
    {
       public function testCreateUser()
       {
           $db = new \PDO('mysql:host=localhost;port=3306;dbname=test','homestead','secret');
    
           $config = new \stdClass();
           $config->email = 'test@example.com';
           $config->site_url = 'http://example.com';
    
           $user = new User(['firstName' => 'FirtsName', 'lastName' => 'LastName', 'email' => 'user@example.com', 'password' => 'password123']);
           $email = $this->getMock('Mail');
           $userManager = new UserManager($email, $db, $config);
           $this->assertTrue($userManager->createUser($user));
           $this->assertEquals(sha1('password123'.$user->salt), $user->password);
           $this->assertTrue($user->userId > 0);
       }
    }
```