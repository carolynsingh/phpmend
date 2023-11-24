CLASS CONSTANTS
=================

Class constants can be useful if you need to define some constant data within a class.

.. note::

   A class constant is declared inside a class with the const keyword.

   We can access a constant from outside the class by using the class name followed by the scope resolution operator (::) followed by the constant name, like here:

Example
------------

.. code-block::

   <?php
   class Goodbye {
   const LEAVING_MESSAGE = "Thank you for visiting W3Schools.com!";
   }

   echo Goodbye::LEAVING_MESSAGE;
   ?>

Result :

.. image:: images/img.png

Or, we can access a constant from inside the class by using the self keyword followed by the scope resolution operator (::) followed by the constant name, like here:

.. code-block::

   <?php
   class Goodbye {
   const LEAVING_MESSAGE = "Thank you for visiting W3Schools.com!";
   public function byebye() {
    echo self::LEAVING_MESSAGE;
   }
   }

   $goodbye = new Goodbye();
   $goodbye->byebye();
   ?>

Result :

.. image:: images/img_1.png

