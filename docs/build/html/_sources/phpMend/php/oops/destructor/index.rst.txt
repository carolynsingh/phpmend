DESTRUCTOR
============

A destructor is called when the object is destructed or the script is stopped or exited.

If you create a __destruct() function, PHP will automatically call this function at the end of the script.

.. note::

   The construct function starts with two underscores (__)

Example
---------

.. code-block:: php

   <?php
   class Fruit {
   public $name;


   function __construct($name) {
    $this->name = $name;
   }
   function __destruct() {
    echo "The fruit is {$this->name}.";
   }
   }

   $apple = new Fruit("Apple");
   ?>

Result :

.. image:: images/img.png
