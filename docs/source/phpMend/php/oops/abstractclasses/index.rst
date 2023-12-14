ABSTRACT CLASSES
==================

Abstract classes and methods are when the parent class has a named method, but need its child class(es) to fill out the tasks.

An abstract class is a class that contains at least one abstract method. An abstract method is a method that is declared, but not implemented in the code.

.. note::

   An abstract class or method is defined with the abstract keyword:

Example :

.. code-block::

   <?php
   // Parent class
   abstract class Car {
   public $name;
   public function __construct($name) {
    $this->name = $name;
   }
   abstract public function intro() : string;
   }

   // Child classes
   class Audi extends Car {
   public function intro() : string {
    return "Choose German quality! I'm an $this->name!";
   }
   }

   // Create objects from the child classes
   $audi = new audi("Audi");
   echo $audi->intro();
   echo "<br>";

   ?>

Result :

.. image:: images/img.png

The Audi class is inherited from the Car class. This means that the Audi class can use the public $name property as well as the public __construct() method from the Car class because of inheritance.
