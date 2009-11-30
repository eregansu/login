Introduction
============

Eregansu is an application framework written in PHP. It requires PHP 5, probably PHP 5.2. It shouldn’t require PHP 5.3 just yet.

Rather than try to build yet another PHP framework designed to hide the gory details of how a modern application works from the poor developer who doesn’t really know what’s going on, Eregansu has quite a specific target audience: developers who could write this themselves, but have better things to be doing with their time.

As such, Eregansu doesn’t go out of its way to be the easiest or most modular framework in the world. It provides enough to make life easier, and aims to be well-structured enough that it can be extended easily as required. Extensions to some parts (such as authentication or session-handling) will require changing the code, but this is one of those age-old trade-offs.

Eregansu does not seek to be as comprehensive as, say, the Zend Framework, which currently weighs in at several tens of megabytes. Eregansu is lightweight and fast.

This is not an abstract work, either. Eregansu (in its current incarnation) was created for a specific application and open sourced. There wasn’t a meeting one day where somebody exclaimed “I know, let’s write an open source PHP application framework!”

To many developers, the structure will be immediately recognisable as an Model-View-Controller (MVC) framework. Within Eregansu, models are just that, descendants of [Routable](http://github.com/nexgenta/eregansu/blob/master/routable.php) (technically, any instance of IRequestProcessor will do) are the controllers, and instances of [Template](http://github.com/nexgenta/eregansu/blob/master/template.php) are the views. The template engine itself is as simple as it can possibly be: templates are PHP, though by convention have a “.phtml” extension to indicate the fact that they’re primarily mark-up (with code sprinkled over them), rather than the other way around.

If you’re just getting started with Eregansu, you could do worse than clone the [Eregansu-Examples](http://github.com/nexgenta/Eregansu-Examples/) project.

One caveat: it is assumed, just about everywhere, that you know how to configure your web server. Although the code doesn’t, the default .htaccess file makes the assumption that each Eregansu application runs on its own virtual host (see the RewriteBase statement). It’s also assumed that you know what a symbolic link is and how to use them.

If you want to learn how Eregansu works, start by reading the default [index.php](http://github.com/nexgenta/eregansu/blob/master/index.dist.php) and follow the code.

Eregansu Hello World
====================

Change to your web server root (or wherever you want your application to
be served from).

Create directories named 'config', 'app', and 'templates'

	$ mkdir config app templates


Check out Eregansu into a directory called 'platform':

	$ git clone git://github.com/nexgenta/eregansu.git platform


Create symbolic links and copies to templates, and create a sensible
default index.php:

	$ cd templates
	$ ln -s ../platform/login/templates login
	$ cd ..
	$ cp platform/index.dist.php index.php


You will also want an .htaccess file. Start with the provided one:

	$ cp platform/htaccess.dist .htaccess


If you’re going to use the provided login applet, you’ll want Chroma-Hash:

	$ git clone git://github.com/mattt/Chroma-Hash.git


Next, configure your instance. Copy the default (empty) instance configuration
and make it the current configuration:

	$ cp platform/config.default.php config/config.myhostname.php
	$ ( cd config && ln -s config.myhostname.php config.php )

(Replace “myhostname” with the name of your host — e.g., johndev)

Now you can configure your application. Launch a PHP editor and create a new
file containing the following:

	<?php
	
	/* My application configuration */
	
	$HTTP_ROUTES = array(
		'__NONE__' => array('class' => 'Page', 'templateName' => 'hello.phtml'),
	);
	
Save this file as config/appconfig.hello.php

Make this application configuration active by symlinking it:

	$ ( cd config && ln -s appconfig.hello.php appconfig.php )

Create a dummy template:

	$ mkdir templates/default
	$ echo '<h1>Hello, World!</h1>' > templates/default/hello.phtml

Finally, launch your web browser and navigate to the server you’ve performed
all of this on. If all is well, you should see a page containing just the words
“Hello, World!”.
