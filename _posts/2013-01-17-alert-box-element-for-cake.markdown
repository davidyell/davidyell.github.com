---
layout: post
title: "Twitter Bootstrap alert-box element for CakePHP"
description: "An element which can be used for displaying flash messages with appropriate colours."
comments: true
categories: [twitter-bootstrap, cakephp]
date: 2013-01-17 09:43:25
---

So I like the [Twitter Bootstrap alerts](http://twitter.github.com/bootstrap/components.html#alerts) and I wanted a way to use them in my [CakePHP](http://cakephp.org/) projects when controller actions return flash messages.

### Element  
**app/View/Elements/alert-box.ctp**  
```php
<div class="alert <?php echo $class;?>">
    <?php echo $message; ?>
    <a href="#" class="close" onclick="$(this).parent().fadeOut();return false;">&times;</a>
</div>
```

### Usage
The element is used from the controller by appending the `element` and `class` to the `$this->element()` call.  
**Success**  
```php
<?php $this->Session->setFlash(__('The article has been saved'), 'alert-box', array('class'=>'alert-success')); ?>
```

**Failed**  
```php
<?php $this->Session->setFlash(__('The article could not be saved. Please, try again.'), 'alert-box', array('class'=>'alert-error')); ?>
```