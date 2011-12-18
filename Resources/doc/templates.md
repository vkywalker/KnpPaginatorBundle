# Templates

This document will describe how pagination can be rendered, extended and used in
templates. For now theres only a sliding pagination supported, so all documentation
will reference it.

## Overriding default pagination template

There are few ways to override templates

### Configuration

This way it will override it globally for all default pagination rendering.
Place this parameter in **app/config/parameters.yml**

    knp_paginator.template.pagination: MyBundle:Pagination:pagination.html.twig

Same for sorting link template:

    knp_paginator.template.sortable:   MyBundle:Pagination:sortable.html.twig

### Directly in pagination

``` php
$paginator = $this->get('knp_paginator');
$pagination = $paginator->paginate($target, $page);
$pagination->setTemplate('MyBundle:Pagination:pagination.html.twig');
$pagination->setSortableTemplate('MyBundle:Pagination:sortable.html.twig');
```

or in view

``` html
{% pagination.setTemplate('MyBundle:Pagination:pagination.html.twig') %}
```

### In render method

{{ pagination.render('MyBundle:Pagination:pagination.html.twig')|raw }}

## Other useful parameters

By default when render method is triggered, pagination renders the template
with standard arguments provided:

- pagination parameters, like pages in range, current page and so on..
- route - which is used to generate page, sorting urls
- request query, which contains all GET request parameters
- extra pagination template parameters

Except from pagination parameters, others can be modified or adapted to some
use cases. Usually its possible, you might need setting a route if default is not
matched correctly (because of rendering in sub requests). Or adding additional
query or view parameters.

### Setting a route and query parameters to use for pagination urls

``` php
<?php
$paginator = $this->get('knp_paginator');
$pagination = $paginator->paginate($target, $page);
$pagination->setUsedRoute('blog_articles');
```

In case if route requires additional parameters

``` php
<?php
$pagination->setParam('category', 'news');
```

This would set additional query parameter

### Additional pagination template parameters

If you need extra parameters in pagination template, use:

``` php
<?php
$pagination->setExtraViewParam('style', 'top');
// or an array of extra parameters
$pagination->setExtraViewParams(array(
    'style' => 'bottom',
    'span_class' => 'whatever'
));
```

### You can also change the page range

Default page range is 5 pages in sliding pagination. Doing it in controller:

``` php
<?php
$pagination->setPageRange(7);
```

In template:

``` php
{% pagination.setPageRange(7) %}
```
