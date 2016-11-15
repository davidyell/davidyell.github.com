---
layout: post
title: "Custom Pagination Helper links"
date: 2013-11-27 11:00
comments: true
categories: [cakephp, pagination]
---
###Scenario
You are paginating a set of records but you want to use a custom url for that specific filter. For me this was pagination a set of news articles by category.

The url I wanted to use was `/news/category/daves-awesome-category` and then paginating results on `/news/category/daves-awesome-category/page:3`. However the Paginator helper didn't want to play ball.

I had already created my route.

```php
Router::connect('/news/category/:category/*', array('controller' => 'news_articles', 'action' => 'index'), array('category' => '[a-z0-9-]+', 'pass' => array('category')));
```

But instead of the expected links above, I was getting `/news/daves-awesome-category/page:2` missing out my keyword from the url.

### Solution
The Paginator helper options array to the rescue! You can actually configure the options of the helper right in the view. Such a simple fix.

Here is my pagination including the fix to adjust the url if a category is set.

```php
if (isset($category)) {
	$this->Paginator->options['url'] = array('controller' => 'news_articles', 'action' => 'index', 'category' => $category['NewsCategory']['slug']);
}
echo $this->Paginator->prev('< ' . __('previous'), array(), null, array('class' => 'prev disabled'));
echo $this->Paginator->numbers(array('separator' => ''));
echo $this->Paginator->next(__('next') . ' >', array(), null, array('class' => 'next disabled'));
```

### Done!
Make a brew, and probably have a biscuit too. Why not eh? You've earned it.