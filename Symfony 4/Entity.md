## Entity

#### 1. Create a `Dinsaur` class

```bash
bin/console make:entity
```

length - integer  

#### 2. Connect to database
- Create `.env.local`
```bash
DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name
```
- Create mysql container https://gist.github.com/rilwanfit/a12e8d3f0dcc2b3b0d7f21c5e836095c
##### How to run migration?
```bash
 bin/console make:migration
 bin/console doctrine:migrations:migrate
```