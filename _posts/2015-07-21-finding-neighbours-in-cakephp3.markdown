---
layout: post
title: "Finding neighbours in CakePHP3"
date: 2015-07-21 20:37
comments: true
categories: ['cakephp3', 'cakephp', 'finders', 'neighbours', 'neighbors']
---
So it seems that in Cake 3 you can no longer use the `find('neighbors')` to find posts which are next to each other according to a defined series.

As I tend to use this for previous and next blog posts when viewing a blog post, I'll note down my technique here for future reference.

## The method

```php
<?php
// src/Model/Table/ExamplesTable.php
/**
 * Find an item from a table by slug, along with it's two adjacent items
 *
 * @param string $slug
 * @return array
 */
public function neighbours($slug)
{
    $current = $this->find()
        ->where(['slug' => $slug])
        ->first();

    $previous = $this->find()
        ->where(['publish_date <' => $current->publish_date->format('Y-m-d')])
        ->order(['publish_date' => 'DESC'])
        ->first();

    $next = $this->find()
        ->where(['publish_date >' => $current->publish_date->format('Y-m-d')])
        ->order(['publish_date' => 'DESC'])
        ->first();

    return [
        'current' => $current,
        'previous' => $previous,
        'next' => $next
    ];
}
```

## The finder

I've not been able to develop a finder for this as a custom find only gets passed a single query object and in order to do a Union between the previous and next you'd need to join two query objects.

There is also the fact that you need to execute the query to return the result of the parent item so that its value can be passed to the query to find the siblings.

If you have any ideas on how to tackle this challenge, please let me know in the comments.