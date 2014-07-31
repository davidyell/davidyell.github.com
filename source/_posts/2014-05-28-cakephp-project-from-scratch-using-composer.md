---
layout: post
title: "Creating a CakePHP project using Composer"
date: 2014-05-28 11:47
comments: true
categories: ['cakephp', 'composer']
---
### Scenario
I want to start a new CakePHP project, but I don't want to download anything. Ideally I want to create my application with the framework as a dependancy.

### Solution
So firstly you'll want to create yourself a project folder. For this example we'll create a project to adopt a cat.
We need to create ourself a project folder, make it into a Git repo so we can version our code, and then we can start adding our dependancies.

```bash
$ mkdir KittyMarket
$ cd KittyMarket
$ git init
$ composer init
```

So after the last command Composer will ask you about your project, so you can complete the details there. Be sure that you set your `"minimum-stability": "dev"` so that packages loading from `dev-master` will work.

Then you can start defining your dependancies. Firstly you'll want `cakephp/cakephp` and set a version of the latest, currently `2.5.1`.

After this I like to edit my `composer.json` to add some configuration, so that Composer will put things in the correct CakePHP place.

```json
    "config": {
        "vendor-dir": "vendors",
        "bin-dir": "vendors/bin"
    }
```

Once you've added all your dependancies it's time to make the app. Be aware that the `../vendors/bin` might be different if you didn't use the above amend to your `composer.json`.

```bash
$ mkdir app
$ cd app
$ ../vendors/bin/cake bake
```

This will copy in the framework skeleton.

### Done!
Make a brew!
