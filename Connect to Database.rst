Connect to Database
===================

* Go to localhost:81/phpmyadmin
* Create a new user with password and all privileges
* Create a Database called myblog
* Create one table called posts with 7 columns 

+----------+----------+--------+---------+------+
| Name     | Type     | Length | Index   | A_I  |
+==========+==========+========+=========+======+
| id       | INT      | 11     | PRIMARY | Tick |
+----------+----------+--------+---------+------+
| title    | VARCHAR  | 255    |         |      |
+----------+----------+--------+---------+------+
| body     | TEXT     |        |         |      |
+----------+----------+--------+---------+------+
| category | VARCHAR  | 255    |         |      |
+----------+----------+--------+---------+------+
| author    | VARCHAR  | 255    |         |      |
+----------+----------+--------+---------+------+
| created  | DATETIME |        |         |      |
+----------+----------+--------+---------+------+
| modified | DATETIME |        |         |      |
+----------+----------+--------+---------+------+

* Insert some data into posts table
* In the project folder go to config->app.php and in approximately line 230 start changing user name with user from phpmyadmin with the corresponded password and change the name of database to myblog


