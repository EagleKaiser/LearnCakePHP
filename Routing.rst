Routing
=======


* Routing allows us to map URLs to specific controller actions
* Routes are defined in the config/routes.php
* There are 2 interfaces to define a rout:

1- Scoped Builder (Recommended)
-------------------------------
.. code-block:: php

    <?php
    Router::scope('/users', function($routes)
    {
        $routes->connect('/',['controller'=>'Users', 'action'=>'index']);
    });

2- Static Method
----------------
.. code-block:: php

    <?php
    Router::connect('/users',['controller'=>'Users', 'action'=>'index']);

Simple Route
------------
.. code-block:: php

    <?php
    $routes->connect('/users',['controller'=>'Users','action'=>'index']);
    //this will execute the index() method inside of the Users controller

.. code-block:: php
    
    <?php
    $routes->connect('/users',['controller'=>'Users']);
    //this will do the same thing because index() is the default action

Passing Parameters
------------------

.. code-block:: php

    <?php
    Router::connect(
    '/users/:id',
    ['controller'=>'Users', 'action'=>'view'],
    ['id'=>'\d+','pass'=>['id']] //'\d+' is a regular expression that will validate that the id is a digit
    );

Wildcard
--------
.. code-block:: php

    <?php
    $routes->connect(
        '/listings/*',
        ['controller'=>'Listings','action'=>'display']
    );

.. note:: 

    * Use the * to specify a Wildcard
    * The above Wildcard will take /listings/[anything] to the display() method of the Listings controller
    * You can also use ** to capture the remainder of a URL as a single passed argument

Route Elements
--------------

* Specifying route elements allows you to define places in the URL where parameters for controller actions should lie

.. code-block:: php
    
    <?php
    $routes->connect(
    '/:controller/:id',
    ['action'=>'view'],
    ['id'=>'[0-9]+']  //regular expression [0-9]
    );

* This will allow you to view models from any controller with a URL like /controllername/:id

Special Route elements
----------------------

+------------+--------+---------+---------+
| Controller | Action | Plugin  | Prefix  |
+------------+--------+---------+---------+
| _ext       | _base  | _scheme | _host   |
+------------+--------+---------+---------+
| _port      | _full  | _ssl    | _method |
+------------+--------+---------+---------+
| name       |        |         |         |
+------------+--------+---------+---------+

Named Elements
--------------

* You can use named routes to make calling them more convenient as well as boost performance a bit

.. code-block:: php

    <?php
    $routes->connect('/login',
        ['controller'=>'Users','action'=>'login'],
        ['_name'=>'login]
    );
    //Generate a URL 
    $url = Router::url(['_name'=>'login]);
    //with query strings
    $url = Router::url(['_name'=>'login','username'=>'jimmy']);

Route Prefixes
--------------

* Specifying route elements allows you to define places in the URL where parameters for controller actions should lie

.. code-block:: php

    <?php
    Router::prefix('admin',function($routes){
        $routes->connect('/create',['controller'=>'Posts','action'=>'create']);
    });
    
* Now we can go to /admin/create to create a new Posts

Plugin Routes
-------------

* You can create special routes for plugins

.. code-block:: php

    <?php
    Router::plugin('MyPlugin',function($routes){
        $routes->connect('/:controller');
    });

* Routes connected above will automatically have the prefix of '/my_plugin'

Dashed Route
------------

* Dashed Route can be used to format URLs to use dashes

.. code-block:: php

    <?php
    Router::plugin('ToDo',['path'=>'to-do'],function($routes){
    $routes->fallbacks('DashedRoute');
    });

File Extensions
---------------

.. code-block:: php

    <?php
    Router::extensions(['html','json']);
    //this will allow you to use the .html or .json extensions in your routes
    //you can also set extensions per scope
    Router::scope('/api',function($routes){
        $routes->extensions(['json','xml']);
    });

Restful Routes
--------------

You can easily generate RESTful routes with the Router

.. code-block:: console

    <?php
    Router::scope('/',function($routes){
        $routes->extensions([json']);
        $routes->resources('Posts');
    });


Nested resources
----------------

* Once resources are in a scope, you can connect sub-resource routes as well

.. code-block:: php

    <?php
    Router::scope('/api',function($routes){
        $routes->resources('Posts',function($routes){
            $routes->resources('Comments');
        });
    });

.. note:: 

    /api/posts/:id/Comments
    /api/posts/:id/Comments/:id

