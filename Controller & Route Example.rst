Controller & Route Example
==========================

* Go to url localhost:81/myblog/posts and it will through an error asking to create PostsController class in src/Controller/PostsController.php with the following code:

.. code-block:: php

    <?php
    namespace App\Controller;
    use App\Controller\AppController;
    class PostsController extends AppController
    {

    }

* Create PostsController.php file in src/Controller folder and but the above code in it
* Now if you go to the url it will ask for an index function inside PostsController class

.. code-block:: php

    <?php
    namespace App\Controller;
    use App\Controller\AppController;
    class PostsController extends AppController
    {
        public function index()
        {
            
        }
    }

* Now CakePHP will ask for a view for the PostsController::index() in src/Template/Post/index.ctp but we will change the Route
* Go to config/routes.php to add a route

.. code-block:: php

    <?php
    Router::scope('/posts', function(RouteBuilder $routes){
        $routes->connect('/',
            ['controller' => 'Posts']
        );
    });

* To test that the route is working add the following code to the PostsController index action then go to url localhost:81/myblog/posts

.. code-block:: php

    <?php
    die('This is the posts/index');

.. note:: 

    die function will stop the execution of the program at this point and without it the program will proceed to need a view for the controller

* Note that you can use a constructor code to run a code as soon as AppController start

.. code-block:: php

    <?php
    public function initialize(){
        die('This is the initialize constructor for AppController');
    }

.. note:: 

    initialize() function will override index() function

* Now we will pass a parameter after post url and use it in the controller:
    * In routes.php add the following code:

    .. code-block:: php
        
        <?php
        Router::scope('/posts/:id', function($routes){
            $routes->connect('/',
            ['controller'=>'Posts', 'action'=>'view'],
            ['id'=>'\d+', 'pass' => ['id']] //the \d+ is a regular expression for validation
            );
        });

    * In PostsController.php add the following code: 

    .. code-block:: php
    
        <?php
        public function view($id){
            die('This is post '.$id);
        }

* Note that you have to comment the initialize function that we have created before or it will override this function
* Now if you go to url localhost:81/myblog/post/1 it will show This is post 1
* The following code are in the same scope (posts) and should be modified:

.. code-block:: php

    <?php
    Router::scope('/posts', function(RouteBuilder $routes){
    $routes->connect('/',
        ['controller' => 'Posts']
        );
    });

    Router::scope('/posts/:id', function($routes){
        $routes->connect('/',
        ['controller'=>'Posts', 'action'=>'view'],
        ['id'=>'\d+', 'pass' => ['id']]
        );
    });

* Modified to:

.. code-block:: php

    <?php
    Router::scope('/posts', function(RouteBuilder $routes){
        $routes->connect('/',
            ['controller' => 'Posts']
        );
        $routes->connect('/:id',
        ['controller'=>'Posts', 'action'=>'view'],
        ['id'=>'\d+', 'pass' => ['id']]
        );
    });

* Add create and edit route to the post scope 
    * In routes.php add the following code

    .. code-block:: php

        <?php
        
            $routes->connect('/create',
                ['controller'=>'Posts', 'action'=>'create']
            );
            $routes->connect('edit/:id',
                ['controller'=>'Posts', 'action'=>'edit'],
                ['id' => '\d+', 'pass' => ['id']]
            );

    * In PostsController.php add the following code

    .. code-block:: php
    
        <?php
        public function edit($id){
            die('editing post '.$id);
        }
        public function create(){
            die('create post');
        }
        
* This will add url localhost:81/myblog/edit/<any number> and localhost:81/myblog/create

Query String
------------

* To get the a parameter from url like localhost:81/myblog/posts/hello?name=mike
    * In PostsController.php add the following code:

    .. code-block:: php

        <?php
        public function hello(){
            die('hello '.$this->request->query['name']);
            //or
            //die('hello '.$this->request['url']['name']);
        }

    * And add the following code to routes.php inside posts scope

    .. code-block:: php

        <?php
        $routes->connect('/hello',
            ['controller'=>'Posts', 'action', => 'hello']
        );

    * Now if you go to url localhost:81/myblog/posts/hello?name=mike will get hello mike
* And for multiple parameter in a url like localhost:81/myblog/posts/hello?name=mike&age=34 use the following code for the hello() function

.. code-block:: php

    <?php
    public function hello(){
        //print_r($this->request->query);
        //or for more info about parameters
        print_r($this->request->params);
        die();
    }

* And to know if the request is a post or get use the following in the hello() function

.. code-block:: php

    <?php
    if($this->request->is('post')){
        die('This is a POST');
    } elseif($this->request->is('get')){
        die('This is a GET');
    }

* And to get path information use the following in hello() function

.. code-block:: php

    <?php
    //die($this->request->webroot); //will output: /myblog/ 
    die($this->request->base); //will output: /myblog
    die($this->request->hear); //will output: /myblog/posts/hello
    die($this->request->header('User-Agent')); //will output: agent information

Router Prefixes
---------------

* Adding prefix for admin localhost:81/myblog/admin add the following code to routes.php

.. code-block:: php

    Router::prefix('admin', function($routes){
        $routes->connect('/', ['controller' => 'Dashboard']);
        $routes->connect('/create', ['controller' => 'Posts', 'action' => 'create']);
    });

* Now if you go to localhost:81/myblog/admin it will ask for DashboardController.php which we will add to a new folder in src/Controller folder called Admin 
* Also create PostsController.php in the same folder
    * DashboardController.php 
    
    .. code-block:: php 

        <?php
        namespace App\Controller\Admin;  // note the Admin namespace point to the Admin folder
        use App\Controller\AppController;

        class DashboardController extends AppController
        {
            public function index(){
                die('Dashboard');
            }
        }

    * PostsController.php

    .. code-block:: php
    
        <?php
        namespace App\Controller\Admin;  // note the Admin namespace point to the Admin folder
        use App\Controller\AppController;

        class PostsController extends AppController
        {
            public function create(){
                die('Admin creating Post');
            }
        }

    * So now if you go to localhost:81/myblog/admin you will get Dashboard and if you go to localhost:81/myblog/admin/create you will get Admin Creating Post
    
    .. note:: 
    
        Note that if you change the route to scope instead of prefix you will get create post which is the result for PostsController.php that belong to the Controller folder not the one belongs to Admin folder and this is the diffident between scope and prefix where prefix will look for a controller within a sub-namespace which in this case is the Admin folder




