### Welcome to the CodeIgniter-Playbook wiki!

## Introduction

CodeIgniter is a Powerful PHP Framework based on MVC(Model-View-Controller) design pattern. There are various other powerful framework on PHP like Laravel, CakePHP. But there are not application specific. They contains a lot of code that might not being utilised by your Project. At the time of writing CodeIgniter version 3.1.4 is released. Also on every new release of any framework bugs are fixed and new features were added for Rapid Application Development(RAD) click changelog link below to find all new features and bug fixes.

### Table of Contents

1) [Introduction](#wiki-introduction)
2) [Best Practices](#wiki-best-practices)
3) [CodeIgniter URL](#wiki-codeigniter-url)
4) [Doctrine](#wiki-doctrine)
5) [Hierarchical Model View Controller](#wiki-hierarchical-model-view-controller)
6) [HTTP Caching in CodeIgniter](#wiki-http-caching-in-codeigniter)
7) [REST API with CodeIgniter](#wiki-rest-api-with-codeigniter)
8) [Routes](#wiki-routes)
9) [Smarty Template Rendering Engine](#wiki-smarty-template-rendering-engine)
10) [TroubleShooting](#wiki-troubleshooting)
11) [Why choose CodeIgniter](#wiki-why-choose-codeigniter)



**Changelog for CodeIgniter version**

[CodeIgniter Official ChangeLog](https://www.codeigniter.com/userguide3/changelog.html)

## **Server Requirements**

PHP version 5.6 or newer is recommended.
It should work on 5.4.8 as well, but we strongly advise you NOT to run such old versions of PHP, because of potential security and performance issues, as well as missing features.

## **Setup Server for CodeIgniter**
Operating System Centos 7
Install Apache Server for Serving web Traffic:

`sudo clean all # Clean all repositories`

`sudo yum -y update # Update Repositories /etc/yum.repos.d`

`sudo yum -y install httpd`   # install apache web server from repositories

`sudo systemctl start httpd`  # Start httpd web service on Default port 80 and 443 

`sudo systemctl enable httpd` # Enable the Apache Web Server on boot. alternate: chkconfig httpd on

`sudo systemctl status httpd` # Check whether Apache Web Server is Running 

Mod_Rewite module in apache web server is used for URL manipulation, It is loaded on every start of httpd service. Once the mod_rewrite module has been activated, you can set up your URL rewrites by creating an .htaccess file in your default document root directory.
If Mod_rewrite Module is enabled by default. To Enable manually then edit 00-base.conf file located in /etc/httpd/conf.modules.d/ directory.

`sudo nano /etc/httpd/conf.modules.d/00-base.conf`

Add or uncomment the following line:

`LoadModule rewrite_module modules/mod_rewrite.so`

Save and close the file, then restart the httpd service:

`sudo systemctl restart httpd`

Now Apache Web Server is Installed our demo app needs MariaDB (MySQL fork):

`yum install MariaDB-server MariaDB-client -y`

`systemctl start mariadb`

`systemctl enable mariadb`

`systemctl status mariadb`

`mysql_secure_installation` # Setup your MySQL defaults

Finally to Complete our Lamp Stack install PHP and Restart web server

`rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`

`rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`

You can alternatively install PHP 5.6’s php-fpm SAPI (along with an opcode cache by doing:

`yum install php56w-fpm php56w-opcache`

`systemctl restart httpd`

## Dependency Management in PHP
Composer is a tool for dependency management in PHP. It allows you to declare the libraries your project depends on and it will manage (install/update) them for you.
Install Composer First:

`sudo yum -y update`  # Update your system repositories

`cd /tmp`  # Change your current working directory to /tmp directory

`sudo curl -sS https://getcomposer.org/installer | php`  # Get composer script from cURL and execute it with php cli

`mv composer.phar /usr/local/bin/composer`  # Move the composer file to local user bin directory 

Install CodeIgniter and PHPUnit from Composer:

`composer require --dev codeigniter/framework`

`composer require --dev phpunit/phpunit ^6.1`

Composer.json file looks like

    {
	"description": "The CodeIgniter framework",
	"name": "codeigniter/framework",
	"type": "project",
	"homepage": "https://codeigniter.com",
	"license": "MIT",
        "support": {
		"forum": "http://forum.codeigniter.com/",
		"wiki": "https://github.com/bcit-ci/CodeIgniter/wiki",
		"irc": "irc://irc.freenode.net/codeigniter",
		"source": "https://github.com/bcit-ci/CodeIgniter"
	},
	"require": {
		"php": ">=5.6.1"
	},
	"suggest": {
		"paragonie/random_compat": "Provides better randomness in PHP 5.x"
	},
	"require-dev": {
		"mikey179/vfsStream": "1.1.*",
		"phpunit/phpunit": "4.* || 5.*"
	}
    }

You can also run your CodeIgniter framework from PHP local server:

`php -S localhost:8888 index.php`

After configuring your Server and Composer. Your are ready to execute your scripts.

## Best-Practices

Follow Below Practices to Design Effective, Maintainable  and Reliable code.

Key Points when you Design and Develop Web Application using CodeIgniter.

1) [Creating Barebones Setup](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/create-a-barebones-setup)
2) [Using Third Party Libraries](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/using-third-party-libraries)
3) [Using Composer](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/using-third-party-libraries)
4) [Creating Project Templates](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/creating-the-project-template)
5) [Code for the Future](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/code-for-the-future)
6) [Debug Fast and Hard](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/debug-fast-and-hard)
7) [Extend the Controller](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/extend-the-controllers)
8) [Thin controllers Fat Model](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/thin-controllers-fat-models)
9) [Some More Controllers](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/some-more-controllers)
10) [Security Aspects](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/security)
11) [Working With Sessions](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/working-with-sessions)
12) [CodeIngiter Caching](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/codeigniter-caching)
13) [Zend Cache](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/zend_cache)
14) [Make it Modular](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/make-it-modular)
15) [Migrations and the Terminal](https://code.tutsplus.com/courses/codeigniter-best-practices/lessons/migrations-and-the-terminal)
16) [General Tips and Tricks](https://code.tutsplus.com/courses/codeigniter-best-practices)


## CodeIgniter-URL
 
CodeIgniter URL's are designed in such a way that it can be easily crawlable by search engines and also user-friendly.
CodeIgniter didn't use "Query String".

(Eg: [https://www.google.co.in/?gfe_rd=cr&ei=0_8kWYmHG5DT8gep9YH4Bw#q=CodeIgniter](https://www.google.co.in/?gfe_rd=cr&ei=0_8kWYmHG5DT8gep9YH4Bw#q=CodeIgniter) ). 

To retrieve this arguments you need yo use PHP Global Variables `$_GET['param_name']` or `$_REQUEST['param_name']`. But "Query String" is not user friendly also difficult to crawl by websites. CodeIgniter use Segment-Based approach.

(Eg: [https://www.amazon.com/product/id/123](https://www.amazon.com/product/id/123))

CodeIgniter uses `$_SERVER['REQUEST_URI']` to retrieve all details after hostname. To retrieve query string from this url use
`parse_url()` function which maps different url fields. To parse Query String use `parse_str()` function. 

Now Entry point of every MVC framework handled by index.php file which looks at the route and if there exists a class and function then they are handled by autoload. Otherwise 404 not found error is encountered.

To Accomplish it use .htaccess:

    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php/ [L]

Which request index.php file on every request and controller performs action for corresponding URL.

### CodeIgniter URL Structure

Now the url handled by CodeIgniter is in stream based URL.

http://www.crownstack.com/[class-name]/[function-name]/arguments/arguments
CodeIgniter first autoload the class [class-name] if found using `class_exists([class-name])` and then looks for the function [function-name] using `function_exists([function-name])`. when method got dispatched then `funct_nums_args()` is called to get num of arguments passed and retrieve it by using `func_get_args()`. 

http://www.crownstack.com/[class-name]/
CodeIgniter first autoload class and then looks for `index()` function in that class.

You can also call magic method _remap() if you want to handle argument passed after class name in URL. 
 
CodeIgniter allows you to use "Query String" change the config variable for url.

## Doctrine

Doctrine is a Powerful ORM(Object Relational Mapping) in PHP and it can be easily integrated with CodeIgniter.
Doctrine is also easy to install from composer just add below lines to composer.json file.

    "require": { "doctrine/orm": "dev-master" }

ORM helps to minimize the complexity of SQL query. But there is no inbuilt support of ORM in CodeIgniter framework.

Now, in ‘application/libraries’ directory, lets create a new class file named ‘doctrine.php’ that will include all initialization of our doctrine library for our CodeIgniter application. The class is as follows:

    use Doctrine\Common\ClassLoader,
       Doctrine\ORM\Configuration,
       Doctrine\ORM\EntityManager,
       Doctrine\Common\Cache\ArrayCache,
       Doctrine\DBAL\Logging\EchoSQLLogger,
       Doctrine\ORM\Mapping\Driver\DatabaseDriver,
       Doctrine\ORM\Tools\DisconnectedClassMetadataFactory,
       Doctrine\ORM\Tools\EntityGenerator;
    /**
    CodeIgniter Smarty Class
    initializes basic doctrine settings and act as doctrine object
    @final   Doctrine
    @category    Libraries
    @author  Md. Ali Ahsan Rana
    @link    http://codesamplez.com/
    */
    class Doctrine {
    /** @var EntityManager $em  */
    public $em = null;
    /** constructor  */
    public function __construct()
    {
    // load database configuration from CodeIgniter
    require APPPATH.'config/database.php';
    // Set up class loading. You could use different autoloaders, provided by your favorite framework,
    // if you want to.
    require_once APPPATH.'third_party/Doctrine/Common/ClassLoader.php';
    $doctrineClassLoader = new ClassLoader('Doctrine',  APPPATH.'third_party');
    $doctrineClassLoader->register();
    $entitiesClassLoader = new ClassLoader('models', rtrim(APPPATH, "/" ));
    $entitiesClassLoader->register();
    $proxiesClassLoader = new ClassLoader('proxies', APPPATH.'models');
    $proxiesClassLoader->register();
    // Set up caches
    $config = new Configuration;
    $cache = new ArrayCache;
    $config->setMetadataCacheImpl($cache);
    $driverImpl = $config->newDefaultAnnotationDriver(array(APPPATH.'models/Entities'));
    $config->setMetadataDriverImpl($driverImpl);
    $config->setQueryCacheImpl($cache);
    // Proxy configuration
    $config->setProxyDir(APPPATH.'models/proxies');
    $config->setProxyNamespace('Proxies');
    // Set up logger
    //$logger = new EchoSQLLogger;
    //$config->setSQLLogger($logger);
    $config->setAutoGenerateProxyClasses( TRUE );   
    // Database connection information
    $connectionOptions = array(
        'driver' => 'pdo_mysql',
        'user' =>     $db['default']['username'],
        'password' => $db['default']['password'],
        'host' =>     $db['default']['hostname'],
        'dbname' =>   $db['default']['database']
       );
      // Create EntityManager
      $this->em = EntityManager::create($connectionOptions, $config);  
      }
    }

When your project grows use namespace to avoid conflicts between variable names. PHP namespace provides the flexibility of filesystem.

## Hierarchical-Model-View-Controller

HMVC is a Modular Extension which helps you develop modules. Modules can be third party library or integration of some widgets which simplifies your code in large project. 
![Hierarchical Model View Controller](https://madetech.s3.amazonaws.com/sir_trevor_images/images/000/000/053/original/MVC-HMVC.png?1412158722)
> Image Credit Madetech

HMVC Most useful when you are developing project that requires many third party libraries and various other stuff. That can make your code look massive and difficult to maintain. But HMVC provides modularity so even if you want to upgrade a particular third party module you need not to make changes to that third party module directory. This is also best practice in Software Engineering. The Project should have low coupling and high cohesion. 

### File Directory Structure:

![File Directory Structure for CodeIgniter](http://quizld.com/wp-content/uploads/2012/05/HMVC.png)
> Image Credit Quizld

![Example of HMVC Project](http://quizld.com/wp-content/uploads/2012/05/HMVC-Example1.png)
> Image Credit Quizld

## HTTP-Caching-in-CodeIgniter

How Caching Works

![How Caching Works](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/images/http-cache-decision-tree.png)

Find how HTTP Cache Header Looks Like [PHP Code Snippet](https://stackoverflow.com/questions/1971721/how-to-use-http-cache-headers-with-php)

Caching your pages improves the performance of your website and saves your resources (Like RAM, CPU Cycles and Disk IO).
You can cache each page and these pages are stored in application/cache directory. You have to set a time when this cache data expires or also you can delete any cache page explicitly using

    // Deletes cache for the currently requested URI
    $this->output->delete_cache();

    // Deletes cache for /foo/bar
    $this->output->delete_cache('/foo/bar');

When you render a template then all your dynamic data is loaded into template. Next time when accessed they need not to render this dynamic data again. these rendered template will be stored in Cache and loaded from cache from user's browser.
When cache is expired then they are requested by server.

Caching saves your RAM, CPU Cycles and Disk IO from server perspective. And Improves Page Load Speed. 

## REST-API-with-CodeIgniter

REST API can be built with CodeIgniter.

![REST API](http://networkop.co.uk/images/rest-crud.png)
> Image Credit Networkop.co.uk

You need to define all your endpoint for your rest api on $route associative array with method. 

`$route['product/endpoint']['POST'] = 'class/method'`

`$route['shop/endpoint']['DELETE'] = 'class/method'`

Now if you want to return json then first stringify JSON (JSON Serialization) for transfer over internet and the the content-type information in http header using php header() function

`header('content-type: application/json')`

To define your endpoints and need regular expression read our route pages to understand more about routes.

Create REST API to return user information

Now REST Client will make request to endpoint

URL :- **http://crownstack.com/users/ashok_fins**

Request Method :- **GET**

**Return JSON result :-**


    {
    "name": "Ashok Mehta",
    "age": 22,
    "father_name" : "Sri Ramesh Mehta",
    "descendants" : ["Sonu Mehta","Ankita Mehta","Ritu Mehta"],
    "DOB" : "24/04/1991 12:10:44",
    "IP" : "192.144.55.32"
    }

**To accomplish this example simply use Callback function**

    $route['users/(:any)'] = function($user_id)
    {
    header('content-type: application/json');
    $query = $this->db->query("select * from users where user_id = '$user_id'");
    foreach($query->result_array() as $row)
    {
        $result["name"] = $row["name"];
        $result["age"] = $row["age"];
        $result["father_name"] = $row["father_name"];
        $result["descendants"] = $row["descendants"];
        $result["DOB"] = $row["DOB"];
        $result["IP"] = $row["IP"];
    }
    return json_encode($result);
    };

## Routes

CodeIgniter supports regular expression in their routes and some are in built regexes.

### Application flow Chart
![Application Flowchart](https://www.codeigniter.com/user_guide/_images/appflowchart.gif)
> Image Credit CodeIgniter Official

1. The index.php serves as the front controller, initializing the base resources needed to run CodeIgniter.
2. The Router examines the HTTP request to determine what should be done with it.
3. If a cache file exists, it is sent directly to the browser, bypassing the normal system execution.
4. Security. Before the application controller is loaded, the HTTP request and any user submitted data is filtered for security.
5. The Controller loads the model, core libraries, helpers, and any other resources needed to process the specific request.
6. The finalized View is rendered then sent to the web browser to be seen. If caching is enabled, the view is cached first so that on subsequent requests it can be served.

### Few ways to write routes

`$route['product/:num'] = 'catalog/product_lookup';`

In this code snippet the URI product/[any number] when hit, CodeIgniter route will load product_lookup in catalog class. 
But you want the autoload to work as default to don't define the URL in $route associative array.
(:num) will match a segment containing only numbers. (:any) will match a segment containing any character (except for ‘/’, which is the segment delimiter). You can also use regular expression instead of pre-created regexes.

`$route['products/([a-z]+)/(\d+)'] = '$1/id_$2';`

The first segment contain products/accessories/123  then the mapped route will be accessories class and id_123 method.

You can also define callbacks.

    $route['products/([a-zA-Z]+)/edit/(\d+)'] = function ($product_type, $id)
    {
        return 'catalog/product_edit/' . strtolower($product_type) . '/' . $id;
    };

When products/Shop/edit/123 is called then the closure is executed (Anonymous function). You can also define verbs in http which means you can execute function when a HTTP Method is encountered.

`$route['product/edit']['PUT']  = 'product/update'`

`$route['product/push']['GET'] = 'product/process'`

These are the standard ways PSR 7 (PHP Standard Recommendation) compliant.

## Smarty-Template-Rendering-Engine

Smarty is one of the most popular template rendering engine comes with cache engine to improve performance. Template Rendering engine works on Presentation layer to Provide User Interface and hold data passed to it by controller. It also follow layered architecture. Which means you can create header.tpl, footer.tpl, chat.tpl and load them when required with a template. There are different available template engine like twig, php-view.

Compare Smarty Template Rendering Engine with others
[Click to Compare Template Engine](http://www.onextrapixel.com/2010/07/06/template-engine-an-overview-of-smarty-templates-other-comparsions)

Download and Install Smarty
Official Website: [Visit Smarty ](http://www.smarty.net/)

Now Put the download files in Third Party Library module and perform these changes.

### Define Cache, Root, Config Directory 

    if (!defined('BASEPATH')) exit('No direct script access allowed');
    require_once(APPPATH.'libraries/smarty/Smarty.class.php');
    /**
    * MySmarty Class
    * initializes basic smarty settings and act as smarty object
    * @final Mysmarty 
    * @category Libraries
    * @author  Md. Ali Ahsan Rana
    * @link    http://codesamplez.com/
    */
    class Mysmarty extends Smarty
    {
    /** constructor  */
    function __construct()
      {
        parent::__construct();
        $this->template_dir = APPPATH."/views/";
        $this->config_dir = APPPATH."/conf/";
        $this->compile_dir = APPPATH."/cache/";
        $this->caching = 0;       `
      }
    }

Smarty uses .tpl files for templates.  Passing information to template using Smarty object assign function. Assign function takes two arguments.First argument is template variable name and second argument is Original Data/PHP Variable. And Display method is used to load the template with variables.

    // create an smarty object of our customized class
    $smarty = new Mysmarty ;
    // basic assignment for passing data to template file
    $smarty->assign('title', 'Test Title');
    $smarty->assign('description', 'Test Description');
    // show the template
    $smarty->display('index.tpl');

### Index.tpl File
    <html>
    <head>
    <title>Info</title>
    </head>
    <body>
    Test Article:
    Title: {$title}
    Description: {$description}
    </body>
    </html>

If your template have variable means no control structure need to be performed then type simply `{$variable_name}`. But to use control structure use 

    {foreach $articles as $article}
    <h2>{$article->title}</h2>
    <p>{$article->description}</p>
    {/foreach}

## TroubleShooting

List of Issues and their Solutions in CodeIgniter


## Why-choose-CodeIgniter

The Top Five Reasons why CodeIgniter is Awesome for your Project.

### MVC Architecture
CodeIgniter is built in MVC(Model View Controller) Design pattern. MVC gives the flexibility to maintain and manage your codebase very effectively. As Business logic and UI/UX and their REquest handling all is performed by different individuals of MVC Design pattern. Many popular Web Application using MVC Pattern (OpenCart, Prestashop, Magento).

### Easy PHP Version Migration
CodeIgniter supports both php 5 and 7 without making any changes to the server. this helps the developer to upgrade their CodeIgniter framework without making any changes and putting efforts for debugging.

### Light Weight Application Specific
CodeIgniter comes with limited capability that is used by every web application project. You can define your own controller class. Define your own routes with regular expression capability. Which means CodeIgniter highly customizable capability to developer to create fast and efficient web application.

### Easy to Install and Configure
You can use composer to install CodeIgniter,PHPUnit, Smarty and other stuffs and manage dependency. You need to configure base URL and other database credentials to connect with database. There are more than 10 popular database drivers available.

### Database Abstraction and ORM Integration
CodeIgniter framework allow to execute CRUD operation on database without need to write raw SQL query. Handle multiple databases connection. Easily integrate with ORM (Doctrine or NotORM).  It supports almost all popular databases (MySQL, Oracle, PostgreSQL, MSSQL, SQLite).
 
### References 
1) https://www.codeigniter.com/
2) https://stackoverflow.com/
3) https://www.udemy.com
4) https://www.tutorialspoint.com/
