Component and  Helpers
======================

Component
---------

* Add a new file in controller folder Called DevsController.php with the following code

.. code-block:: php

    <?php
    namespace App\Controller;
    use App\Controller\AppController;

    class DevsController extends AppController
    {
        public function index()
        {
            
        }
    }

* In Template folder add a new folder called Devs and inside create a new file called index.ctp

.. code-block:: php

    <?php
    echo 'Devs';
    //go to localhost:81/myblog/devs to see the result

* Create BlogComponent.php file in Controller/Component folder with the following code:

.. code-block:: php

    <?php
    namespace App\Controller\Component;
    use Cake\Controller\Component;

    class BlogComponent extends Component 
    {
        public function sayHello()
        {
            return 'Hello';
        }
    }

* In PostController.php add the following code:
    * Create initialize() function

    .. code-block:: php

        <?php
        public function initialize()
        {
            $this->loadComponent('Blog');
        }

    * In index() function add the following code
    
    .. code-block:: php
    
        <?php
        die($this->Blog->sayHello());

* Now if you go to url localhost:81/myblog/posts will return Hello

.. note:: 

    Because we have created a route from default page to PostController we will get the same result by going to url localhost:81/myblog

* Now remove die() function from PostsController() and add the following code to BlogComponent.php

.. code-block:: php

    <?php
    public function getPosts()
    {
        $posts = [
                    ['id' => 1, 'title' => 'First Post', 'body' => 'This is my first post'],
                    ['id' => 2, 'title' => 'second Post', 'body' => 'This is my second post'],
                    ['id' => 3, 'title' => 'third Post', 'body' => 'This is my third post'],
                ];
        return $posts;
    }

* Change PostController.php to be as follows:

.. code-block:: php

    <?php
    private $posts;
    public function initialize()
    {
        $this->loadComponent('Blog');
        $this->posts = $this->Blog->getPosts();
    }

* Add the following code to DevsController.php

.. code-block:: php

    <?php
    private $posts;
    public function initialize()
    {
        $this->loadComponent('Blog');
        $this->posts = $this->Blog->getPosts();
    }
    public function index()
    {
        $this->set('posts', $this->posts);
    }

* Add a new component file in component directory called DevsComponent.php with the following code:

.. code-block:: php

    <?php
    namespace App\Controller\Component;

    use Cake\Controller\Component;

    class DevsComponent extends Component 
    {
        public function generatePassword()
        {
            $password = '';
            $desired_length = rand(8,12);
            
            for ($length = 0; $length < $desired_length; $length++)
            {
                $password .= chr(rand(32,126));
            }
            return $password;
        }
    }

* And Add the following to DevsController.php

.. code-block:: php

  <?php
    namespace App\Controller;
    use App\Controller\AppController;

    class DevsController extends AppController
    {
        public function initialize()
        {
            $this->loadComponent('Devs');
        }
        public function index()
        {
            $this->set('password', $this->Devs->generatePassword());
        }
    }

* Then add the following to Template/devs/index.ctp

.. code-block:: php

    <?php
    <h4>Password: <?= $password ?></h4>

Helpers
-------

* You can find Html helpers ( $this->Html ) examples in Template/Layout/default.ctp
* Also in default.ctp you can add custom style attribute by adding the following code in the <head> tag

.. code-block:: PHP

    <?PHP
    <style>
        body{
            <?= $this->Html->style([
            'background' => '#000'
            ]); ?>
        }
    </style>
    // to generate a link use
    $this->Html->link('link text', 'link location');
    // to add attribute to Html helpers use the following
    $this->Html->link('link text', 'link location', ['class' => 'active']);

* To add an image using html helper use the following code:
    * in PostController.php modify  view function as follows:
    
    .. code-block:: php

        <?php
        $post = [
            'id' => $id,
            'title' => 'First Post',
            'body' => 'This is my first post',
            //added image link
            'image' => 'http://oi63.tinypic.com/21blk5g.jpg'
        ];
        $this->set('post', $post);

    * in Posts/view.ctp adde the folowing to view the image

    .. code-block:: php

        <?= $this->Html->image($post['image'], ['alt' => 'myImage']); ?>

* There are multiple helpers available like media

Nested list
-----------

* To use nested list use the following code
    * In PostsController.php add the following to the view function

    .. code-block:: php

        <?php
        $languages = [
            'Languages' => [
                'English' => [
                    'American',
                    'Canadian',
                    'British'
                ],
                'Spanish',
                'German'
            ]
        ];
        $this->set('languages', $languages);

    * In Posts/view.ctp add the following to view the list

    .. code-block:: php

        <?php
        <?= $this->Html->nestedList($languages); ?>

Html Table Helper
-----------------

* Use the following code to create a table using Html table helper

.. code-block:: php

    <?php
    <table>
        <?= $this->Html->tableHeaders(
            ['Id', 'Name', 'Email']
        ); ?>
        <?= $this->Html->tableCells([
            ['1', 'John', 'j@j.com'],
            ['2', 'Bob', 'b@b.com']
        ]); ?>
    </table>

Html Form Helper
----------------

* In Template/Layout/default.ctp add the following ling in <div class="top-bar-section>

    .. code-block:: php

        <?php
        <li><?= $this->Html->link('Create Post', '/posts/create'); ?> </li>

* Create a new Template Posts folder called create.ctp with the following code:

.. code-block:: php

    <h3>Create Post</h3>
    <?= $this->Form->create(); ?>
        <?= $this->Form->input('title', array(
                                    'label' => 'Post Title',
                                    'class' => 'class-name'
                            )); ?>
        <?= $this->Form->input('body', array(
                                    'label' => 'Post Body',
                                    'type' => 'textarea',
                                    'escape' => false,
                                    'class' => 'class-name'
                            )); ?>    
        <?= $this->Form->input('category', array(
                                    'label' => 'Categoty',
                                    'type' => 'select',
                                    'empty' => 'Select One',
                                    'options' => ['Web Development', 'Design', 'Marketing'],                                     
                                    'class' => 'class-name'                                
                            )); ?>  
        <?= $this->Form->input('author', array(
                                    'label' => 'Author',
                                    'class' => 'class-name'
                            )); ?>  
        <?= $this->Form->input('time', [
            'type' => 'time',
            'interval' => 15
        ]); ?>
        <?= $this->Form->inputs([
            'name' => ['label' => 'Name', 'class' => 'class-name'],
            'age' => ['label' => 'Age', 'class' => 'class-name']
        ]); ?>
        <hr>
        <?= $this->Form->submit('Submit', array('class' => 'class-name')); ?>
    <?= $this->Form->end(); ?>

Text Helper
-----------

.. code-block:: php

    <?= $this->Text->truncate($post['body'],196,['ellipsis' => '...', 'exact' => false]); ?>