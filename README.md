﻿# Div PHP Ways 1.3

A "way" is different to a "route". We need a path for found 
a specific resource, but we need a way for do something. 
This library follow this concept when implements the 
routing and control of PHP application.

With Div Ways you should think more about "control points" 
than on controllers of an MVC pattern. Control points are 
activated when they are needed, ie on demand, depending on 
the definition you have made.

On other platforms it is common to define all routes to 
the drivers in the same file and once. In Div Ways this 
is not an obligation. As you can see in the examples, 
you can have an initial control point and depending on 
the URL go to another control point X where routes are 
defined, so that the path is formed on demand, thus 
improving performance of its application. The structure 
of a URL may suggest that Div Ways allows a hierarchical 
structure of contorl points, but it does not, it can 
create a whole graph structure.

In addition to this, a control point may require the 
previous execution of another control point. You can also 
implement events or hooks, so you can execute one control 
point before or after another, without the latter knowing 
the existence of the first. These flexibilities are valid 
for example in a plugins architecture.

The control points can interact, and this means, redirect 
the flow to another, call control points directly, exchange 
data and url arguments, handle the output on screen, etc.

Div Ways is not only intended for the web but also for 
command line applications. Div Ways is implemented in a 
single class, in a single file. This allows quick start-up
and easy adaptation with other platforms.

## Basic usage
```php
<?php

// arbitrary location for software's packages
define('PACKAGES', 'path/to/app/');

// include the library
include "path/to/divWays.php";

// ways with closure
divWays::listen("get://home", function($data){
	echo "Hello {$data['user']}";
}, "home");

// add a hook
divWays::hook(DIV_WAYS_BEFORE_RUN, "home", function($data){
	$data['user'] = "Peter";
});

// listen... 
$data = divWays::bootstrap('_url', 'home');
```

## Call a static method

**app/control/Home.php**
```php
<?php

#id = home
#listen = /home

class Home {
	
	static function Run($data)
	{
	    echo "Hello world";
	}
	
	static function About($data)
	{
		echo "About us";
	}
}
```

**index.php**
```php
<?php

include "divWays.php";

// register a controller with the default static method ::Run()
divWays::register("app/control/Home.php");

// route to another static method ([controllerID]@[method])
divWays::listen("/about", "home@About");

// route to a static method
divWays::bootstrap("_url", "home");
```

**.htaccess**
```apacheconfig
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^((?s).*)$ index.php?_url=/$1 [QSA,L]
```
See the example.

