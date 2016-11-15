---
layout: post
title: "How to configure flash messages using Crud plugin"
date: 2015-07-21 21:33:09 +0100
comments: true
categories: ['cakephp', 'cakephp3', 'crud']
---
## Scenario
You're using the [Crud](https://github.com/friendsofcake/crud) plugin you might find that it will use the default flash message elements, so if, like me, you want to use something like [Twitter Boostrap Alerts](http://getbootstrap.com/components/#alerts), you're out of luck.

Never fear, help is at hand.

## Customise flash

```php
<?php
// src/Controller/AppController.php
public function beforeFilter(Event $event)
{
	$this->eventManager()->on('Crud.beforeHandle', function () {
    	$this->Crud->action()->config('messages.success', ['params' => ['class' => 'alert alert-success alert-dismissible']]);
    	$this->Crud->action()->config('messages.error', ['params' => ['class' => 'alert alert-danger alert-dismissible']]);
	});
}
```

I tend to put this, as per the example, in my `AppController` because that way it will only configure the messages for any action which will be handled by the Crud plugin, but across all my controllers.

## Success!

That's it! Go make a brew.