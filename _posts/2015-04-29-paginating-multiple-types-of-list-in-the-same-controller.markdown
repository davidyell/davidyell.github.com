---
layout: post
title: "Paginating multiple types of list in the same controller"
date: 2015-04-29 11:57:10 +0100
comments: true
categories: ['cakephp', 'cakephp3', 'pagination']
---
## What a complicated title
Okay, agreed. Perhaps an example would be better. Imagine you have a `ContentsController` which you use in your application to render lots of different types of content. You might have `ContentTypes` such as News, Guides, Articles, Pages, etc. so there is no point creating lots of controllers, when your `ContentsController::index()` method could just output the same thing with just different data.

This post is about how you deal with those lists in your routing and how to paginate them so that you can preserve your urls.

## Routing
So the first thing to do is to setup your routing so that you can keep your content separate.

```php
<?php
Router::scope('/', function ($routes) {
    $routes->connect('/guides/:slug', ['controller' => 'Contents', 'action' => 'view', 'type' => 'Guides'], ['slug' => '[a-z-]+', 'pass' => ['type', 'slug']]);
    $routes->connect('/guides', ['controller' => 'Contents', 'action' => 'index', 'type' => 'Guides'], ['pass' => ['type']]);
    
    $routes->connect('/news/:slug', ['controller' => 'Contents', 'action' => 'view', 'type' => 'News'], ['slug' => '[a-z-]+', 'pass' => ['type', 'slug']]);
    $routes->connect('/news', ['controller' => 'Contents', 'action' => 'index', 'type' => 'News'], ['pass' => ['type']]);
});
```

This now means that we can access all our different types of content on different urls so that it makes sense to the user.

## Creating our links
So now we need to point users to our content.

```php
<?php
// Link to the index
echo $this->Html->link('Guides', ['controller' => 'Contents', 'action' => 'index', 'type' => 'Guides']);

// Link to a single item
echo $this->Html->link($guide->title, ['controller' => 'Contents', 'action' => 'view', 'type' => 'Guides', 'slug' => $guide->slug]);
```

## Paginating the index
As we are using a single view to paginate many different lists we need to tell the Pagination helper how to form the url properly.

```php
<?php
$this->Paginator->options([
    'url' => ['controller' => 'Contents', 'action' => 'index', 'type' => $type->name]
]);
```

This will now pass the `type` correctly in all our pagination links, which means that it'll match the routing correctly and you should end up with nicely paginated urls which match your routing such as `/guides?page=2`.

## Getting the list
So you'll need to process the `type` when it's passed to your controller, so be sure to include it in your controllers method signature.

```php
<?php
class ContentsController extends AppController {
	public function index($type)
	{
		// Lookup the data by type
		// Perhaps with a custom finder
	}
}
```

## Make a brew
It really is that simple!
