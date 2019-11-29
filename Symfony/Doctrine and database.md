---
id: doctrine-database
title: Doctrine and database
---

## Installing doctrine
1. `compose require doctrine`
2. Configure the database connection
 `DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name`
 
> Create mysql container https://gist.github.com/rilwanfit/a12e8d3f0dcc2b3b0d7f21c5e836095c

3. Create the database
`bin/console doctrine:database:create`

4. Create an entity
[Read here](entity)

## Generate migration
`bin/console make:migration`

- It creates a `src/Migrations/Version*` class  find your code.

## Executing migrations
`bin/console doctrine:migrations:migrate`

- The migration system automatically creates a new table called `migration_versions`
- When you run above command, it looks for entry with same version number as it stated in the part of the class name, if not find a record, then it creates new entry otherwise nothing happens

> Whenever we need to make a database change, we follow this simple two-step process: (1) Generate the migration and (2) run that migration

## Drop the database
`bin/console doctrine:schema:drop --full-database --force`

## how do you save data to the database with Doctrine?
### from a controller action.
1. Create an Entity Object
```php
$article = new Article();
$article->setTitle('Something')
       ->setSlug('something');
```
2. Save the entity
We need `Doctrine\ORM\EntityManagerInterface`
> use `bin/console debug:autowiring` to find required service
```php
public function new(EntityManagerInterface $em)
{
    ...
    $em->persist($article);
    $em->flush();
```

- Always persist and flush
- Persist simply says that you would like to save this article, but Doctrine does not make the INSERT query yet. That happens when you call $em->flush(). Why two separate steps? Well, it gives you a bit more flexibility: you could create ten Article objects, called persist() on each, then flush() just one time at the end. This helps Doctrine optimize saving those ten articles.
- When we call flush(), Doctrine will insert the new row, get the new id, and put that onto the Entity for us.

### How we can verify this worked?
`bin/console doctrine:query:sql "SELECT * FROM article"`

> Doctrine creates snake case table and column names

## how do you query for data with Doctrine?
1. First get the repository for the entity
```php
$repository = $em->getRepository(Article::class);
```

> Better approach to get the Repository via DI, for example
```php
public function homepage(ArticleRepository $repository)
{
    $articles = $repository->findAllPublishedOrderedByNewest();
```

2. Call required method, find(), findAll() etc
```php
/** @var Article $article */
$article = $repository->findOneBy(['slug' => $slug]);
```

- the findAll() methods gives us everything... but it's pretty limited. In fact, it takes zero arguments: you can't control it at all.
    - so, the alternative approach would be
```php
$articles = $repository->findBy([], ['publishedAt' => 'DESC']);
```

> When we query something doctrine return the object.

### If you required to have a custom query, better approach via Repository
For example, If you want to do `WHERE publishedAt IS NOT NULL`

### How entity and repository connected?
```php
/**
 * @ORM\Entity(repositoryClass="App\Repository\ArticleRepository")
 */
```
This annotation can be added to top of the entity class

```php
/**
* @return Article[] Returns an array of Article objects
*/
public function findAllPublishedOrderedByNewest($value)
{
    return $this->createQueryBuilder('a')
        ->andWhere('a.exampleField = :val')
        ->setParameter('val', $value)
        ->orderBy('a.id', 'ASC')
        ->setMaxResults(10)
        ->getQuery()
        ->getResult()
    ;
}
*/
```
- Order of method call is does not matter.

#### `where()` vs `andWhere()`
I recommend andWhere(), because where() will remove any previous where clauses you may have added.

- Use prepared statement, instead of hacking and this prevent SQL injection
```php
->andWhere('a.exampleField = :val')
->setParameter('val', $value)
```
- Example custom queries
```php
->andWhere('a.publishedAt IS NOT NULL')

->andWhere('a.publishedAt IS NULL OR a.publishedAt > NOW()')
```

- Once you're done building your query, you always call `getQuery()` and then, to get the array of Article objects, `getResult()` or `getOneOrNullResult()`

> Bad practise, writing raw sql in doctrine




### What if there is NO records?
it will return NULL, and it should trigger 404 page
```php
if (!$article) {
    throw $this->createNotFoundException(sprintf('No article for slug "%s"', $slug));
}
```

- this method comes from a `trait` that's used by the base `AbstractController`.

### Symfony customize error pages?


### One to many relations with another entity
- you have a `Dinosaurs` and `Enclosure` entities.
- a `Enclosure` class which will hold collection of `Dinosaurs`

1. Create relation in Post
```php
/**
 * @var Enclosure
 * @ORM\ManyToOne(targetEntity="AppBundle\Entity\Enclosure", inversedBy="dinosaurs")
 */
private $enclosure;
```

### Article and Comments relationship
- each `Article` can have many `Comment`and each `Comment` belongs to one `Article`
1. Create `Artcle` Entity [Read here](entity)
2. Create `Comment` Entity with author `autherName` and `content` fields [Read here](entity)
3. Before create a migration, update the `Comment` entity as following
```php
use Gedmo\Timestampable\Traits\TimestampableEntity;
/**
 * @ORM\Entity(repositoryClass="App\Repository\CommentRepository")
 */
class Comment
{
    use TimestampableEntity;
```
That will give us `$createdAt` and `$updatedAt` fields.

4. Generate relationships
- in`Comment` entity, Add `article` with relation type, ManyToOne relation

5. Generate migration `bin/console make:migration` and make sure about the change in the file
> For example, if you're working on multiple branches, then your database may be out-of-sync before you run make:migration. If that happens, the migration file would contain extra changes that you'll want to remove. 
6. Executing migrations `bin/console doctrine:migrations:migrate`

### ManyToOne Versus OneToMany
In reality, ManyToOne and OneToMany do not represent two different types of relationships! Nope, they describe the same, one relationship, just viewed from different sides.

### Saving relations
1. Create Article Fixures
```php

```

## Data Fixtures
### Adding Fixtures
1. `bin/console make:fixtures` This can be used to add dummy users i.e. UserFixtures
    
    NOTE:
    Running above command requires you to have following packages installed
    - `composer require orm-fixtures --dev`
    
    you can use `BaseFixtures` https://gist.github.com/rilwanfit/e10a406109ad67f852064f6d3e61b3e4
    
    - It requires you to have faker library installed
2. Create 10 users
    ```php
    protected function loadData(ObjectManager $manager)
    {
        $this->createMany(10, 'main_users', function ($i) {
            $user = new User();
            $user->setEmail(sprintf('rilwan%d@example.com', $i));
        
            return $user;
        });
    
        $manager->flush();
    }
    ```
3. Load fixtures
    ```bash
    bin/console doctrine:fixtures:load
    ```
    
4. Make sure data is in databse
    ```bash
    bin/console doctrine:query:sql 'SELECT * FROM user'
    ```

### Adding Fixtures with password encoder
```php
    protected function loadData(ObjectManager $manager)
    {
        $this->createMany(10, 'main_users', function ($i) {
            $user = new User();
            $user->setPassword($this->passwordEncoder->encodePassword($user,'rilwan'));
        
            return $user;
        });
    
        $manager->flush();
    }
```
This requires you to inject the `UserPasswordEncoderInterface`

https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html
