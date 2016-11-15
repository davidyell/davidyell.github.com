---
layout: post
title: "How to implement CodeSniffer into Netbeans"
description: "Linking up phpcs and phpmd with Netbeans"
comments: true
categories: [pear, phpcs, phpmd, netbeans]
date: 2013-01-15 09:43:25
---

### Things you will need
**Netbeans plugin**
Download the plugin from here, [http://plugins.netbeans.org/plugin/40282/phpmd-php-codesniffer-plugin](http://plugins.netbeans.org/plugin/40282/phpmd-php-codesniffer-plugin)  
**OR**  
[http://plugins.netbeans.org/plugin/42434/phpcsmd](http://plugins.netbeans.org/plugin/42434/phpcsmd)  
This plugin will highlight the lines in your IDE rather than just ouputting items to your action window.  

Download whichever one you think will be less invasive in your dev. As switching between frameworks and standards can cause whole pages to go pink with `phpcsmd` plugin.

**PEAR installed**
You can test this by typing into a terminal `which pear` and it should return a path. If you do not have PEAR installed, you can find out how to install it from the site, [http://pear.php.net/](http://pear.php.net/)

**PEAR packages**
_Note_
If you get errors whilst installing PEAR packages, try using `sudo`.  

```bash
$ pear install PHP_CodeSniffer
```  
[http://pear.php.net/package/PHP_CodeSniffer](http://pear.php.net/package/PHP_CodeSniffer)

```bash
$ pear channel-discover pear.phpmd.org
```  
```bash
$ pear channel-discover pear.pdepend.org
```  
```bash
$ pear install --alldeps phpmd/PHP_PMD
```  
[http://phpmd.org/](http://phpmd.org/)

### Install the plugin
In Netbeans click **Tools** > **Plugins** and click the **Downloaded** tab. Click on **Add Plugins…** and browse to where your `.nbm` file is located. Install this file and restart Netbeans.

### Configuring the plugin
Now you need to configure the plugin so it can find your PEAR packages.
You will need to configure this in **Preferences**.

Click on the **PHP** tab, and browse to the **phpMD** tab. Click **change** and in here specify the path to your `phpmd`. For me this is `/usr/local/Cellar/php53/5.3.17/bin/phpcs` but you can test this in a terminal with `which phpmd` to ge the path.

Now click on **phpCodeSniffer** tab and do the same as above to enter the path to your `phpcs` executable.

#### Other settings
In here I tend to specify that the `tabsize` is 4 to conform with PSR-2.

### How do I 'run' it?
**Window** > **Action items**

or `CMD` + `6`

Then you can use the filters on the left to adjust which things it scans. I tend to use the 'paper sheet' icon at the top which is 'current file'. You can then use the funnel icon to specify which things you want displayed.