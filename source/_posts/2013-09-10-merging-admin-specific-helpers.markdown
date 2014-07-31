---
layout: post
title: "Merging Admin specific helpers"
date: 2013-09-10 14:32
comments: false
categories: [cakephp, layout, admin, helpers, optimisation]
---

### The scenario
You have a bunch of helpers which you are only using in the `/admin` section of your website. We don't need to load these on the front-end part of the site.

### Solution
```php
<?php
// app/AppController.php
	public $helpers = array(
		'Html',
		'Form',
	);

	public $adminHelpers = array(
        'NiceAdmin.Actions',
        'NiceAdmin.Boolean',
	);

public function beforeFilter() {
	if (isset($this->request->params['admin'])) {
		$this->layout = 'admin';
		$this->helpers = array_merge($this->helpers, $this->adminHelpers);
	}
}
```

### Success!
Make a brew.