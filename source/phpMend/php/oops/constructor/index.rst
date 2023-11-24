CONSTRUCTOR
=============

A constructor allows you to initialize an object's properties upon creation of the object.

If you create a __construct() function, PHP will automatically call this function when you create an object from a class.

.. note::

   The construct function starts with two underscores (__)

Example
--------

.. code-block:: php

   <?php
   class Fruit {
   public $name;

   function __construct($name) {
    $this->name = $name;
   }
   function get_name() {
    return $this->name;
   }
   }

   $apple = new Fruit("Apple");
   echo $apple->get_name();
   ?>

Result :

.. image:: images/img.png


