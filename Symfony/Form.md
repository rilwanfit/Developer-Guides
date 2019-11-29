---
id: form
title: Form
---
## Install
`composer require form`

## Creating a form class
- src/Form directory
```php
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
class ArticleFormType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('title')
            ->add('content')
        ;
    }
}
```

### Instantiate a form object
Inside a controller
```php
$form = $this->createForm(ArticleFormType::class);

return $this->render('article_admin/new.html.twig', [
    'articleForm' => $form->createView()
]);
```

### Render the Form
```twig
{{ form_start(articleForm) }}
    {{ form_widget(articleForm) }}
    <button type="submit" class="btn btn-primary">Create!</button>
{{ form_end(articleForm) }}
```

- `form_start` renders the opening form tag
- `form_end` renders the closing form tag
- `form_widget` renders all of the fields

>  `{{ form(form) }}`  renders all fields *and* the form start and end tags

### Handle form submit
  