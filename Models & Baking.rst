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
    <small><?= $post['created']->format(DATE_RFC850); ?></small>

