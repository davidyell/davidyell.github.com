---
layout: post
title: "Allow alt-tab across all viewports in Ubuntu"
description: "How to be able to alt-tab across multiple viewports in Ubuntu"
comments: true
categories: [ubuntu, productivity]
date: 2013-01-16 09:43:25
---

I dev at work on a Mac and at home in an Ubuntu VM. On my Ubuntu machine I tend to spread my apps across multiple viewports for ease of organisation. However the new Ubuntu Unity plugin for Compiz means that alt-tab only works for your current viewport. Not ideal.

###First thing's first
You'll need to install the CompizConfig Settings Manager. You can find this in the software centre by searching for '**Compiz**', or you can install it from Terminal using `sudo apt-get install compizconfig-settings-manager`.

###Configure Unity
Once this is installed it should appear in your applications bar on the left. If not, hit the Ubuntu button and type 'Compiz'.  

Scroll down to the third section, and click on the '**Ubuntu Unity Plugin**'. Click on the '**Switcher**' tab at the top, and then untick the '**Bias alt-tab to prefer windows on the current viewport**'.

###Make a brew!
That's it. Job done. Happy alt-tabbing.