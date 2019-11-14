### Welcome to the Python-Flask-Playbook wiki!

## Introduction

Flask is written in Python Programming Language. 
Flask is a MicroFramework based on Werkzeug and Jinja2. It is based on BSD (Berkley Software Distribution) License. 

## Table of Contents

1) [Introduction](#wiki-introduction)
2) [Best Practices](#wiki-best-practices)
3) [Flask-URL](#wiki-flask-url)
4) [How to Structure Large Flask Application](#wiki-how-to-structure-large-flask-application)
5) [HTTP Caching in Flask](#wiki-http-caching-in-flask)
6) [Jinja2 Template Rendering Engine](#wiki-jinja2-template-rendering-engine)
7) [ORM SQLAlchemy](#wiki-orm-sqlalchemy)
8) [REST API with Flask](#wiki-rest-api-with-flask)
9) [Routes](#wiki-routes)
10) [TroubleShooting](#wiki-troubleshooting)
11) [Why Choose Flask](#wiki-why-choose-flask)

Read Documentation of Python Flask [Official Documentation ](http://flask.pocoo.org/docs/0.12/)

Download Source Code of Python Flask [Download Source](https://pypi.python.org/packages/4b/3a/4c20183df155dd2e39168e35d53a388efb384a512ca6c73001d8292c094a/Flask-0.12.tar.gz#md5=c1d30f51cff4a38f9454b23328a15c5a)

### Setup CentOS Operating System

As we know Flask is written in python we need to Install python

    cd /root
    yum install python 
    # To install other extension use  Command: yum list python*
    # After python is installed then install pip
    yum install python-pip
    # Now Install flask using pip installer
    pip install flask

Now you need database to store persistent data. Flask supports a Relational, Graphical and NoSQL database effectively.

    yum install mariadb-server mariadb-client
    systemctl start mariadb
    systemctl enable mariadb
    mysql_secure_installation
    # Complete installation steps


### Web Server

If you want to setup your Python Flask for Development purpose then use Flask Internal Server
Otherwise need a web server for Production use these steps [Install uWSGI and Nginx](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-centos-7)


### Getting started with Flask

You need to import Flask class and Create an Instance of your flask app. You can explicitly define your Hostname and Port address. Which means you have have your own socket programmed for you by Fask.

    from flask import Flask
    application = Flask(__name__)
    @application.route("/")
       def hello():
       return "<h1 style='color:blue'>Hello There!</h1>"

    if __name__ == "__main__":
       application.run(host='0.0.0.0')

Run your Project from Command Line : python filename.py

### Integration with Third Party Libraries

Now you can integrate your Project with various Libraries. The Best way to Installing and Configuring your Libraries using PIP. which plays role dependency management. Some third party library for your project
(IF you are using PyCharm IDE then create requirements.txt and copy below Packages)

    FLASK == 0.12.1
    requests==2.13.0
    beautifulsoup4==4.5.3
    pymongo==3.4.0

## Best-Practices

Here we cover Scalability, Modularity, Optimizations, many more.

1) [Full Stack Python](https://www.fullstackpython.com/flask.html)
2) [Install and Configure Packages with PIP](https://pip.pypa.io/en/stable/)
3) [Python Integration Forms](https://flask-wtf.readthedocs.org/en/latest/index.html)
4) [Flask Cache](http://pythonhosted.org/Flask-Cache/)
4) [ORM For your Project](http://pythonhosted.org/Flask-SQLAlchemy/)
5) [RESTful Services with Flask](https://flask-restful.readthedocs.io/en/0.3.5/)
6) [Cache Pages with Flask](https://pythonhosted.org/Flask-Cache/)
7) [Large Flask Application](https://www.digitalocean.com/community/tutorials/how-to-structure-large-flask-applications)
8) [Continous Integration with Flask](http://bango29.com/continuous-web-development/)
9) [Flask Assets for View](http://flask-assets.readthedocs.io/en/latest/)
10) [Handle Session Securely](https://stackoverflow.com/questions/20314921/how-to-properly-and-securely-handle-cookies-and-sessions-in-pythons-flask)

## Flask-URL

Flask Uses Segment based URL(Uniform Resource Locator). Writing your endpoints should be User Friendly and Search Engine Friendly. Flaks gives enormous power to write routes which regular expression. 

Design Technique to build such URL is Slug (hostname/how-to-access-logs).

Understand below code snippet to Generate slug for your project. Good Practice from Search Engine Optimization(SEO) point of view is use your keywords. 

    import re
    _punct_re = re.compile(r'[\t !"#$%&\'()*\-/<=>?@\[\\\]^_`{|},.]+')
    def slugify(text, delim=u'-'):
        """Generates an ASCII-only slug."""
        result = []
        for word in _punct_re.split(text.lower()):
        word = word.encode('translit/long')
        if word:
            result.append(word)
    return unicode(delim.join(result)) 

> Code From Jason Kirkland's 

RE (It is Python Regular Expression to write patterns).

Now if you want to generate a URL with Payload.Now If you want to send a URL to user in order to activate its account. Then Understand below code snippet to accomplish your task. And that URL can be used once.

One way is to use the cryptographic signing request with the help of [Dangerous](http://pythonhosted.org/itsdangerous/) Module.

    from flask import abort, redirect, flash
    from itsdangerous import URLSafeSerializer, BadSignature
    def get_serializer(secret_key=None):
       if secret_key is None:
         secret_key = app.secret_key
       return URLSafeSerializer(secret_key)
    
    @app.route('/users/activate/<payload>')
    def activate_user(payload):
       s = get_serializer()
       try:
         user_id = s.loads(payload)
       except BadSignature:
         abort(404)
       user = User.query.get_or_404(user_id)
       user.activate()
       flash('User activated')
       return redirect(url_for('index'))


So, how to generate URL

    def get_activation_link(user):
       s = get_serializer()
       payload = s.dumps(user.id)
       return url_for('activate_user', payload=payload, _external=True)

The URL generated will look something like this: http://example.com/users/activate/NDI.qufkoGPGURs0UuFTludpcHLKa20

The Above link can be used multiple times now we want to restrict the URL to only once triggered it is non-usable next time when triggered you can return any custom message that URL is expired

    @app.route('/voucher/redeem/<payload>')
    def redeem_voucher(payload):
       s = get_serializer()
       try:
           user_id, voucher_id = s.loads(payload)
       except BadSignature:
           abort(404)
           user = User.query.get_or_404(user_id)
           voucher = Voucher.query.get_or_404(voucher_id)
           voucher.redeem_for(user)
           flash('Voucher redeemed')
           return redirect(url_for('index'))

    def get_redeem_link(user, voucher):
       s = get_serializer()
       payload = s.dumps([user.id, voucher.id])
       return url_for('redeem_voucher', payload=payload,_external=True)

> Code Credit : Armin Ronacher 

## How-to-Structure-Large-Flask-Application

To Build large application with flask the best way to accomplish is use tools that you are comfortable. The most common (and sensible) of choices in terms of extensions and libraries (i.e. database extraction layer). They avoid complex SQL queries and provides you an abstraction layer.
These choices will involve:

> SQLAlchemy (via Flask-SQLAlchemy)

> WTForms (via Flask-WTF)

### Flask-SQLAlchemy

Adds SQLAlchemy support to Flask. Quick and easy.

This is an approved extension.

    Author: Armin Ronacher
    PyPI Page: Flask-SQLAlchemy
    Documentation: Read docs @ packages.python.org
    On Github: [mitsuhiko/flask-sqlalchemy](https://github.com/mitsuhiko/flask-sqlalchemy)

### Flask-WTF

Flask-WTF offers simple integration with WTForms. This integration includes optional CSRF handling for greater security.

This is an approved extension.

    Author: Anthony Ford (created by Dan Jacob)
    PyPI Page: Flask-WTF
    Documentation: Read docs @ packages.python.org
    On Github: [ajford/flask-wtf](https://github.com/mitsuhiko/flask-wtf)

### FileSystem Structure

Organize your files in the below directory structure to build Hierarchical Model View Controller(HMVC). 

 	~/LargeApp
		|-- run.py
		|-- config.py
		|__ /env             # Virtual Environment
		|__ /app             # Our Application Module
			 |-- __init__.py
			 |-- /module_one
				 |-- __init__.py
				 |-- controllers.py
				 |-- models.py                
			 |__ /templates
				 |__ /module_one
					 |-- hello.html
			 |__ /static
			 |__ ..
			 |__ .
		|__ ..
		|__ .

Follow the [Digital Ocean tutorial ](https://www.digitalocean.com/community/tutorials/how-to-structure-large-flask-applications) to understand more about to to structure your Model(Fat Model) and Controller(Slim Controller) to make your project Maintable and Scalable.

## HTTP-Caching-in-Flask

First Understand how caching works and how it improve performance by Saving CPU cycles, Main Memory and Disk IO. Also it improves the page speed load time by keeping the already rendered template.

![What is Caching](https://cdn.lynda.com/video/533074-93-636144929085401145_338x600_thumb.jpg)
> Image Credit Lynda.com

Flow Diagram of how cache data is retrieved and manipulated.

![How Caching Works](https://i.stack.imgur.com/4tHhy.png)
> Image Credit StackExchange.com

### Adding Caching in Flask 
Werkzeug includes a number of caching options in the werkzeug.contrib.cache package: [werkzeug.contrib.cache](http://werkzeug.pocoo.org/documentation/dev/contrib/cache.html) handles low-level caching - this is handy for caching individual objects (such as the results of a database query) but not if you want to cache an entire view, or even a whole site.

For a per-view cache we can use a decorator:

	from werkzeug.contrib.cache import SimpleCache
	CACHE_TIMEOUT = 300
	cache = SimpleCache()
	class cached(object):
		def __init__(self, timeout=None):
			self.timeout = timeout or CACHE_TIMEOUT
		def __call__(self, f):
			def decorator(*args, **kwargs):
				response = cache.get(request.path)
				if response is None:
					response = f(*args, **kwargs)
					cache.set(request.path, response, self.timeout)
				return response
			return decorator
	@app.route("/")
	@cached()
	def index():
		return render_template("index.html")

Here we are using SimpleCache Engine but it for production you need Memcached to store your Queries and ResultSet. MemCache is a Caching Engine which stores your Key Value Pairs and you can restrict the Hits(Expire after 50 Hits) or Expire in an Interval of Time. So Memcache gives you enormous power to handle cache data effectively. To Undertand how Memcache works and its Installation Method use [Memcache Documentation](https://memcached.org/)

## Jinja2-Template-Rendering-Engine

Now its time to working on view layer of MVC Design pattern to prove full stack development strength of Flask.
when you requested with an endpoint you can render your template with the dynamic data payload to the Jinja.

Jinja use template inheritance which gives you Modularity (Design your Header.html and Footer.html and extend them when they are needed). 

Put all your jinja2 templates in the /templates folder which will be rendered from this directory. 

**Example Layout.html**

	<!doctype html>
	<title>Flaskr</title>
	<link rel=stylesheet type=text/css href="{{ url_for('static', filename='style.css') }}">
	<div class=page>
	  <h1>Flaskr</h1>
	  <div class=metanav>
	  {% if not session.logged_in %}
		<a href="{{ url_for('login') }}">log in</a>
	  {% else %}
		<a href="{{ url_for('logout') }}">log out</a>
	  {% endif %}
	  </div>
	  {% for message in get_flashed_messages() %}
		<div class=flash>{{ message }}</div>
	  {% endfor %}
	  {% block body %}{% endblock %}
	</div>

This Layout.html contains external style.css file linked with HTML Tag. The session dict is available in the template as well and you can use that to check if the user is logged in or not. Note that in Jinja you can access missing attributes and items of objects / dicts which makes the following code work, even if there is no 'logged_in' key in the session:

Extending this Layout.html in other templates

Example Login.html

	{% extends "layout.html" %}
	{% block body %}
	  <h2>Login</h2>
	  {% if error %}<p class=error><strong>Error:</strong> {{ error }}{% endif %}
	  <form action="{{ url_for('login') }}" method=post>
		<dl>
		  <dt>Username:
		  <dd><input type=text name=username>
		  <dt>Password:
		  <dd><input type=password name=password>
		  <dd><input type=submit value=Login>
		</dl>
	  </form>
	{% endblock %}

If you have used Twig-View Template Rendering Engine in PHP. Then it will be very easy to grab syntax of Jinja2.

These are varoius Method and Keywords used in Jinja2.

Read its Documentation: [Jinja2 Official Documentation](http://jinja.pocoo.org/docs/latest/) 

Download Source Code: [Github Jinja2 Repo](https://github.com/pallets/jinja)

## ORM-SQLAlchemy

Performing CRUD Operation and Writing Complex Query can be Tedious. So a data abstraction layer (ORM: Object Relational Mapping) is integrated to make them Demystifying. 

**What is Flask-SQLAlchemy?**

Flask-SQLAlchemy's functionality can be broken down into several core components:

1) Database session configuration and session management within an HTTP request context
2) Custom signalling events that fire before and after models are committed and after a transaction rollback
3) Custom declarative base model with support for query property and pagination
4) Proxy access from the extension instance to the SQLAlchemy module
5) Other minor conveniences like creating/dropping all models, accessing model metadata, autogenerating table names, and applying driver hacks

**NOTE:** The links to Flask-SQLAlchemy's source code are pinned to the latest commit at the time of this writing.

**Install SQLAlchemy using PIP**

    pip install flask-sqlalchemy

**Import SQLAlchemy class from Module**

    from flask_sqlalchemy import SQLAlchemy

**Now create a Flask application object and set URI for the database to be used.**

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///students.sqlite3'

Understand following Operation in SQLAlchemy

1) [Declarative Base Model](http://derrickgilland.com/posts/demystifying-flask-sqlalchemy/)
2) [Decoupling Flask-SQLAlchemy](http://derrickgilland.com/posts/demystifying-flask-sqlalchemy/)
3) [Creating a Declarative Base Model](http://derrickgilland.com/posts/demystifying-flask-sqlalchemy/)
4) [Creating a Query Class Property](http://derrickgilland.com/posts/demystifying-flask-sqlalchemy/)
5) [Extending Flask-SQLAlchemy](http://derrickgilland.com/posts/demystifying-flask-sqlalchemy/)

## REST-API-with-Flask

### DIY(Do-It-yourself) Approach for Developing REST API from Ground up
REpresentational State Transfer (REST) is a web development architecture design style which refers to logically separating your API resources so as to enable easy access, manipulation and scaling. Reusable components are written in a way so that they can be easily managed via simple and intuitive HTTP requests which can be GET, POST, PUT, PATCH, and DELETE (there can be more, but above are the most commonly used ones).

![REST API](http://networkop.co.uk/images/rest-crud.png)
> Image Credit Networkop.co.uk 

Create Your First REST API : [Follow Tuts](https://code.tutsplus.com/tutorials/building-restful-apis-with-flask-diy--cms-26625)

![REST API](https://lh6.googleusercontent.com/-u9hFxEK0OS8/T6a9yHaniHI/AAAAAAAAF_Y/prEsvdWrNtI/s550/rest.png)

> Image Credit: 9Lessons

**Note: ** .htaccess file is used to write rules for your web server which is an entry point of your application. For flask application it should look like 

    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f # Don't interfere with static files 
    RewriteRule ^(.*)$ /path/to/the/application.cgi/$1 [L]

Let's begin by making a simple script that responds to requests at the root, /articles and /articles/:id.

    from flask import Flask, url_for
    app = Flask(__name__)
    @app.route('/')
    def api_root():
       return 'Welcome'
    @app.route('/articles')
    def api_articles():
       return 'List of ' + url_for('api_articles')
    @app.route('/articles/<articleid>')
    def api_article(articleid):
       return 'You are reading ' + articleid
    if __name__ == '__main__':
       app.run()

**Note: ** flaks Internal server Default Host is Localhost and Port number is 5000. This is only be used for Development purpose for Production use use Nginx or uWSGI Web Server.

**How to test your REST API**

1) For Linux or Mac user use cURL utility from terminal (Helps you analyze your Header)
2) For Windows user Postman google chrome extension is a good option.

## Routes

Routing is an important part of the REST(Representation State Transfer) API. You need to detect endpoint and match with the pattern then only callback function will be executed. Now if you want to write a endpoint that accepts email id.
Now to write an endpoint that accept email id use regular expression to write url patterns and then only enter the closure when they are matched.

You can take optional argument as well use character class. 

	from flask import Flask
	from flask import make_response
	from flask import render_template
	from flask import request
	from flask import session

	from src.Common.database import Database
	from src.Models.blog import Blog
	from src.Models.post import Post
	from src.Models.user import User

	app = Flask(__name__)
	app.secret_key = "jasper@1356"


	@app.route('/')
	def home_template():
		return render_template("home.html")


	@app.route('/login')
	def login_template():
		return render_template("login.html")


	@app.route('/register')
	def register_template():
		return render_template("register.html")


	@app.before_first_request
	def initialize_database():
		Database.initialize()


	@app.route('/auth/login', methods=['POST'])
	def login_user():
		email = request.form['email']
		password = request.form['password']

		if User.login_valid(email, password):
			User.login(email)
		else:
			session['email'] = None
		return render_template('profile.html', email=session['email'])


	@app.route('/auth/register', methods=['POST'])
	def register_user():
		email = request.form['email']
		password = request.form['password']
		User.register(email, password)

		return render_template('profile.html', email=session['email'])


	@app.route('/blogs/<string:user_id>')
	@app.route('/blogs')
	def user_blogs(user_id=None):
		if user_id is not None:
			user = User.get_by_id(user_id)
		else:
			user = User.get_by_email(session['email'])
		blogs = user.get_blogs()

		return render_template("user_blogs.html", blogs=blogs, email=user.email)


	@app.route('/blogs/new', methods=['POST', 'GET'])
	def create_new_blog():
		if request.method == 'GET':
			return render_template("new_blog.html")
		else:
			title = request.form['title']
			description = request.form['description']
			user = User.get_by_email(session['email'])
			new_blog = Blog(user.email, title, description, user._id)
			new_blog.save_to_mongo()
			return make_response(user_blogs(user._id))


	@app.route('/posts/<string:blog_id>')
	def blog_posts(blog_id):
		blog = Blog.from_mongo(blog_id)
		posts = blog.get_posts()

		return render_template('posts.html', posts=posts, blog_title=blog.title, blog_id = blog._id)


	@app.route('/posts/new/<string:blog_id>', methods=['POST', 'GET'])
	def create_new_post(blog_id):
		if request.method == 'GET':
			return render_template("new_post.html", blog_id=blog_id)
		else:
			title = request.form['title']
			content = request.form['content']
			user = User.get_by_email(session['email'])
			new_post = Post(blog_id, title, content, user.email)
			new_post.save_to_mongo()
			return make_response(blog_posts(blog_id))

	if __name__ == '__main__':
		app.run(port=5454)

Run the above scripts when you have all the packages installed and templates files replaced with your custom templates.

    python app.py

When the python script is executed. just enter localhost:5454 on your http client. And Follow the routes we created

You can have same closure to be executed when URL is triggered with different method.
You have root URL /, which will render home.html file. You can also pass the parameter to the home.html file.

List of routes and their corresponding view loaded from templates folder.

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

This is how you can define your own endpoints and make your REST API that can return XML, JSON or HTML. But remember to set header belongs to your http response.

## TroubleShooting

List of Issues and Solutions in Flask

## Why-Choose-Flask

Flask is a "microframework" primarily aimed at small applications with simpler requirements. Pyramid and Django are both aimed at larger applications, but take different approaches to extensibility and flexibility. Pyramid targets flexibility and lets the developer use the right tools for their project. This means the developer can choose the database, URL structure, templating style, and more. Django aims to include all the batteries a web application will need so developers need only open the box and start working, pulling in Django's many modules as they go.

1) [Benefits of Flaks](https://www.airpair.com/python/posts/django-flask-pyramid)
2) [Flaks vs Djnago](https://www.codementor.io/garethdwyer/flask-vs-django-why-flask-might-be-better-4xs7mdf8v)
3) [Flaks Vs Bottle](https://www.codementor.io/garethdwyer/flask-vs-django-why-flask-might-be-better-4xs7mdf8v)
4) [Advantages of Flask](http://www.butenas.com/blog/why-flask/)
5) [When to Use Flask](https://www.quora.com/Why-should-we-use-Flask-for-web-development)
6) [Code Less Get More](http://flask.pocoo.org/docs/0.12/)

### References
1) http://flask.pocoo.org/docs/0.12/
2) https://stackoverflow.com/
3) http://werkzeug.pocoo.org/docs/0.12/
4) http://jinja.pocoo.org/docs/2.9/
5) https://www.tutorialspoint.com/flask/