TRAITS
=======

PHP only supports single inheritance: a child class can inherit only from one single parent.

Traits are used to declare methods that can be used in multiple classes. Traits can have methods and abstract methods that can be used in multiple classes, and the methods can have any access modifier (public, private, or protected).

.. note::

   Traits are declared with the trait keyword.

   To use a trait in a class, use the use keyword.

Example
---------

.. code-block:: php

   <?php
   trait message1 {
   public function msg1() {
    echo "OOP is fun! ";
   }
   }

   trait message2 {
   public function msg2() {
    echo "OOP reduces code duplication!";
   }
   }

   class Welcome {
   use message1;
   }

   class Welcome2 {
   use message1, message2;
   }

   $obj = new Welcome();
   $obj->msg1();
   echo "<br>";


   $obj2 = new Welcome2();
   $obj2->msg1();
   $obj2->msg2();
   ?>

Result :

.. image:: images/img.png

Here, we declare two traits: message1 and message2. Then, we create two classes: Welcome and Welcome2. The first class (Welcome) uses the message1 trait, and the second class (Welcome2) uses both message1 and message2 traits (multiple traits are separated by comma).

