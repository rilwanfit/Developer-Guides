---
id: upgrade-guide
title: Upgrade guide
---

This guide is to upgrade symfony 2.8 version to latest 5

## 1. Update symfony version on composer file
```json
"symfony/symfony": "^5.0",
```

 Run `composer update`

## 2. Use _defaults, autowire & autoconfigure

Add following configuration in `app/config/services.yml`
```yaml
services:
    _defaults:
        autowire: true
        autoconfigure: true
```

> _defaults is a new special keyword that allows you to set default configuration for all services in this file. It's equivalent to adding autowire and autoconfigure keys under every service in this file only. And of course, any config from _defaults can be overridden by a specific service.

Now you can remove all the `autowire` tags from the services defined in this file.

> Setting autowire: true under _defaults is safe to add to an existing project.

## Autowire
autowiring works by looking for a service - or alias - whose id exactly matches the type-hint.

## Autoconfigure
When a service is autoconfigured, it means that Symfony will automatically tag it when possible.

> It does not work for `doctrine.event_subscriber` or `form.type_extension`

## 3. Service Class Name as Service id

in Symfony 3.3, the best practice has changed: a service's id should now be equal to its class name. 

Changing old service id to class name will let us to remove class tag.
ex: 
```yaml
app.markdown_transformer:
        class: AppBundle\Service\MarkdownTransformer
```

to
```yaml
AppBundle\Service\MarkdownTransformer: ~
```

> It is not possible to change all the service ids at once. so, better approach is,
 ## Create Legacy Aliases
create a new `legacy_aliases.yml` and add the old service id and the class name with `@`
```yaml
services:
    app.markdown_transformer: '@AppBundle\Service\MarkdownTransformer'
```
This is called service alias.

now load the file on top of the services.yml

```yaml
imports:
    - { resource: legacy_aliases.yml }
```

Now we can replace all the services id

### How to deal with Multiple Services for the Same Class
```yaml
app.encouraging_message_generator:
    class: AppBundle\Service\MessageGenerator
app.discouraging_message_generator:
    class: AppBundle\Service\MessageGenerator
```
https://symfonycasts.com/screencast/symfony-3.3/problematic-multiservice-classes#play


## 4. Making all Services Private
```yaml
services:
    _defaults:
        public: false
```

## 5. Auto-Registering All Services

```yaml
services:
    _defaults:
    # makes classes in src/AppBundle available to be used as services
    # this creates a service per class whose id is the fully-qualified class name
    AppBundle\:
        resource: '../../src/AppBundle/*'
        # you can exclude directories or files
        # but if a service is unused, it's removed anyway
        exclude: '../../src/AppBundle/{Entity,Repository}'
```

each class in src/AppBundle is available to be used as a service. This means we can reference it as argument in services.yml or type-hint its class in a constructor so that it's autowired. But if you do not reference one of these classes, that service is automatically removed.

### Count number of services
`bin/console debug:container | wc -l`

## 6. Controllers as Services

```yaml
# controllers are imported separately to make sure they're public
# and have a tag that allows actions to type-hint services
AppBundle\Controller\:
    resource: '../../src/AppBundle/Controller'
    public: true
    tags: ['controller.service_arguments']
```

# Possible errors

1. The "framework.trusted_proxies" configuration key has been removed in Symfony 3.3.

- Remove above configuration from `app/config/config.yaml


## Useful
### How to find where the service is used
`git grep app.markdown_transformer`

