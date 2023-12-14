CLASSES & OBJECTS
===================

A class is a template for objects, and an object is an instance of class.

A class is defined by using the class keyword, followed by the name of the class and a pair of curly braces ({}). All its properties and methods go inside the braces.

.. note::

   In a class, variables are called properties and functions are called methods!

We can create multiple objects from a class. Each object has all the properties and methods defined in the class, but they will have different property values.

Objects of a class are created using the new keyword.

Below we declare a class named Fruit consisting of two properties ($name and $color) and two methods set_name() and get_name() for setting and getting the $name property.

Example
--------


In the example below, $apple and $banana are instances of the class Fruit:

.. code-block:: php

   <?php
   class Fruit {
   // Properties
   public $name;
   public $color;

   // Methods
   function set_name($name) {
    $this->name = $name;
   }
   function get_name() {
    return $this->name;
   }
   }

   $apple = new Fruit();
   $banana = new Fruit();
   $apple->set_name('Apple');
   $banana->set_name('Banana');

   echo $apple->get_name();
   echo "<br>";
   echo $banana->get_name();
   ?>

Result :

.. image:: images/img.png
