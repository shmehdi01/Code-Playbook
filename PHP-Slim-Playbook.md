Welcome to the Slim-Framework-Playbook wiki!

## Introduction

You need to setup server to run your Slim Framework. Linux Server (Any Red Hat Distro) can be configured with multiple php version. You need to follow CodeIgniter Home Wiki. 

Table of Contents
1) [Introduction](#introduction)
2) [Best Practices](#best-practices)
3) [Caching Data](#caching-data)
4) [NotORM for Slim Framework](#notorm-for-slim-framework)
5) [REST API with Slim](#rest-api-with-slim)
6) [Routes](#routes)
7) [TroubleShooting](#troubleshooting)
8) [Twig Template Rendering Engine](#twig-template-rendering-engine)
9) [Why choose Slim Framework](#why-choose-slim-framework)

Slim can be easily installed using composer(Dependency Injection tool). Also Slim is not based on MVC(Model-View-Controller) framework and it does not follow any design pattern. But by using third party libraries and template rendering engines.
We can build full-fledged MVC framework.  

Slim Class Instance is created and you need to autoload in vendor directory.

    $app = Slim\App(); # Slim uses Namespace very intelligently

After your Slim app is instantiated you can define your endpoints.
Slim has a very good support of REST(Representation State Transfer) API(Application Programming Interface). You can make callback function for a route when called with different header.

Example:

    $app->get('/api/users',function(){
    // Statements
    });

You can also receive argument from the URL.
In POST Method case there are in-built function to extract information from Header or Body.

You can also define regular expression for your routes to handle the request.

To handle the requests your must use Twig(Template Rendering Engine) or PHP View.
For Simplifying complexity of SQL Queries use Slim with NotORM

## Best-Practices

Slim is designed to develop small or medium project. We here understand how to get more from less code and standardize it.

How to Organize Your Application of any scale (Scalability)

1) [FileSystems Layout (Structure)](https://www.slimframework.com/2011/09/24/how-to-organize-a-large-slim-framework-application.html) 
2) [Lazy Loading Slim Controller using Slim](http://nesbot.com/2012/11/5/lazy-loading-slim-controllers-using-pimple)
3) [REST API Components](https://www.sitepoint.com/best-practices-rest-api-scratch-introduction/)
4) [Fork MVC Schema for Slim](https://github.com/revuls/SlimMVC)
5) [Object Relational Mapping (Not ORM)](http://www.notorm.com/)
6) [Template Rendering with Twig-View](https://twig.sensiolabs.org/)
7) [Slim HTTP Cache](https://github.com/slimphp/Slim-HttpCache)
8) [Accessing Services in Slim](https://akrabat.com/accessing-services-in-slim-3/)

## Caching-data

HTTP Caching Engine will improve performance of your application. By Caching static data which need not to be called always when site loads. 

You can install Slim-HttpCache using composer

`composer require slim/http-cache`

To Read Documentation and Source code.
[HTTP Caching](https://github.com/slimphp/Slim-HttpCache)

To Understand how to _add _ , _expiry time _ , _Etags_ for your Cache data then follow below link

[Slim Official Documentation for HTTP Caching](https://www.slimframework.com/docs/features/caching.html)

## NotORM-for-Slim-Framework

NotORM is a Popular PHP library handles data without any complexity of SQL Queries. 

NotORM can be easily installed using composer

`composer require vrana/notorm:dev-master`

To Harness the Power Of Object Relational Mapping. Read the NotORM official Documentation 

[Not ORM official Documentation](http://www.notorm.com)

To Read Source code of Not ORM

[Source Code](https://github.com/vrana/notorm)

## REST-API-with-Slim

REST API is all about how to handle requests and responses from HTTP. 

**Note:** REST only works for HTTP Protocol whereas SOAP is used for various other TCP protocols including (HTTp, SMTP, FTP). Also when your are returning your result in REST then uses the Proper header.
Example:
> header('content-type: application/json');

Default is HTML. you can also specify various other fields of HTTP Header.

### CodeBlock:

	<?php
	use \Psr\Http\Message\ServerRequestInterface as Request;
	use \Psr\Http\Message\ResponseInterface as Response;
	# Call the Vendor File 
	require 'vendor/autoload.php';
	error_reporting(E_ALL);
	# Once Called Create the Slim class object
	$app = new \Slim\App;
	$container = $app->getContainer();
	// Register component on container
	$container['view'] = function ($container) {
		$view = new \Slim\Views\Twig('templates', [
			'cache' => 'cache'
		]);
		// Instantiate and add Slim specific extension
		$basePath = rtrim(str_ireplace('index.php', '', $container['request']->getUri()->getBasePath()), '/');
		$view->addExtension(new Slim\Views\TwigExtension($container['router'], $basePath));
		return $view;
	};

	# Write the route of the rest api
	$app->get('/', function(Request $request, Response $response) {
		# Now Set the status to 200
			echo "Hello World Home Page\n";
	});
	// Render Twig template in route
	$app->get('/hello/{name}', function (Request $request, Response $response) {
		return $this->view->render($response, 'index.html', [
			'name' => $request->getAttribute('name'),
			'title' => 'Hello Page'
		]);
	});
	// Name and Age
	$app->get('/hello/{name}/{age}', function(Request $request, Response $response){
		# Now Set the status to 200
		$name = $request->getAttribute('name');
		$age = $request->getAttribute('age');
		if(is_numeric($age)==false)
		{
			echo "Wrong Input";
		}
		$response->getBody()->write("Hello, $name. your age is $age");
		return $response;
	});
	$app->run();
	?>

### Handling Requests with Slim

In Above code this will handle three endpoints and rest urls will be redirected to 404 Error.

When the BASE_URL or BASE_URL/ will be requested using GET method then the Closure(Anonymous function) will be Executed. 
you can define your routes specific, map your routes using map() function and for all method use any() function. For Regex if you need to accept URL contain only digits user :num or [0-9]. To define the length of these digits use character width fields. [0-9]{0,3} from 0 to 3. If you want URL should accepts digits more than three then [0-9]{3,}.

> $app->get('URL Pattern', function(){});

> $app->post('URL Pattern', function(){}); 

> $app->put('URL Pattern', function(){});

Similarly for other HTTP Methods but remember that how the arguments are passed in HTTP Header and retrieval of these arguments is important and error prone is not Handled safely.

### Handling Responses with Slim

Depends upon your requirement whether you need to return JSON, XML , HTML use the corresponding header.
When you are sending data over the network remember to Serialize the data.
It is a Array (Associative array or Numeric nested arrays). Encode then into JSON and then only pass them over network.
It is an Object then also you need to serialize to obtain all variables.
For HTML using any template rendering engine register it with container as above code does. 
And then render these templates.

### Handling Cookies with Slim

Best Practice to handle cookies is don't rely on Slim Framework. Create your own Helper method and include it. And Perform all other operation from this Class. Remember to create method for SetCookie, DeleteCookie, UpdateCookie, Retrieve parameters from cookie with associative array in key value format. 

## Routes

Slim doesn't have an MVC design pattern. So to understand how slim routes work.

Slim Framework gives enormous power to writes that best for your application. But it does not follow any design pattern it is difficult to build large application.

### Getting Started with routes in Slim Framework

1) **Pass the variable name in URL Pattern and In Closure args(associative array will store values with their corresponding keys)**

    $app = new Slim\App();
    $app->get('/product/{id}', function($request, $response, $args){
    // Access your id using $args['id']
    });

You can pass as many number of arguments enables in your PHP Configuration file.

2) **Any Routes in Slim Framework**

You have the flexibility to use routes for any specified method and for all methods

    $app = new Slim\App();
    $app->any('/product/{id}', function($request, $response, $args){
    // This Method will be called when requested with GET, POST, PUT, DELETE, OPTIONS  
    });

3) **Map Routes or Optional Routes**

You can define routes that are optional for your application

    $app = new Slim\App();
    $app->map(['GET', 'POST'], '/products/', function($request, $response, $args){
    // Now When base_url/product is called with GET or POST then the same methods is called 
    // Now benefit of map is your code is almost same but only few statements needs to be method
    // Oriented then use $_SERVER['REQUEST_METHOD']
    });

4) **Optional Arguments**

When you want optional arguments in your routes then use character class

    $app = new Slim\App();
    $app->get('/user[/{reg_id}][/{api_key}]',function(){
    // Write your Statements Here.....
    });

## TroubleShooting

Here we listed Issues with Their Solutions.

## Twig-Template-Rendering-Engine
 
Slim framework don't have Views like MVC framework whereas it has Views as HTTP Response. You need to return view as HTTP Response message being compliant with PSR-7. 
To integrate views in Slim Framework you can use any Template Rendering Engine form Twig-view or Smarty-View and many others.

Here we will use Twig-View because it is super easy and flexible to use. Also peoples having experience of Python Jinja/jinja2 for template rendering engine will find almost identical Syntax.

### Installing Twig View for Slim Framework using Composer

    composer require slim/twig-view

After your Twig-View is installed create a Sample template and then return to Http Response.

### Creating Twig-View Template

    {% extends "layout.html" %}
    {% block body %}
    <h1>User List</h1>
    <ul>
       <li><a href="{{ path_for('profile', { 'name': 'josh' }) }}">Josh</a></li>
    </ul>
    {% endblock %}

**Note:** To Access Variable and other Properties you need to enclose them with {} and if your code uses control structures method or twig keywords then enclose them with {% keyword statements %}
 
### Registering Twig view with Container

    <?php
    // Create app
    $app = new \Slim\App();
    // Get container
    $container = $app->getContainer();
    // Register component on container
    $container['view'] = function ($container) {
    $view = new \Slim\Views\Twig('path/to/templates', [
        'cache' => 'path/to/cache'
    ]);
    // Instantiate and add Slim specific extension
    $basePath = rtrim(str_ireplace('index.php', '', $container['request']->getUri()->getBasePath()), '/');
    $view->addExtension(new Slim\Views\TwigExtension($container['router'], $basePath));
    return $view;
    };

### Rendering Twig Templates

    // Render Twig template in route
    $app->get('/hello/{name}', function ($request, $response, $args) {
    return $this->view->render($response, 'profile.html', [
        'name' => $args['name']
    ]);
    })->setName('profile');
    // Run app
    $app->run();

For Performance tuning you can cache your templates using the caching engine you get from Slim Framework. So harness the power of  Caching Engine.

## Why-choose-Slim-Framework

Slim Framework is not like other MVC Full-Fleged Framework for Development which covers Security and Language Recommendations. But 
> At its core, Slim is a dispatcher that receives an HTTP request, invokes an appropriate callback routine, and returns an HTTP response. That’s it.


Learn to Setup and Design a Sample App (REST API): [Sample REST API App](https://www.ibm.com/developerworks/library/x-slim-rest/)

Even Though you can do anything with Slim Framework but prefer Slim Framework for Static Websites and Prototyping of your project, here are some of its features and advantages:

> 1. HTTP Router
> 2. Middleware
> 3. PSR-7 Support
> 4. Dependency Injection
> 5. Template rendering
> 6. HTTP Caching
> 7. Flash messages
> 8. Encrypted cookies
> 9. Logging
> 10. Error handling and debugging
> 11. Slack Integration
> 12. Create REST applications

Find Some More Amazing Features and Advantages of Slim Framework : [Slim Framework 3](https://www.agriya.com/blog/2016/10/25/new-amazing-features-and-advantages-slim-framework/)

### References
* [Slim Documentation](https://www.slimframework.com/docs/)
* [Github Best Pracrices](https://github.com/xssc/awesome-slim)
* [Sample Slim App](https://www.reddit.com)
* [IBM Developers](https://www.ibm.com/developerworks)