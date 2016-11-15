---
layout: post
title: "Pagination inside a View Cell"
date: 2016-11-15T16:36:20+00:00
comments: true
categories: ['cakephp', 'cakephp3', 'pagination', 'view cells']
---

## What?
So I encountered a use-case where I wanted to be able to render a complex table simply whilst allowing it to be easily
integrated with other things in the template.

The obvious choice here was to use a [View Cell](http://book.cakephp.org/3.0/en/views/cells.html) so that it would be neatly enclosed.

However this meant that I'd need to load the data and paginate it inside the View Cell.

## How?
Working in your `ExampleCell.php` file, in your method you can instanciate a new controller, with the request and attach 
the Paginator.

```php
<?php
// In /src/View/Cell/ExampleCell.php

use Cake\Controller\Component\PaginatorComponent;
use Cake\Controller\ComponentRegistry;
use Cake\Controller\Controller;

public function display()
{
    $controller = new Controller($this->request);
    $componentRegistry = new ComponentRegistry();
    $componentRegistry->setController($controller);
    $paginator = new PaginatorComponent($componentRegistry);
    
    $this->loadModel('Examples');
    
    $query = $paginator->paginate($this->Examples);
    
    $this->set('examples', $query);
}
```

This code will build a new controller instance using the request sent to the View Cell. Load the Paginator component and then paginate
 the query. Because the request is sent, the Paginator can already read from the query strings in the url, so all the pagination happens automatically.
 
## Implementing the View Cell
In your template you can simply call the Cell as you would normally.

```php
<?= $this->cell('Example');?>
```

The View Cell gets the request anyway, so there is no need to pass it in via the options.

## Make a brew!
That's it. You can now have a paginated View Cell of data right in your template!

## Cleverclogs
If you're feeling clever, perhaps you can call the Cell using Ajax to update it right inside the page!