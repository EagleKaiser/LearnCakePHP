Views & Templates
=================

What is a View?
---------------

* Views are responsible for generating and displaying the output of an application
* Everything that the user sees is output from a view/view template 

View Forms
----------

* Views can be displayed as different forms with different extensions 
* some of the most common types are:

+------+-----+
| HTML | XML |
+------+-----+
| JSON | RSS |
+------+-----+

Anatomy of the view layer
-------------------------

* Views - Templates that hold the presentation logic of an application
* Elements - Small, reusable bits of code rendered inside views 
* Layouts - Template wrapper code. Most views are rendered inside of a layout 
* Helpers - Classes that encapsulate view logic that can be used in many areas of the application like date formatter or currency formatter

AppView
-------

* All application have a default AppView class which is located in src/View/AppView.php
* AppView can be used to load helpers that can be used in all of an application's views. Use "initialize()" for this

View Templates
--------------

* By default, templates are in PHP and use a special file extension called ".ctp"(CakePHP Template)
* These files contain representational logic and are stored in "src/Template"
* Templates should be named after the controller action that it corresponds to (eg. src/Template/Posts/view.ctp)

View Variables
--------------

* Variables can be set in a controller and passed on to a view using "set()"

.. code-block:: php

    <?php
    $this->set('name','John');

* You can then use the "name" variable in the view

.. code-block:: php

    <?php
    <p>My name is <?=name?></p>

* Variable data can easily be escaped using "h()"

.. code-block:: php

    <?php
    <p>My name is <?=h(name)?></p> // this is for security

Extending Views
---------------

* Views can be wrapped in another by using view extending. This allows us to follow DRY(Don't Repeat Yourself) principles

.. code-block:: php

    <?php
    <h1><?=$this->fetch('title')?></h1>
    <?=$this->fetch('content')?>

    <div> class="actions">
        <h3>User Actions</h3>
        <?=$this->fetch('sidebar')?>
    </div>
    //title, content and sidebar are files in another folder

View Blocks
-----------

* Using view blocks, we can define blocks in our views that will be defined somewhere else
* This is ideal for things like sidebars, footers, etc

.. code-block:: php

    <?php
    $this->start('sidebar');
    echo $this->element('sidebar/recent_posts');
    echo $this->element('sidebar/recent_comments');
    $this->end();

Layouts
-------

* Layouts are templates that wrap another template and contain code that can be displayed on all pages/view
* The default layout is at src/Templates/Layouts/default.ctp

.. code-block:: php

    <?php
    <?=$this->fetch('content')?> 
    //displays the content from the current rendered view

* set the title with 

.. code-block:: php

    <?php
    <?=$this->assign('title','Welcome');?>

Elements
--------

* Elements are small bits of view code that can be re-used from page to page
* This would be things like ad boxes, navigation, sliders, etc
* Can be considered a mini-view

.. code-block:: php

    <?php
    echo $this->element('navbar');

* Can add a second param to pass values

Custom View Classes
-------------------

* You can add custom classes for new types of data view
* Classes go in "src/View"
* Should be suffixed with View (eg. pdfView)
* When referencing a view class, do not use "View"

.. code-block:: php

    <?php
    $builder->viewClass('pdf');

Themes
------

* Themes are plugins that focus on providing template files. They are used to switch the look of the page quickly and easily
* Set the theme name in the beforeRender() callback in the controller

.. code-block:: php

    <?php
    class PostController extends AppController{
        public function beforeRender(Event $event){
            $this->viewBuilder()->theme('Modern');
        }
    }

JSON & XML Views (Data Views)
------------------------------

* JsonView & XmlView allow us to create JSON & XML responses
* You must enable RequestHandlerComponent

.. code-block:: php

    <?php
    $this->loadComponent('RequestHandler');

* You can generate these either by using the _serialize key of by just creating normal template files



