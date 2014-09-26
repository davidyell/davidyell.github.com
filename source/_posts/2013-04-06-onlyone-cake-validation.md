---
layout: post
title: 'Only One validation for CakePHP'
comments: true
categories: [cakephp, validation, onlyone]
date: 2013-04-06 09:43:25
---

### Use case
You want to set a default for a collection of items and you need to validate that there is only one default set in the collection.

### Setup

I tend to use a `bool NOT NULL DEFAULT 0` field for this in the database. Then in my model I'll implement a custom validation function.

```php
<?php
public $validate = array(
        'default' => array( // Name of the field
            'one' => array( // The name of the rule
                'rule' => 'onlyOne', // My custom validation
                'message' => 'Only one can be default. A default already exists',
                'required' => false
            )
        )
);

/**
 * Checks to ensure that only one record has the default flag
 * @param array $check An array containing {field}=>{value}
 * @return bool
 */
    public function onlyOne($check) {
        // If turning off default, we don't mind
        if ($check['default'] == 0) {
            return true;
        }

        // Find the current default
        $default = $this->find('first', array(
            'conditions' => array(
                'default' => 1,
            ),
            'fields' => array('id', 'name')
            ));

        // No default exists
        if (!$default) {
            return true;
        }

        // If we are updating the current default
        if ($default['Model']['id'] == $this->data['Model']['id']) {
            return true;
        }

        return false;
    }
    
```

### Success

Go make a brew!
