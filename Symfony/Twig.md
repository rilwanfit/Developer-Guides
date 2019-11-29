---
id: twig
title: Twig
---


#### Installing twig
`composer require twig`



1.  Extend base controller that has few helper methods
1.  Create a twig template

```php
class ArticleController extends AbstractController
{

    /**  @Route("/{slug}") */
    
    public function homepage(string $slug): Response
    {
        return $this->render('article/show.html.twig', [
            'title' => ucwords(str_replace('-', ' ', $slug)),
            'comments' => ['thanks', 'awesome'],
        ]);
```

```twig
templates/article/show.html.twig
{% extends 'base.html.twig' %}

{% block title %}Read: {{ title }} {% endblock %}

{% block body %}


{{ title }}
Some text......


Comments ({{ comments|length }})
```

## Twig Basics

Syntax: 

1.  {{ }}  say something - it prints
1.  {% %}  do something - can have if, for .. etc
1.  {# #}  comments

[Twig Reference.](https://twig.symfony.com/doc/2.x/#reference)

[screencast](https://knpuniversity.com/screencast/twig)



## Rendering magic

### how use object in twig?
- It is possible to pass an object to twig template
- What is happening when you say `article.title`?
    - Twig first looks to see if the class has a title property
    - It does! But since that property is private, it can't use it. No worries! It then looks for a getTitle() method. And because that exists:
    
    
## Twig extension
https://symfonycasts.com/screencast/symfony-doctrine/twig-extension#play
https://symfonycasts.com/screencast/symfony-doctrine/service-subscriber#play

## Twig Filter
### ago Filter with KnpTimeBundle
https://symfonycasts.com/screencast/symfony-doctrine/ago-filter#play

## How to Inject Variables into all Templates

## How to Render a Template without a custom Controller

## Caching the static Template

