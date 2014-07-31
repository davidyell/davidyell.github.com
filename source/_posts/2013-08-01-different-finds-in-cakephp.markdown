---
layout: post
title: "Different finds in CakePHP"
date: 2013-08-01 15:11
comments: false
categories: [cakephp]
---
I had a reference for how to do different types of finds in CakePHP using the manual join method and the Containable or Linkable behaviours.

```php
<?php
/*
Using containable

The problem with this is that if Group doens't match, then it will just return a blank array
dimension, because containable is just going to join the arrays in php
*/
$this->User->find('first', array(
    'contain' => array(
        'Group' => array(
            'conditions' => array(
                'Group.id' => $groupId
            )
        )
    ),
    'conditions' => array(
        'User.id' => $userId
    )
);


/*
Using Linkable (https://github.com/lorenzo/linkable)

This will do the same as above, but force a join. So if group doesn't match, you won't even get a result
*/
$this->User->find('first', array(
    'link' => array(
        'Group'
    ),
    'conditions' => array(
        'User.id' => $userId,
        'Group.id' => $groupId
    )
);

/*
Using joins

This does the same as above, but obviously is a bigger find
*/
$this->User->find('first', array(
    'joins' => array(
        array(
            'table' => 'groups',
            'alias' => 'Group',
            'type' => 'LEFT',
            'conditions' => array(
                'Group.id' => $groupId
            )
        )
    ),
    'conditions' => array(
        'User.id' => $userId
    )
);

/*
 * Using Paginate with Containable
 */
// Load the component
$components = array('Paginator');

// Paginate contained data
$this->Paginator->settings = array(
    'contain' => array(
        'Group' => array(
            'conditions' => array(
                'Group.id' => $groupId
            )
        )
    ),
    'conditions' => array(
        'User.id' => $userId
    )
    'limit' => 10
);
$this->Paginator->paginate($this->User);
```