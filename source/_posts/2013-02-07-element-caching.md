---
layout: post
title: 'How to cache an element in CakePHP'
categories: [cakephp, cache, element]
---

So this is something that I do on a regular basis and can never quite remember the correct order of the options. Oh sure, it's in the [book](http://book.cakephp.org) but I always struggle to find the bit I need.  

> The url is here, [http://book.cakephp.org/2.0/en/views.html#caching-elements](http://book.cakephp.org/2.0/en/views.html#caching-elements)  

Also worth including some code here to save me having to click a link.  

```php
<?php 
echo $this->element(
    'helpbox',
    array('var' => $var),
    array('cache' => array('key' => 'first_use', 'config' => 'view_long')
);
```

**Params**  
Name of element, array of data then options. Ref, [http://api.cakephp.org/class/view#method-Viewelement](http://api.cakephp.org/class/view#method-Viewelement)  
**NB**  
It's also worth remebering that if you are caching something it will be cached in that state from the moment it's called. If you something is dynamic and you are caching it, *always* use a proper `key` for it in the options array! Or use `$this->here` to use the current url slug as the key.
