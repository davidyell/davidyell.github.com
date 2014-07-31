---
layout: post
title: 'Hack to fix SoftDelete behaviour return in CakePHP'
categories: [cakephp, softdelete, return]
---

### Use case
You have downloaded and installed the [Utils.SoftDelete](https://github.com/cakedc/utils) behaviour, but when you delete an item the return is `false` because the behaviour has stopped the delete and run an update instead.  

You still need to feedback to the user that the item has been deleted succesfully, ie, returning a `true`.

### References
* [https://github.com/CakeDC/utils/issues/19](https://github.com/CakeDC/utils/issues/19)  
* [cakephp-core › callbacks and returning true](https://groups.google.com/forum/?fromgroups=#!topic/cakephp-core/2vIZN8Sq8RE)

### Solution
This is a solution that I've written myself based on the solutions of Ceeram and Augusto Franzoia.  

It will need to go into the `AppModel` of your application.

```php
<?php
public function delete($id = null, $cascade = true) {
    $result = parent::delete($id, $cascade);
    if ($result === false && $this->Behaviors->enabled('SoftDelete')) {
       return $this->field('deleted', array('deleted' => 1));
    }
    return $result;
}
```

### Success

Go make a brew!