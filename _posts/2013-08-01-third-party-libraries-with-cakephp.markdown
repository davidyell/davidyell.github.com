---
layout: post
title: "Third party libraries with CakePHP"
date: 2013-08-01 15:23
comments: true
categories: [cakephp, libraries, external]
---

I always struggle to include third party library files in my Cake project as they very often do not adhere to any coding standard such as [PSR0](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md).

### Where to put the files?
Third party files should always go into your `Vendor` folder inside your project, if they are not loaded using Composer.

### How to load them?
**Update, 6 March 2015**  
One of the core developers for the framework [advices against using the technique](http://stackoverflow.com/questions/8158129/loading-vendor-files-in-cakephp-2-0/8158269#8158269) I describe in this post. So I am amending the post to reflect this.

####Using require_once
In order to include third party library files, you can use `require_once()`. The only complication is getting the correct path to use.

```php
// To load app/Vendor/DavesLibrary/DavesClass.php
require_once(APP . 'Vendor' . DS . 'DavesLibrary' . DS . 'DavesClass.php');
```

####The older way
Usually you will want to use the `App` class to load your third party classes into your application using `App::import()`. The parameters of this in the api do not match the usage, which apparently is for backwards compatibility.

[The App class in the cookbook](http://book.cakephp.org/2.0/en/core-utility-libraries/app.html#including-files-with-app-import).

The code will look like the following.
```php
<?php
App::import('Vendor', 'TheNameOfMyClass', array('file' => 'DavesLibrary'.DS.'DavesClass.php'));

// DavesLibrary/DavesClass.php
class TheNameOfMyClass { }

// Importing a Vendor library from a plugin? No problem, use plugin.notation
App::import('Vendor', 'DavesPlugin.TheNameOfMyClass', array('file' => 'DavesLibrary'.DS.'DavesClass.php'));
```

### Success
Make a brew!