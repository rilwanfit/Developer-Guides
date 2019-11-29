---
id: easy-admin
title: Easy admin
---

Let's create an admin to manage post, category, tags

1. `composer req admin`

If you want an authentication system, `bin/console make:auth`
This requires following
 - Database connection established
 - User entity created
 - some user entries, (Check here)[security#adding-fixtures]
 
2. Configure entities

Create following entities
 - Post - category_id(ManytoOne), title, content, created_at, published
 - Category - title, slug
 - Tag - name, slug
 
Then add those entities in the `config/package/easy_admin.yaml`