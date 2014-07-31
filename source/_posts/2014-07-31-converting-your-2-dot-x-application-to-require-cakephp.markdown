---
layout: post
title: "Requiring CakePHP 2.x using Composer"
date: 2014-07-31 15:00:34 +0100
comments: true
categories: ['cakephp', 'composer', '2.x']
---

### Scenario
You have started your project using one of the 2.x downloads. Now you are using Composer to pull in your plugins, but you are still having to commit the framework to your repo.

So it's time to remove your CakePHP lib and start requiring it as a dependancy instead.

I will assume you are fimiliar with [Composer](http://getcomposer.org/).

### Solution
So it's actually pretty straight forward. For the sake of this example, we will assume that you have a standard layout in your current project.

If you are not familiar with the current layout, [take a quick look at the 2.5.3 tag](https://github.com/cakephp/cakephp/tree/2.5.3) to check the folders.

To see what we are aiming at, [check out the Friends of Cake app-template](https://github.com/FriendsOfCake/app-template).

If you don't already have a `composer.json` file, then you'll want to create that. `composer init` inside your project folder.

#### Adding the framework
Firstly we want to let Composer know that you want to require the framework, so let's update the `composer.json` to add the following to your `require` section.

```json
"cakephp/cakephp": "2.5.3"
```

You can pick your version here if you want to lock in the version, which I'd recommend. Or you could specify another version, such as `2.5.*`. [Check the Composer docs for more on versions](https://getcomposer.org/doc/01-basic-usage.md#package-versions).

When you `composer update` now it should download the framework for you into `vendors/cakephp/cakephp`.

#### Pointing to the correct core
The next task is to tell CakePHP that we have a new place for it to find the core. There are only really a few files you need to update.

* `app/webroot/index.php`
* `app/Console/cake.php`

**app/webroot/index.php**  
You'll want to update the `ROOT`, `APP_DIR`, `TMP` and the `CAKE_CORE_INCLUDE_PATH` constants.

```php
define('ROOT', dirname(dirname(__FILE__)));
define('APP_DIR', 'app'); // I define this as a string because why not right?
define('TMP', ROOT . DS . 'tmp' . DS);
define('CAKE_CORE_INCLUDE_PATH', ROOT . DS . 'vendor' . DS . 'cakephp' . DS . 'cakephp' . DS . 'lib');
```

**app/Console/cake.php**  
So that the console commands can still find the right lib, we need to update the include path here too.

```php
ini_set('include_path', '..' . $ds . 'vendor' . $ds . 'cakephp' . $ds . 'cakephp' . $ds . 'lib' . PATH_SEPARATOR . ini_get('include_path'));
```

As you are running your shell tasks from within `app` we can use a relative path.

#### Testing
To check that the new setup is working you can run a `Console/cake` to check what the `core` value is.

```bash
$ cd app
$ Console/cake

Welcome to CakePHP v2.5.3 Console
---------------------------------------------------------------
App : app
Path: /Users/david/Sites/example/app/
---------------------------------------------------------------
Current Paths:

 -app: app
 -working: /Users/david/Sites/example/app
 -root: /Users/david/Sites/example
 -core: /Users/david/Sites/example/vendor/cakephp/cakephp/lib
```

### Done!
Feel free to delete your old `lib` folder as you now don't need it. Have fun updating your applications version of CakePHP using Composer.