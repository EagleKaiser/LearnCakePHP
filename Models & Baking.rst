Models & Baking
===============

* In src/Model/Table folder create a new file called PostsTable.php with the following code:

.. code-block:: php

    <?php
    namespace App\Model\Table;
    use Cake\ORM\Table;

    class PostsTable extends Table{
        public function initialize(array $config){
            $this->addBehavior('Timestamp');
        }
    }

* In PostsController.php remove the code from initialize and remove private $posts function and modify index function as follows:

.. code-block:: php

    <?php
    public function index(){
        $posts = $this->Posts->find('all');
        $this->set(compact('posts'));
    }

* Now to get posts of certain $id change view function in PostsController to have the following code
    * In PostsController.php

    .. code-block:: php

        <?php
        public function view($id){
            $posts = $this->Posts->find('all');
            $post = $this->Posts->get($id);
            $this->set(compact('posts', 'post'));
        }
    
    * In View.ctp

    .. code-block:: html

        <div class = "row">
            <div class="columns large-3 medium-4">
                <h3>sidebar</h3>
            </div>
            <div class="columns large-9 medium-8">
                <h1><?= $post['title'] ?></h1>
                <p><?= $post['body'] ?></p>
            </div>
        </div>

* To sort the results so the newest posts will be on top modify the index function as follows:

.. code-block:: php

    <?php
    public function index(){
        $posts = $this->Posts->find('all', array('order' => array('created' => 'desc')));
        $this->set(compact('posts'));
    }

* To create a new post do the following:
    * PostsController.php add the following code:

    .. code-block:: php
    
        <?php
        public function initialize(){
            parent::initialize();
                $this->loadComponent('Flash');
            }
            public function create(){
                $post = $this->Posts->newEntity();

            if($this->request->is('post')){
                $post = $this->Posts->patchEntity($post, $this->request->data);
                if($this->Posts->save($post)){
                    $this->Flash->success(__('Post Created'));
                    return $this->redirect(['action' => 'index']);
                }
                $this->Flash->error(__('Unable to save post'));
            }
            $this->set('post', $post);
        }

* To add timestamp to each post add the following to Template/Posts/index.ctp after the title tag:

.. code-block:: php

    <?php
    <small><strong><?= $post['created']->format(DATE_RFC850); ?></strong></small>

Validation
----------

* To Add validation add the following code in the src/Model/PostsTable.php

.. code-block:: php

    <?php
    namespace App\Model\Table;
    use Cake\ORM\Table;
    use Cake\Validation\Validator;

    class PostsTable extends Table{
        public function initialize(array $config){
            $this->addBehavior('Timestamp');
        }
        public function validationDefault(Validator $validator){
            $validator
                ->notEmpty('title')
                ->RequirePresence('title')
                ->notEmpty('body')
                ->RequirePresence('body')
                ->notEmpty('author')
                ->RequirePresence('author')
                ->notEmpty('category')
                ->RequirePresence('category');
            return $validator;
        }
    }

Update
------

* To update a post add the following code to PostsController.php

.. code-block:: php

    <?php
    public function edit($id){
        //die('create post');
        //$posts = $this->Posts->find('all');
        //$this->set(compact('posts'));

        $post = $this->Posts->get($id);

        if($this->request->is(['post', 'put'])){
            $this->Posts->patchEntity($post, $this->request->data);
            if($this->Posts->save($post)){
                $this->Flash->success(__('Post Updated'));
                return $this->redirect(['action' => 'index']);
            }
            $this->Flash->error(__('Unable to update post'));
        }
        $this->set('post', $post);
    }

* Create a new file under Layout/Posts called edit.ctp with the following code which is almost same as create.ctp 

.. code-block:: php

    <?php
    <h3>Edit Post</h3>
    <?= $this->Form->create($Post); ?>
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

* Now edit Layout/Posts/view.ctp to have the following code:

.. code-block:: php

    <?php
    <div class = "row">
    <div class="columns large-3 medium-4">
        <h3>sidebar</h3>
    </div>
    <div class="columns large-9 medium-8">
       <h1><?= $post['title'] ?></h1>
       <p><?= $post['body'] ?></p>
    </div>

    <?= $this->Html->link('Edit Post', ['action' => 'edit', $post['id']], ['class' => 'class-name']); ?>

Delete
------

* In PostsController.php add the following code:

.. code-block:: php 

    <?php
    public function delete($id){
        $this->request->allowMethod(['post', 'delete']);
        $post = $this->Posts->get($id);
        if($this->Posts->delete($post)){
            $this->Flash->success(__('Post Deleted'));
            return $this->redirect(['action' => 'index']);
        }
    }

* Now in Layout/Posts/view.ctp add the following code:

.. code-block:: php

    <?= $this->Form->postLink('delete',
                            ['action' => 'delete', $post['id']],
                            ['confirm' => 'Are you sure?','class' => 'className']); ?>

