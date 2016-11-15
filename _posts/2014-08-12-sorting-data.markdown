---
layout: post
title: "Sorting multi-dimension model data"
date: 2014-08-12 15:42:10 +0100
comments: true
categories: [cakephp, php, data manipulation]
---
### Scenario
So often you have a large collection of collated data, probably with some calculated fields and you want to sort one of the dimensions by a certain value. Pretty hard using regular array methods, unless you start slicing arrays out of their dimension, sort them and inject them back in. We don't want to do that because it's fiddly.

### Solution
So the solution I like to use is to use [usort()](http://php.net/manual/en/function.usort.php). Which is a handy function allowing you to sort an array using your own function. Perfect if you want to sort a related models data.

### Example
So for the example we'll assume that we have a `League` model which `hasMany` `LeaguesUser`. We want to count the number of points a user has and order the data accordingly.

When returned from a Cake `find()` we'll end up with related data in dimensions.
```php
<?php
array (size=2)
  'League' => 
    array (size=9)
      'id' => string '1' (length=1)
      'name' => string 'Examples!' (length=9)
      'slug' => string 'examples' (length=9)
      'description' => string 'The league for people who like examples' (length=45)
      'cover' => string '1000x1000.jpg' (length=13)
      'image_dir' => string '1' (length=1)
      'join_code' => string 'da39a3ee5e6b4b0d3255bfef95601890afd80709' (length=40)
      'created' => string '2014-08-04 10:45:45' (length=19)
      'modified' => string '2014-08-11 14:54:34' (length=19)
  'LeaguesUser' => 
    array (size=2)
      0 => 
        array (size=5)
          'id' => string '1' (length=1)
          'league_id' => string '1' (length=1)
          'user_id' => string '2' (length=1)
          'admin' => boolean true
          'User' => 
            array (size=5)
              'id' => string '2' (length=1)
              'email' => string 'test@example.com' (length=16)
              'username' => string 'testuser' (length=8)
              'predictions' => int 2
              'correct' => int 0
      1 => 
        array (size=5)
          'id' => string '2' (length=1)
          'league_id' => string '1' (length=1)
          'user_id' => string '1' (length=1)
          'admin' => boolean false
          'User' => 
            array (size=5)
              'id' => string '1' (length=1)
              'email' => string 'testuser1@example.com' (length=20)
              'username' => string 'testuser1' (length=8)
              'predictions' => int 2
              'correct' => int 1
```

So let's sort that `LeaguesUser['User']` dimension by the number of correct predicitons.

Firstly, we'll want to create a new `private function sortByCorrect($a, $b)` in our controller. Then we just need to sort using it in our currenct controller method.

The important thing to note is that the callable function passed to `usort()` is an array containing the current controller as the first item. `usort($data, [$this, 'callableFunction'])` without including `$this` you'll get an error.

```php
<?php
public function view($slug) {
	$league = $this->League->find('first', [
		'contain' => [
			'LeaguesUser' => [
				'User' => [
					'fields' => ['id', 'email', 'username', 'correct', 'predicitons']
				]
			]
		],
		'conditions' => [
			'League.slug' => $slug
		]
	]);
	
	usort($league, [$this, 'sortByCorrect']);
	
	$this->set('league', $league);
}

private function sortByCorrect($a, $b) {
	if ($a['User']['correct'] < $b['User']['correct']) {
		return 1;
	} elseif ($a['User']['correct'] == $b['User']['correct']) {
		return 0;
	} else {
		return -1;
	}
}

// Rest of controller
```

### Done
Go make a brew! Your work here is done.