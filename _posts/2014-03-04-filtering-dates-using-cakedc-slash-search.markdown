---
layout: post
title: "Filtering dates using CakeDC/Search"
date: 2014-03-04 10:54
comments: true
categories: ['cakephp', 'cakedc', 'search']
---

### Scenario
I needed to do a search on a date field in my database which was stored as a `DATE`, but only filtering by month.

### Solution
So I had already implemented the [CakeDC/Search](https://github.com/cakedc/search) into my project, so all I needed to do was create a custom method to return the right conditions for the query.  

My model already had the `filterArgs` setup.  

```php
/**
 * Setup default search filters
 *
 * @var array
 */
	public $filterArgs = [
		'day_type_id' => ['type' => 'value'],
        'month' => ['type' => 'query', 'method' => 'filterByMonth']
	];
```

So I just added a `type` of `query` and passed in a `method`.  

```php
/**
 * Filter the pagination by month
 *
 * @param array $data
 * @return array
 **/
    public function filterByMonth(array $data = array()) {
        return [
            "DATE_FORMAT({$this->alias}.date, '%c')" => (int)$data['month']
        ];
    }
```

The `$data` array will contain all the fields setup in your model along with their values. So for me it looked like  

```php
[
    'day_type_id' => 3,
    'month' => '08' // Note the string type, hence why I cast to int
]
```

### Done!
Make a brew!
