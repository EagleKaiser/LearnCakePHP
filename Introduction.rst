Introduction
============

Installation
------------

* Install XAMPP, change port to 81
* Web server is in c->xampp->htdocs which is the place where the program will run through localhost:81
* Download Composer.phar by using the following console command line in the htdocs directory

.. code-block:: console

    php -r "readfile('https://getcomposer.org/installer');" | php
    php composer.phar create-project --prefer-dist cakephp/app myblog

* If Composer is installed globally in the machine by using composer.exe then use the following comand line in the htdocs directory

.. code-block:: console

    composer create-project --prefer-dist cakephp/app myblog

.. note:: 

    * This command will create a folder in htdocs called myblog with the required files to run a CakePHP project
    * To run the app go to localhost:81/myblog

Naming Conventions
------------------

* Files and class names
    * Filenames are underscored (my_cake_class.php)
    * Class names are CamelCased (MyCaseClass)
    * Controller class names and files end with "controller"
* Models & Databases
    * Model class name are singular and CamelCased (User)
    * Table names corresponding to models are plural and underscored(users)
    * You can use `inflector <http://inflector.cakephp.org>`_  to check singular and plural words(category and categories)
    * Foreign keys are recognized by default as the singular name of the related table followed by _id (user_id)
    * Tables where models interact will require a singular primary key to uniquely identify each row
* Controllers
    * Controller class names are plural and CamelCased and ends with "controller" (UsersController)
    * Index() is the default method in a controller
    * yourapp.com/welcome will call the welcome controller and the index() method
* Views
    * Template files should be named after the controller functions that they display
    * The doThis() function of the UsersController class looks for a template in src/Template/Users/do_this.ctp
* Example
    * Database table: “articles” 
    * Table class: ArticlesTable, found at src/Model/Table/ArticlesTable.php 
    * Entity class: Article, found at src/Model/Entity/Article.php 
    * Controller class: ArticlesController, found at src/Controller/ArticlesController.php 
    * View template, found at src/Template/Articles/index.ctp
    
Routing
-------

    * Routing maps URLs to controller actions
    * Routing helps URLs look much better and perform better in search engines
    * Apache's mod_rewrite is not required for routing but makes things look neater
    * Default route pattern: http://example.com/controller/action/param1/param2..

Helpers
-------

* Helpers are component-like classes for the presentation layer of the application
* Presentation logic is shared between views or layouts
* To use a helper, add it to the controller's helpers array
* var $helpers = array('Form','Html','Javascript','Time');

+--------------+--------+------+------------+
|Helpers List                               |
+==============+========+======+============+
| Ajax         | Number | XML  | Cache      |
+--------------+--------+------+------------+
| Paginator    | Form   | RSS  | HTML       |
+--------------+--------+------+------------+
| Session      | JS     | Text | Javascript |
+--------------+--------+------+------------+
| Time         |        |      |            |
+--------------+--------+------+------------+

Components
----------

* Components are packages of logic shared between Controllers
* CakePHP comes with a core set of components that can be loaded into the controller at any time 
* Components usually have some sort of configuration in the $components array of the controller's beforeFilter() method

+-----------------+---------------------+----------+
| Core Components                                  |
+=================+=====================+==========+
| Security        | Authentication      | Sessions |
+-----------------+---------------------+----------+
| Request Handler | Access Control list | Emails   |
+-----------------+---------------------+----------+
| Cookies         |                     |          |
+-----------------+---------------------+----------+

Plugins
-------

* CakePHP allows you to combine controllers, models and views together to package as a "plugin" that others can use in their Cake applications
* Plugins are tied to an application only by configuration. Otherwise, plugins operate in their own space.
* Stored in /app/plugins

Scaffolding
-----------

* Scaffolding allows developers to generate a very basic application that can perform CRUD(create, read, update and delete) operations
* Scaffolding just needs a model and controller
* Scaffolding is great for development or just to get something up and running but using it too much will cause a loss of flexibility



