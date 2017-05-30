Views Example
=============

* First change the homepage to be post instead of default by changing the route of default home page in routes.php

.. code-block:: php

    <?php
    Router::scope('/', function ($routes){
        $routes->connect('/', ['controller'=> 'Posts', 'action' => 'index', 'home']);
    }

* Create a new folder in src/Template directory called Posts and inside it a new file called index.ctp
* In PostsController.php make sure that index do not contain die function or the program will stop executing before reaching Posts view 
* Add the following code to index

.. code-block:: html

    <div class = "row">
        <div class="columns large-3 medium-4">
            <h3>sidebar</h3>
        </div>
        <div class="columns large-9 medium-8">
            <h1>Main Content</h1>
        </div>
    </div>

* Now we will remove Documentation and API links and replace it with Home link by going to src/Template/Layout/default.ctp and modify with the following code
    * Remove the following code:

    .. code-block:: html

        <li><a target="_blank" href="http://book.cakephp.org/3.0/">Documentation</a></li>
        <li><a target="_blank" href="http://api.cakephp.org/3.0/">API</a></li>

    * And modify top-bar-section class as follows:

    .. code-block:: php
    
        <div class="top-bar-section">
            <ul class="right">
                <li><?php echo $this->Html->link('Home','/');?></li>
            </ul>
        </div>
        
