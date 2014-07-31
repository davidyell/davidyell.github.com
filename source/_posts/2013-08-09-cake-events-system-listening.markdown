---
layout: post
title: "How to use a Component to listen to Cake events"
date: 2013-08-09 15:29
comments: false
categories: [cakephp, components, events]
---

I wanted a sleek way to inject SEO meta tags into my project without a large overhead and lots of complicated finds and model joints. Thanks to [jose_zap's](https://github.com/lorenzo) suggestion, I decided to learn about the [Cake Events](http://book.cakephp.org/2.0/en/core-libraries/events.html) system.

```php
<?php
/**
 * Component to find and load seo data and inject it into the view
 *
 * @author David Yell <neon1024@gmail.com>
 */

App::uses('CakeEventListener', 'Event');
App::uses('CakeEvent', 'Event');

class SeoComponent extends Component implements CakeEventListener {
	
/**
 * Setup the component
 * Called after the Controller::beforeFilter() and before the controller action
 * 
 * @param Controller $controller
 */
	public function startup(Controller $controller) {
		parent::startup($controller);
		$controller->getEventManager()->attach($this);
	}

/**
 * List of callable functions which are attached to system events
 * 
 * @return array
 */
	public function implementedEvents() {
		return array(
			'View.beforeLayout' => 'writeSeo'
		);
	}

/**
 * Inject the seo data into the view
 * 
 * @param CakeEvent $event
 * @return void
 */
	public function writeSeo(CakeEvent $event) {
		// Looking for vars set to the view isn't especially robust! This should probably call a behaviour method which goes and looks up data
		if (!empty($event->subject()->viewVars['content']['Content']['seo_title'])) {
			$event->subject()->viewVars['title_for_layout'] = $event->subject()->viewVars['content']['Content']['seo_title'];
		}
		
		if (!empty($event->subject()->viewVars['content']['Content']['seo_description'])) {
			$event->subject()->Html->meta('description', $event->subject()->viewVars['content']['Content']['seo_description'], array('block' => 'meta'));
		}
		
		if (!empty($event->subject()->viewVars['content']['Content']['seo_keywords'])) {
			$event->subject()->Html->meta('keywords', $event->subject()->viewVars['content']['Content']['seo_keywords'], array('block' => 'meta'));
		}
	}
}
```


### Success
Make a brew!