STATIC METHODS AND PROPERTIES
================================

STATIC METHODS
----------------

Static methods can be called directly - without creating an instance of the class first.

.. note::

   Static methods are declared with the static keyword.

   To access a static method use the class name, double colon (::), and the method name.

Example
---------

.. code-block:: php

   <?php
   class greeting {
   public static function welcome() {
    echo "Hello World!";
   }
   }

   // Call static method
   greeting::welcome();
   ?>

Result :

.. image:: images/img.png

STATIC PROPERTIES
----------------------

Static properties can be called directly - without creating an instance of a class.

.. note::


   Static properties are declared with the static keyword

   To access a static property use the class name, double colon (::), and the property name

Example
----------

.. code-block:: php


   <?php
   class pi {
   public static $value = 3.14159;
   }

   // Get static property
   echo pi::$value;
   ?>

Result :

.. image:: images/img_1.png




