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
        
    .. note:: 
    
        You can specify the link with this code 
        
        .. code-block:: html
        
            <li><a target="Home" href="http://localhost:81/myblog/">Home</li>

    * Now we will get a value from the controller to the view
        * In PostsController.php add the following code to index function

        .. code-block:: php
        
            <?php
            $this->set('person', 'John');

        * In index.ctp in Template/Posts folder add the following code in one of the columns

        .. code-block:: php
        
            <?php
            <h1><?= $person </h1>

        * This is another example in PostsController.php add the following code to index function

        .. code-block:: php
        
            <?php
            $people = ['Mike', 'Paul', 'Jeff', 'Michelle'];
            $this->set('people', $people);

        * In index.php add the following code to one of the div 

        .. code-block:: php

            <?php
             <ul>
                <?php foreach($people as $person){ ?>
                    <li><?= $person ?></li>
                <?php } ?>
            </ul>

        * Another example to get values from a multidimensional array, add the following code to PostsController.php

        .. code-block:: php
        
            <?php
                $posts = [
                    ['id' => 1, 'title' => 'First Post', 'body' => 'This is my first post'],
                    ['id' => 2, 'title' => 'second Post', 'body' => 'This is my second post'],
                    ['id' => 3, 'title' => 'third Post', 'body' => 'This is my third post'],
                ];
                $this->set('posts', $posts);

        * In index.ctp add the following code

        .. code-block:: php
        
            <?php foreach($posts as $post) { ?>
                <div>
                    <h4><?= $post['title'] ?></h4>
                    <p><?= $post['body'] ?></p>
                </div>
                <hr>
            <?php } ?>
            
        * An another example we will work with the view method in PostsController.php so first we have to create a new file in Template/Post folder called view.ctp 
        * Then add the following code to view method in PostsController.php
        
        .. code-block:: php
        
            <?php
            $post = [
                'id' => $id,
                'title' => 'First Post',
                'body' => 'This is my first post'
            ];
            $this->set('post', $post);

        .. note:: 
        
            We have previously made a route for this functionality in routes.php posts scope

            .. code-block:: php
            
                <?php
                $routes->connect('/:id',
                    ['controller'=>'Posts', 'action'=>'view'],
                    ['id'=>'\d+', 'pass' => ['id']]
                ); 

        * And in view.ctp add the following code

        .. code-block:: php

            <div class = "row">
                <div class="columns large-3 medium-4">
                    <h3>sidebar</h3>
                </div>
                <div class="columns large-9 medium-8">
                    <h1><?= $post['title'] ?></h1>
                    <p><?= $post['body'] ?></p>
                    
                </div>
            </div>

        * Now if you go to localhost:81/myblog/posts/1 you will get the results

        .. note:: 
        
            in index.ctp you can make a link for each of the id by using the following code

            .. code-block:: php
            
                <h4><?= $this->Html->link($post['title'], '/posts/'.$post['id']) ?></h4>