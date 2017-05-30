Controllers
===========

* Controllers hold the actions that your routes are mapped to
* Controllers are responsible for:
    * The interpretation of any request data 
    * Making sure the right models are called
    * Making sure the right response or view is rendered
* Tour models should be fat and your Controllers thin
* Doing this will help your code be more reusable and also makes it easier to test

Naming Conventions
------------------

* Files should end in "Controller" (UsersController.php)
* Classes should also end in Controller (UsersController)
* Named after the primary model (User)

AppController Class
-------------------

* All controllers you create will extend the AppController Class 
* Location is src/Controller/AppController.php
* You can use the "initializer()" method in your controller class to use a constructor
* AppController extends Cake/Controller/Controller

The Request Object
------------------

* The CakePHP router use connecting routes to find and create the controller instance 
* The request data is stored in the request object
* Access using the $this->request property

Controller Setup
----------------

.. code-block:: php

    <?php
    namespace App\Controller;
    use App\Controller\AppController;
    class BooksController extends AppController{
        public function index(){
            /Action code
        }
        public function view($id){
            //Action code
        }
        public function search($query){
            //Action code
        }
    }

Passing View data
-----------------

* Controller::set() is used to pass data from the controller to the view

.. code-block:: php

    $this->set('firstName','John');
    $this->set('lastName','Doe');

* Because of CakePHP strict conventions, you co not need to render the view manually 
* In View:

.. code-block:: php

    <?php
    My name is <?=h($firstName)?> <?=h($lastName)?>
    //h is a helper function

View Options
------------

* You can set specific view options using viewBuilder()

.. code-block:: php

    <?php
    $this->viewBuilder()
        ->helper(['MyHelper'])
        ->theme('SomeTheme')
        ->className

* You can use the correct naming conventions and the view will be rendered automatically but to do it manually, use:

.. code-block:: php

    <?php
    $this->render()
    //or specify a view
    $this->render('someView')

Handling Redirects
------------------

* The most common way to redirect would be to use Controller::redirect()

.. code-block:: php

    <?php
    return $this->redirect(
        ['controller'=>'Users','action'=>'register']
        );

* Relative and Absolute Paths:

.. code-block:: php

    <?php
    return $this->redirect('/orders/thanks');
    return $this->redirect('http://www.example.com');

* Go back to referrer:

.. code-block:: php

    <?php
    return $this->redirect($this->referrer());

Actions and Model loading
-------------------------

* To redirect to another action on the same controller:

.. code-block:: php

    <?php
    $this->setAction('index');

* Load a model table that is not the controller default

.. code-block:: php

    <?php
    $this->loadModel('Articles');
    $recentArticles = $this->Articles->find('all', [
        'limit'=>5,
        'order'=>'Articles.created DESC'
    ]);

Model Pagination
----------------

* Use the $paginate attribute to easily page result from the model

.. code-block:: php

    <?php
    class PostController extends AppController
    {
        public $paginate = [
            'Post'=>[
                'conditions'=>['published'=>1]
            ]
        ];
    }

Loading Components & Helpers
----------------------------

* Define components in the controller's initialize() function:

.. code-block:: php

    <?php
    public function initialize(){
        parent::initialize();
        $this->loadComponent('Csrf');
        $this->loadComponent('Comments', Configure::read('Comments'));
    }

* Load Helpers:

.. code-block:: php

    <?php
    public $helpers = ['Form','Html'];

Life-Cycle Callbacks
--------------------

* beforeFilter(Event $event) is executed before every action in the controller. Useful for session and permission operations.
* beforeRender(Event $event) is executed after the action logic
* AfterFilter(Event $event) is executed after controller action and after render is complete




