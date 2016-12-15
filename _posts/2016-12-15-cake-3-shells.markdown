---
layout: post
title: "Cake 3 Shell help output"
date: 2016-12-15T12:04:35+00:00
comments: true
categories: ['cakephp', 'cakephp3', 'shell', 'cli', 'console']
---

## Making a shell
So you have some code you want to run on the command line, so we'll build a shell for that. However I'm always looking
for template code which includes shell sub-commands with arguments. I don't find the [Books Shell documentation](http://book.cakephp.org/3.0/en/console-and-shells.html)
very good for this, as I'm always looking for a template I can customise.

Easy to create the basic shell using Bake.

```bash
$ bin/cake bake shell Example
```

## Help output
The actual help output uses an `optionParser` to output what the shell does. The easiest way to [configure this is using 
the fluent interface](http://book.cakephp.org/3.0/en/console-and-shells.html#configuring-an-option-parser-with-the-fluent-interface).

```php
<?php
/**
 * Manage the available sub-commands along with their arguments and help
 *
 * @return \Cake\Console\ConsoleOptionParser
 */
public function getOptionParser()
{
    $parser = parent::getOptionParser();

    $parser->addSubcommand('new', [
        'help' => __('Create a new partner')
    ]);

    $parser->addSubcommand('foo', ['help' => 'A foo command for fooing'])
        ->addArgument('b', [
            'help' => 'For creating bees',
            'required' => true,
            'choices' => ['biological', 'mechanical']
        ])
        ->addArgument('c', ['help' => 'Creates the sea'])
        ->addOption('wet', ['short' => 'w', 'help' => 'A wet run']);

    return $parser;
}
```

## Make a brew
That's it, make a brew.