---
layout: post
title: 'How to debug routes in Symfony2'
comments: true
categories: [routing, symfony2]
date: 2013-03-01 09:43:25
---

When choosing my type of routing in Symfony2, I wanted to avoid using XML as I've been put off it by working with Magento, so that left YAML and Annotation.  

I picked annotation as it looked neater, but I wanted to find out how to find a controller from a url.  

### Trace a url back to a controller
```bash
$ app/console router:match /players/
Route "players_index" matches
$ app/console router:debug players_index
[router] Route "players_index"
Name         players_index
Pattern      /players/
Class        Symfony\Component\Routing\Route
Defaults     _controller: PingPong\Bundle\PlayerBundle\Controller\PlayersController::indexAction
Requirements 
Options      compiler_class: Symfony\Component\Routing\RouteCompiler
Regex        #^/players/$#s
```

This will allow you to chase Controller annotated routes from the command line!

### Success
Have a brew, you've earned it.