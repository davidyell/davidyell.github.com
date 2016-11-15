---
layout: post
title: "Dynamic validation in CakePHP"
date: 2013-09-12 12:53
comments: true
categories: [cakephp, validation, model]
---

### The scenario
You have two different forms which both submit data for the same model. Each of these forms has different fields, but both need to be validated before the data can be saved.

If you setup your model validation normally, both forms will fail to validate as they will be missing fields.

### Solution
The method that I use to solve this is very simple. You can create two validation arrays and dynamically merge them together as and when you need them.

```php
<?php
// Model/Post.php

/**
 * Setup the default rules here, these rules should be common to both forms 
 *
 * @var array $validate
 */
	public $validate = array(
		'title' => array(
			'one' => array(
				'rule' => 'notEmpty',
				'message' => 'Please enter a title',
				'required' => true
			),
			'two' => array(
				'rule' => array('minLength', 10),
				'message' => 'Title must be more than 10 characters'
			)
		)
	);

/**
 * Validation rules for when editing a post
 * 
 * @var array $validatePost
 */
	public $validatePost = array(
		'content' => array(
			'one' => array(
				'rule' => 'notEmpty',
				'message' => 'Please enter some content',
				'required' => true
			)
		)
	);

/**
 * Validate Author
 * 
 * @var array $validateAuthor
 */
	public $validateAuthor = array(
		'author_id' => array(
			'one' => array(
				'rule' => 'notEmpty',
				'message' => 'Please select an Author',
				'required' => true
			)
		),
	);

/**
 * Here we can check some conditions to see what validation we need
 * 
 * @return boolean
 */
	public function beforeValidate() {
		// We might want to check data
		if (isset($this->data['Post']['author_id'])) {
			$this->validate = array_merge($this->validate, $this->validateAuthor);
		}

		// Maybe only on an edit action?
		// We know it's edit because there is an id
		if (isset($this->data['Post']['id'])) {
			$this->validate = array_merge($this->validate, $this->validatePost);
		}

		// Perhaps we want to add a single new rule for add using the validator?
		// We know it's add because there is no id
		if (!isset($this->data['Post']['id'])) {
			$this->validator()->add('pubDate', array(
					'one' => array(
						'rule' => array('datetime', 'ymd'),
						'message' => 'Publish date must be ymd'
					)
				)
			)
		}

		return true;
	}

```

So now our validation array will contain a dynamic selection of rules based on conditions we choose. I prefer to use the `array_merge()` method because I find the validation rules easier to read and maintain.

You can find out more about the `validator()` in the book. [Dynamically change validation rules](http://book.cakephp.org/2.0/en/models/data-validation.html#dynamically-change-validation-rules)

### Success
Go and make a brew, you've just done some dynamic validation. Right in the model too, exactly where it should be. ggwp.
