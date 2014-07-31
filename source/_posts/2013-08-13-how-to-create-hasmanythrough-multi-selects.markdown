---
layout: post
title: "How to create hasManyThrough multi-selects"
date: 2013-08-13 16:29
comments: true
categories: [cakephp, hasmanythrough, habtm, select, checkbox]
---
**Updated 6th November 2013**  

### The scenario
In your CMS you have a HABTM relationship between two models, something like `Post` and `Tag`. However you want to store some extra data about the `Tag`, such as who created it. This means that the relationship between the two models will in fact need to be a `hasManyThrough` and use a join model. [You can read more about this type of relationship in the CakePHP CookBook](http://book.cakephp.org/2.0/en/models/associations-linking-models-together.html#hasmany-through-the-join-model).

You want to be able to select and save multiple tags when creating a `Post`. So the logical step here is to load a list of `Tag` items and display them in a multi-select element in our view.

The alternative scenario is using checkboxes to add a `hasMany` relation to a parent record.

### The problem
When creating a multi-select there is no form field configuration which will allow the data to be formatted in a way which is compatible with any of the `save()` methods.

For example,
Setting up your form using a multi-select such as `$this->Form->input('tag_id', array('type' => 'select', 'multiple' => true))` will not allow you to numerically index your fields. This means that your data will look like the following.
```
array (size=1)
  'tag_id' => 
    array (size=3)
      0 => string '28' (length=2)
      1 => string '29' (length=2)
      2 => string '30' (length=2)
```

However this isn't compatible, and must be transformed into a numerically indexed form such as this.
```
array (size=3)
  0 => 
    array (size=1)
      'tag_id' => string '28' (length=2)
  1 => 
    array (size=1)
      'tag_id' => string '29' (length=2)
  2 => 
    array (size=1)
      'tag_id' => string '30' (length=2)
```

This new data format can now be assigned to your join model and saved using `saveAll()`. However the dependency isn't respected and as such, you need the hack for deleting the existing join model records as you can see below.

### Solution

#### The models
```php
<?php

// Model/Post.php
$hasMany = array(
	'PostsTag' => array(
		'className' => 'PostsTag',
		'foreignKey' => 'post_id'
	)
);

// Model/Tag.php
$hasMany = array(
	'PostsTag' => array(
		'className' => 'PostsTag',
		'foreignKey' => 'tag_id'
	)
);

// Model/PostsTag.php
$belongsTo = array(
	'Post' => array(
		'className' => 'Post',
		'foreignKey' => 'post_id'
	),
	'Tag' => array(
		'className' => 'Tag',
		'foreignKey' => 'tag_id'
	),
);
```

#### The form in the view  
If you want to use a multi-select field here you can use the following.

```php
<?php
echo $this->Form->input(
	'PostsTag.tag_id', 
	array(
		'type' => 'select', 
		'multiple' => true, 
		'selected' => Hash::extract($this->request->data['PostsTag'], '{n}.tag_id')
	)
);
```

If you would like to use checkboxes instead you can change the Form Helper to use checkboxes.  

**Important note**  
If you are using checkboxes and want to validate your data you will need to ensure that the `hiddenField` option is not **false**. Otherwise the data will not appear in the data array and you will not be able to validate it.  

```php
<?php
echo $this->Form->input(
	'Post.tag_id',
	array(
		'multiple' => 'checkbox',
		'options' => $tags // A list of tags fetched with $this->Post->Tag->find('list')
	)
);
```

#### The controller except
Controller method excerpt to show the usage of the function
```php
<?php
// Here we are massaging the data in order to transform it
$this->request->data['PostsTag'] = $this->Post->PostsTag->massageHasManyForSaveAll($this->request->data['PostsTag'], 'tag_id', $this->request->data['Post']['id']);

$this->Post->saveAll($this->request->data;
```

#### The AppModel hack
In the AppModel we need to implement a hack to massage the data. You'll notice that we call it on the join model, as above. This is important as the join model will have the correct relationships beteween the two models. We can use this to our advantage to find out the related keys for the delete.

```php
<?php
/**
* Transform a set of hasMany multi-select data into a format which can be saved
* using saveAll in the controller
* 
* @param array $data
* @param str $fieldToSave
* @param int $deleteId
* @return array
*/
public function massageHasManyForSaveAll($data, $fieldToSave, $deleteId = null) {
	foreach ($this->belongsTo as $model => $relationship) {
		if ($relationship['foreignKey'] != $fieldToSave) {
			$relatedModel = $model;
			$relatedModelPrimaryKey = $this->{$model}->primaryKey;
			$relatedForeignKey = $relationship['foreignKey'];
		}
	}

	if ($deleteId !== null) {
		$this->deleteAll(array(
			$this->alias .'.'. $relatedForeignKey => $deleteId
		));
	}

	if (is_array($data[$fieldToSave])) {
		foreach ($data[$fieldToSave] as $packageId) {
			$return[] = array($fieldToSave => $packageId);
		}
		
		return $return;
	}

	return $data;
}
```

### Success
Make a brew!