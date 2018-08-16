---
layout: post
title: "Adding data attributes to select elements"
date: 2018-08-16T10:08:35+01:00
comments: true
categories: ['cakephp', 'cakephp3', 'select', 'collection', 'attributes']
---

## Use case  
You've loaded a collection of entities, and would like to create a select element out of it without having to do an extra `find('list')` in your controller.

## Organising your data  
There are two approaches here depending on what you'd like to achieve.


### Just an option value and text  
Firstly, let's look at just taking two fields from the entities and using them. This uses the `combine` method from the [Collections class](https://book.cakephp.org/3.0/en/core-libraries/collections.html#Cake\Collection\Collection::combine).

```php
<?php
$optionsData = $entities->combine('id', 'name');

echo $this->Form->control('example', ['options' => $optionsData]);
```

Output would be something like the following

```html
<select name="example" id="example">
    <option value="1">First</option>
    <option value="2">Second</option>
</select>
```

### Adding data attributes  
Secondly we might want to add some more data to the DOM, perhaps for Javascript to interact with. This uses the `map` method from the [Collections class](https://book.cakephp.org/3.0/en/core-libraries/collections.html#Cake\Collection\Collection::map).

```php
<?php
$optionsData = $entities->map(function ($value, $key) {
    return [
        'value' => $value->id,
        'text' => $value->name,
        'data-created' => $value->created
    ];
});

echo $this->Form->control('example', ['options' => $optionsData]);
```

Output would be something like the following

```html
<select name="example" id="example">
    <option value="1" data-created="2018-08-16 11:15:10">First</option>
    <option value="2" data-created="2018-08-16 11:16:22">Second</option>
</select>
```

## Make a brew
That's it, make a brew.