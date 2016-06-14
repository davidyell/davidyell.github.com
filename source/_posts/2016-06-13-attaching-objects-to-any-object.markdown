---
layout: post
title: "Create an association to any object in your project"
date: 2016-06-13 14:41:01 +0100
comments: true
categories: ['cakephp', 'cakephp3']
---

[Adapted from my Stackoverflow answer](http://stackoverflow.com/questions/31953090/cakephp3-link-one-table-to-multiple-tables-depending-on-type/32965679#32965679).

## Why?
Well if you have a project where you need to add arbitrary information to 
anything across your whole application, you'll need a way to create that 
association in a simple way.

The real-life example which got me learning this technique was that I needed to 
be able to add an exit click url to any item across my whole project, so that I 
could control traffic leaving the website.

## How's it work?
For this example, we'll allow attaching of a Policy to any item in the system.

This is actually a lot simpler than it first appears. I've done this a few times 
so I'll detail the technique that I use.

The first thing to do is create a behavior. This will allow you to enable any 
Table class in your application to have a policy attached to it.

The behavior is very simple. I'll talk through it after the code.

```php
namespace App\Model\Behavior;

use Cake\Event\Event;
use Cake\ORM\Behavior;
use Cake\ORM\Query;

class PolicyBehavior extends Behavior
{
    public function initialize(array $config)
    {
        parent::initialize($config);

        $this->_table->hasMany('Policies', [
            'className' => 'Policies',
            'foreignKey' => 'table_foreign_key',
            'bindingKey' => 'id',
            'conditions' => ['table_class' => $this->_table->registryAlias()],
            'propertyName' => 'policies'
        ]);
    }

    public function beforeFind(Event $event, Query $query, \ArrayObject $options, $primary)
    {
        $query->contain(['Policies']);
        return $query;
    }
}
```

So the in the `initialize` method we need to create a relationship to the table we attached the behaviour to. This will create a `Table hasMany Policies` relationship, meaning that any item in your system can have many policies. You can update this relationship to match how you're working.

You can see that there are a number of options defined in the relationship. These are important, as they link the tables items together. So the `table_foreign_key` is a field in your `policies` db table used to store the primaryKey of the related item. So if you're attaching a Policy to a Car, this would be the `Car.id`. The `bindingKey` is the key used in the Policy table to join on.

In order to filter the different types of attachments, you need the `table_class` field in your `policies` db table. This will be the name of the attached table class. So `Cars`, `Cats`, `Houses` etc. Then we can use this in the conditions, so anything pulling the primary table class will automatically filter the related `Policies` to match.

I've also configured the `propertyName`, this means that any item you look for which contains `Policies` will have an entity property called `policies` with the related data inside.

The last function in the behaviour is the `beforeFind`, this just ensures that whenever you look for the primary table class, you always return the related `policies`, you don't have to use this if you don't want to, but I found it handy to always have the related data in my use-case.

So then, how do we use this new behaviour? Just attach it like you would any other behaviour, and that's it. `$this->addBehavior('Policy')`.

**Be aware**  
This just reads data, you'll need to ensure that you save the table alias, and the `foreignKey` into the related table when creating new items.

Easiest way is to just use a `beforeSave()` and append the current Table class `alias()`, and `primaryKey()`.

Just for clarity, your `policies` table schema will need, at a minimum.

```sql
policies.id
policies.table_class VARCHAR(255)
policies.table_foreign_key INT(11)
```

## That's it!
Go make a brew.

