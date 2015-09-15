---
layout: post
title: "Connecting to a remote web service"
date: 2015-09-11 12:21:14 +0100
comments: true
categories: ['cakephp', 'cakephp3']
---
## First things first
So the first thing you'll need is the [Webservice plugin](https://github.com/UseMuffin/Webservice).

```bash
composer require muffin/webservice:dev-master && bin/cake plugin load Muffin/Webservice
```

Then be sure to add the configuration for your service to the `config/app.php` file. 
This means you need to add another array key to your config, it should contain something like the following.

```php
return [
	// The rest of your app config
	
	'Webservices' => [
	    'Twitter' => [
	        'className' => 'Muffin\Webservice\Connection',
	        'service' => 'App\Lib\Twitter\Driver\Twitter',
	        'username' => $_ENV['GITHUB_USERNAME'],
	        'password' => $_ENV['GITHUB_PASSWORD'],
	    ]
	]
]; // This is the end of your config/app.php file, so the key sits on the 
   // same array level as 'Session' above
```

I'll talk through the various config options here.

### className
This is a proxy class which will pass method calls through to your own driver class.

### service
This is your driver class where you'll write your code and connect to the service.

### username/password
Any login credentials you need will have to be included.

## Creating your driver class
So you need to create a new driver class, which uses the Webservice plugin as a base. 
For my example, I've created my Twitter driver class in `src/Lib/Twitter/Driver/Twitter.php` which 
extends `Muffin\Webservice\AbstractDriver`, and I've implemented the stub method, which is just `initialize`, 
to setup the client you want to use.

```php
// src/Lib/Twitter/Driver/Twitter.php
namespace App\Lib\Twitter\Driver;

use Cake\Network\Http\Client;
use Muffin\Webservice\AbstractDriver;

class Twitter extends AbstractDriver
{
	public function initialize()
	{
		// Let's use the CakePHP Http client class
		$this->_client = new \Cake\Network\Http\Client();
	}
	
	// etc
```

**There is a gotcha here!**  
You will need to implement some methods from `Cake\Database\Connection` into 
your driver class so that DebugKit can interact with your driver. These methods 
can be copied and pasted from the Connection class.

* `configName()`
* `logQueries()`
* `logger()`

## Creating your method
So assuming we want to get some tweets, we can create a method for that inside 
our driver class.

```php
	public function tweets()
	{
		// Do some setup for Twitter
		return $this->_client->post($url, $dataArray);
	}
```
This will use the CakePHP Http Client to post to a Twitter url and return the response.

## Using your new driver
So now we need to actually get the data to play with. In our controller we need to 
instantiate the connection and call our new method.

```php
// ExamplesController.php
	public function getTweets()
	{
		$connection = ConnectionManager::get('Twitter');
		$tweets = $connection->tweets();
		$this->set('tweets', $tweets);
	}
```

You must make sure that the name of the connection you give to ConnectionManager matches 
the name you used in your `app.php` config setup.

## That's it!
Job done! Have a brew.
