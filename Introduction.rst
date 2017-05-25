Introduction
============

Convention over Configuration
----------------------------

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
    * Controller class names are plural and CamelCased (Users)
    * Controller class names end with "controller" (UsersController)
    * Index() is the default method in a controller
    * yourapp.com/welcome will call the welcome controller and the index() method
* Views