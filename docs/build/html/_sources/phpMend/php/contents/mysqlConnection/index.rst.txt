MySQL CONNECTION USING PDO
==============================

SETTING UP MYSQL CONTAINER
------------------------------

To set up MySQL :

.. code-block:: bash

   sudo docker run --name mysql -d -p 3307:3306 --

   env="MYSQL_DATABASE=laravel" --

   env="MYSQL_ROOT_PASSWORD=Qwerty@321" --

   env="MYSQL_USER=test" --

   env="MYSQL_PASSWORD=Qwerty@321" mysql --bind_address=0.0.0.0

.. note::

   3307 is the port that can be any within the port range.

   3306 in the right is the internal default mysql port that can't be changed.

To start container :

.. code-block:: bash

   sudo docker start mysql

.. image:: images/img.png

To execute :

.. code-block:: bash

   sudo docker exec -it mysql sh
   mysql -u root -p

.. image:: images/img_1.png

RUNNING MYSQL QUERIES
-------------------------

.. note::

   SQL keywords are NOT case sensitive: select is the same as SELECT.


The CREATE DATABASE statement is used to create a database in MySQL.

.. code-block:: bash

   $sql = CREATE DATABASE myDB;

.. image:: images/img_2.png

The SHOW DATABASE statement is used to view all databases in MySQL.

.. code-block:: bash

   $sql = SHOW DATABASES;

.. image:: images/img_3.png

The USE (name of database) statement is used to use a particular database in MySQL.

.. code-block:: bash

   $sql = USE myDB;

.. image:: images/img_4.png

The CREATE TABLE statement is used to create a table in MySQL.

.. code-block:: bash

   $sql = CREATE TABLE MyGuests (
   id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
   firstname VARCHAR(30) NOT NULL,
   lastname VARCHAR(30) NOT NULL,
   email VARCHAR(50),
   reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   );

.. image:: images/img_5.png

The SHOW FULL TABLES statement is used to view tables in MySQL.

.. code-block:: bash

   $sql = SHOW FULL TABLES;

.. image:: images/img_6.png

The INSERT INTO statement is used to add new records to a MySQL table.

.. code-block:: bash

   $sql = INSERT INTO MyGuests (firstname, lastname, email)
   VALUES ('John', 'Doe', 'john@example.com');

.. image:: images/img_7.png


The SELECT statement is used to select data from one or more tables.

.. code-block:: bash

   $sql = SELECT column_name(s) FROM table_name;

.. image:: images/img_8.png

or we can use the * character to select ALL columns from a table.

.. code-block:: bash

   $sql = SELECT * FROM table_name;

.. image:: images/img_9.png

The DROP DATABASE statement is used to delete a database.

.. code-block:: bash

   $sql = DROP DATABASE mydb;

.. image:: images/img_10.png

Construct PDO :

.. code-block:: bash

   try {

  $pdo = new PDO('mysql:host=127.0.0.1.3307;dbname=mydb', 'root', 'Qwerty@321');

  } catch (PDOException $e)
