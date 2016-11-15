---
layout: post
title: "Working with the Crud plugin in CakePHP"
date: 2015-02-13 12:31
comments: true
categories: ['cakephp', 'crud', 'rad', 'plugin']
---
## So what is it?
It's a plugin for CakePHP 2 and CakePHP 3 which saves you time and energy by automating basic tasks which you would usually write or bake youself. These are the CRUD operations (Create, Read, Update and Delete), hence the name of the plugin. 

The idea is that instead of writing repeated controller actions the same functionality can be provided for any model automatically using the events system.

This post is about Crud v3 which is for Cake 2.x

[Read more about the plugin in the repo](https://github.com/FriendsOfCake/crud).

## I'm sold, how do I use it?
Once the plugin is installed all you have to do is map a controller action to a crud action, create a controller and a view and you're job done. It's really that simple.

Let's do an example. I will add comments to the code for various techniques.  

**Assumptions**  
Here we'll assume the plugin is loaded already, the trait is being used and we have a model called `Examples`.
The following example is for Cake 2, as in Cake 3, there is no need to use the `prefix_method` notation.

```php
// I tend to configure Crud in my AppController to save on repitition
class AppController extends Controller {
	use CrudControllerTrait;
	
	public $components = [
		// Here we can configure our action maps, finders and all kinds of things
		'Crud.Crud' => [
			'actions' => [
				'index' => 'Crud.index', // Map any index action to the Crud index action handler
				'view' => [
					// Configure the options for this action method
					'className' => 'Crud.view',
					'validateId' => false
				],
				'admin_index' => 'Crud.index', // We can even hookup prefix methods
				'admin_add' => 'Crud.add',
				'admin_edit' => 'Crud.edit',
				'admin_delete' => 'Crud.delete'
			]
		]
	];
}

// Create your ExamplesController.php
class ExamplesController extends AppController {
	// We don't need any code here because Crud plugin can handle the methods for us!
	
	// If we want to just change how this action finds it's data we can specify a custom 
	// finder method and still have Crud do the rest of the work for us
    public function view($slug) {
        $this->Crud->action()->findMethod('slug');
		return $this->Crud->execute();
    }
    
    // What if we want to change the pagination in the index action?
    public function index() {
    	// We can hook the beforePaginate event to make changes
    	$this->Crud->on('beforePaginate', function (CakeEvent $event) {
    		// The event subject here is the controller, and paginator is the 
    		// paginator component.
    		// So we can change the page limit to 10 to include 10 items per page
    		// and we could contain a related model too!
    		$event->subject()->paginator->settings = [
    			'contain' => [
    				'User'
    			],
    			'limit' => 10
    		];
    	});
		
		// Let crud handle the rest of the action
		return $this->Crud->execute();
    }
}

// Create our view Examples/index.ctp
<div class='examples index'>
	<?php foreach($examples as $example) {
		echo $example['Example']['title'];
	}?>
</div>
```

## Woah, we just did loads!
Yep, with the above example, we've created all our admin prefixed Example methods, both our index and view methods. It's amazing how much quicker you can do stuff when you focus on the parts of your system which are not in the basic four CRUD actions.

## Make a brew
Why not eh? You've got loads of spare time now.
