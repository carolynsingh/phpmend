INTERFACES
==============

Interfaces allow you to specify what methods a class should implement.

Interfaces make it easy to use a variety of different classes in the same way. When one or more classes use the same interface, it is referred to as "polymorphism".

.. note::

   Interfaces are declared with the interface keyword.

   To implement an interface, a class must use the implements keyword.

   A class that implements an interface must implement all of the interface's methods.

Example
-----------

.. code-block:: php

   <?php
   interface Animal {
   public function makeSound();
   }

   class Cat implements Animal {
   public function makeSound() {
    echo "Meow";
   }
   }

   $animal = new Cat();
   $animal->makeSound();
   ?>

Result :

.. image:: images/img.png