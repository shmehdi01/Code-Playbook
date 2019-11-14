Welcome to the RESTful-API-Design wiki!
 
## Introduction

RESTful web services are basically **REST**(Representational State Transfer) Architectural Design for Web Services. RESTful web services are highly scalable, maintainable, and effective for building web services. Many fortune companies implemented REST API for their application integration. RESTful services used in many open source application like OpenStack. RESTful web services is basically an implementation of HTTP request, response dispatcher. 

### Table of Contents
1) [Introduction](#wiki-introduction)
2) [Six Design Rules for REST API](#six-design-rules-for-rest-api)
3) [How to Write Routes for REST API](#how-to-write-routes-for-rest-api)
4) [Test you REST API with CURL](#test-you-rest-api-with-curl)


Almost every framework written in any language has an inbuilt support for REST API. REST API is based on Client-Server architecture. The Client sends a request to use server resources and get services from REST Server. REST Client after receiving response presents it.

Now we know that REST works only for one **TCP**(Transmission Control Protocol) **HTTP**(Hypertext Transfer Protocol).
But HTTP Protocol has various Request-Response Method. List of some HTTP(S) Method.

| Method  | Action                                                         |
| ------- | -------------------------------------------------------------- |
| GET     | Provides a read only access to a resource.                     |
| POST    | Used to update an existing resource or create a new resource.  |
| PUT     | Used to create a new resource.                                 |
| OPTIONS | Used to get the supported operations on a resource.            |
| DELETE  | Used to remove a resource.                                     |
| HEAD    | identical response that corresponds to a GET request.           |

### Example of Creating RESTful web services.
The endpoint is handled by REST server by various patterns and regular expression. In this example, we will demonstrate how to these endpoints response to which type of method request received. You can respond as you like you content-type. 

List of routes and their corresponding response returned.

|  Routes              | Load View                                            |
|----------------------|------------------------------------------------------|
|   /(root)            | home.html                                            |
|  /login              | login.html                                           |
| /register            | register.html                                        |
| /auth/login          | profile.html(session['email'] parameter passed)      |
| /auth/register       | profile.html(data inserted to DB and Created Session)| 
| /blogs/<user_id>     | user_blog.html                                       |
| /blogs/new           | If GET method new_blog.hml insert the new_blog       |
| /posts/<post id>     | posts.html with arguments passed                     |
| /posts/new/<blog_id> | if GET new_blog.html else insert the new blog        |

REST API provides web services that work for your web application, Android, IOS and other application that have the functionality to consume web services. These web services are consumed by languages that support HTTP request and response. Like Android core programming language 

## Six-Design-Rules-for-REST-API

* **Client-Server**: There should be a separation between the server that offers a service, and the client that consumes it.
* **Stateless**: Each request from a client must contain all the information required by the server to carry out the request. In other words, the server cannot store information provided by the client in one request and use it in another request.
* **Cacheable**: The server must indicate to the client if requests can be cached or not.
* **Layered System**: Communication between a client and a server should be standardized in such a way that allows intermediaries to respond to requests instead of the end server, without the client having to do anything different.
* **Uniform Interface**: The method of communication between a client and a server must be uniform.
* **Code on demand**: Servers can provide executable code or scripts for clients to execute in their context. This constraint is the only one that is optional.

## How to Write Routes for REST API

You can write routes in any Programming and Scripting Language that supports server resource allocation management and any web server for Request and Response handler. We will create a root route for a Web Application using Python's Framework Flask.

**app.py**

    #!flask/bin/python
    from flask import Flask
    
    app = Flask(__name__)

    @app.route('/')
    def index():
       return "Hello, World!"

    if __name__ == '__main__':
       app.run(debug=True)

Flask instance is created and from a decorator, the routes for that web application is designed. Here `/`  is the root of the public_html directory. You can check the working of this route from your browser `localhost:5000`. 
To understand the Query String based and Segment based URL's. You need to understand the URL. 
`http(s)://[username](optional).[hostname].domain-tld/[segments or query string]`

Now the Protocol can be HTTP or HTTP secure for transmitting data securely by encrypting the traffic by 128 or 256-bit key encryption. Username is basically sub-domain which you configure in your DNS server and hostname is the host machine which holds resources.

We have to execute your `app.py` script 

    $ chmod a+x app.py
    $ ./app.py
    # Running on http://127.0.0.1:5000/
    # Restarting with reloader

If you want your endpoint to accept /product(: num) then use a regular expression and pre-defined pattern provided by the flask.
The router of flask this routing of URL for you. You can have your own custom handler for Error Pages(404,301,501, others).

## Test you REST API with cURL 

cURL is a Linux utility which helps you to make UDP/TCP Protocols request and track their responses. To test your rest api cURL is integrated with many Programming and Scripting Languages. We will demonstrate you how to test your REST API using PHP cURL. For the Testing framework, we are using PHPUnit which is highly recommended for Unit Testing.

    <?php
	use PHPUnit\Framework\TestCase;
	require_once('config/config.php');
	/**
	* Class Helpers Stores all the Methods that need to be called for almost each endpoints with different parameter
	* The Result returned by the methods of Helpers with be asserted by endpoint class Method
	*/
	# Its better to divide function for Different Request type
	class Helper 
	{
		var $info = array();
		# Create class variables here
		public function checkHTTPStatus($url)
		{
			# First Create CURL Header and Intialize it
			$ch = curl_init();
			curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
			curl_setopt($ch,CURLOPT_URL,$url);
			curl_exec($ch);
			$this->info = curl_getinfo($ch);
			return intval($this->info['http_code']);
		}
		# Now Check Content Type of Each endpoint and Also Content-Length(Optional)
		public function checkHTTPType()
		{		
			return $this->info['content_type'];
		}
	}
	class EndPoints extends TestCase
	{
		 # create an instance of helper class
		 # Remember you cannot initiate a class outside of a function. that due to scope restriction in php 
		 private $systemc;
		 public function testRest()
 		 {
			$this->systemc = new Helper;
			# read the Endpoint file and check 
			# here just a sample
			# Check Login
			 $this->assertEquals(200,$this->systemc->checkHTTPStatus(URL));
			 $this->assertEquals('text/html; charset=UTF-8',$this->systemc->checkHTTPType());
	                 $this->assertEquals(200,$this->systemc->checkHTTPStatus(URL.'/login'));
                         $this->assertEquals(200,$this->systemc->checkHTTPStatus(URL.'/register'));
		 }
	}
    ?>

In this PHP Script, we have chosen two endpoints. In which the first one is Root and another one is /login and /register with GET Method. cURL default method is GET. we are testing whether the endpoints exist or not by receiving 200 HTTP status code. And you can track the response returned by your web application using the curl_setopt() function to set the request parameter.

## Our Standard:

auth

    /token/from_oauth1

DESCRIPTION

    Creates an OAuth 2.0 access token from the supplied OAuth 1.0 access token.

URL STRUCTURE

    https://api.domain.com/2/auth/token/from_oauth1

AUTHENTICATION

    App Authentication 

PARAMETERS

    {

       "oauth1_token": "qievr8hamyg6ndck",

       "oauth1_token_secret": "qomoftv0472git7"

    }

RETURNS

   {

       "oauth2_token": "9mCrkS7BIdAAAAAAAAAAHHS0TsSnpYvKQVtKdBnN5IuzhYOGblSgTcHgBFKFMmFn"

   }

ERRORS

    Example: invalid_oauth1_token_info

    {

       "error_summary": "invalid_oauth1_token_info/...",

       "error": {

           ".tag": "invalid_oauth1_token_info"

      }

    }

REST Endpoints:

    /public/branches/

Response 

    {

       "status": "success",

       "description": "Here is the endpoint meaning",

       "result_key": "fuzzy data",

       "result_key": "fuzzy data"

    }

Error

    {

       "status": "failure",

       "description": "Here is the endpoint meaning",

       "error_code": "404 | 401 | 403"

    }

**Note**: JSON keys should be a lowercase letter and should follow snake_case.

References 
* [Design Guidelines](https://stackoverflow.com/)
* [RESTful web Services](https://www.tutorialspoint.com/restful/)
* [Sample App REST Server-Client](http://phppot.com/php/php-restful-web-service/)
* [Apps Integration with REST API](https://spring.io/guides/gs/rest-service/) 