---
layout: post
title: "jQuery UI Accordion"
date: 2015-05-20 16:14:00 +0100
comments: true
categories: ['jquery', 'jqueryui']
---
## Time for some Javascript!

So I've implemented the [jQueryUI Accordion](https://jqueryui.com/accordion/) as a menu system for a CMS that I'm building and I've noticed that the active header will not open automatically.

This means that when a user hits a link to visit a new page, the accordion will not display their current menu item as active.

Time for some code!

## Markup

I've used the same markup as the example, except that because it's a menu, I needed some bullet points.

```html
<div id='accordion'>
	<h3>First Heading</h3>
	<div>
		<ul>
			<!-- Add an active class how you want to -->
			<li class='active'>First item</li>
			<li>Second item</li>
		</ul>
	</div>
	<!-- More items repeated -->
</div>
```

## jQuery

So how can we use the markup to tell the javascript which heading to mark active? Turns out it's remarkably simple.

```javascript
$(function () {

//  Accordian navigation
    $('#accordion').accordion({
        heightStyle: "content"
    });

    $('#accordion h3').each(function (i,e) {
        if ($(e).next('div').find('li.active').length > 0) {
            $("#accordion").accordion("option", "active", i);
        }
    });
    
})
```

The trick here is the use of `next()` to select the immediately adjacent `div` without selecting all the other siblings.

## Go make a brew!

All done, that's it!