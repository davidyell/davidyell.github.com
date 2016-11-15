---
layout: post
title: "Upgrading Session to Flash component in Cake 2.7"
date: 2015-09-02 12:30:00 +0100
comments: true
categories: ['cakephp', 'cakephp2', 'session', 'flash', 'upgrade']
---
## Changes in 2.7
So one of the major changes which was made to CakePHP with the 2.7 release was that the flash messages from the Session component 
have been refactored into the Flash Component, as they were with Cake 3.

[You can view the change in the migration log](http://book.cakephp.org/2.0/en/appendices/2-7-migration-guide.html#sessioncomponent).

## How to update?
If, like me you're using the `setFlash` method for all your controller feedback, you'll need to update all that code.

I used this regular expression which captures the various params and re-orders them for replacement in PHP Storm.

```
// Regex search
this->Session->setFlash\((.*),\s(.*),\s(.*)\);

// Replace
this->Flash->set($1, ['params' => $3]);
```

This will convert stuff like this,

```php
<?php
$this->Session->setFlash('Provider updated successfully', 'NiceAdmin.alert-box', array('class' => 'alert-success'));
// into
$this->Flash->set('Provider updated successfully', ['params' => array('class' => 'alert-success')]);
```

So that's it, you can run this per script if you want to check it, or just across all your controllers.

## All done!
Time for a brew!
