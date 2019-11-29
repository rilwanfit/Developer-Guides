---
id: entity
title: Entity
---

## 1. Create a `Article` class

```bash
bin/console make:entity
```

title - string, 255 NOT NULL
slug - string, 100  NOT NULL
content - text, NULL
publishedAt, datetime, NULL

This command will create an Article entity in src/Entity folder.

## 2. Connect to database
- Create `.env.local`
```bash
DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name
```
- Create mysql container https://gist.github.com/rilwanfit/a12e8d3f0dcc2b3b0d7f21c5e836095c
## 3. Create and run migration?
```bash
 bin/console make:migration
 bin/console doctrine:migrations:migrate
```

## Annotations 
1. The `@ORM\Entity` above the class tells Doctrine that this is an entity that should be mapped to the database

https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/reference/annotations-reference.html

## If you create an Entity manually?

if you want to add doctrine annotations.
- Code->Generate menu - or Command+N on a mac - and select "ORM Class". That's just a shortcut to add the annotations above the class.

### Adding more Fields to an existing entity
`bin/console make:entity` will allow editing an existing entity.

> every time you modify entity, DO NOT forgot to create and run migrations 