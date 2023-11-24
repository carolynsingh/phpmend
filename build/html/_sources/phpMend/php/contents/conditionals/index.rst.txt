CONDITIONALS
==============

PHP - The if Statement
-------------------------
The if statement executes some code if one condition is true.

Example :

.. code-block:: php

   <?php
   $t = date("H");

   if ($t < "20") {
   echo "Have a good day!";
   }
   ?>

Result :

.. image:: images/img.png

PHP - The if...else Statement
-----------------------------------
The if...else statement executes some code if a condition is true and another code if that condition is false.

Example :

.. code-block:: php

   <?php
   $t = date("H");

   if ($t < "22") {
   echo "Have a good day!";
   } else {
   echo "Have a good night!";
   }
   ?>


Result :

.. image:: images/img_1.png


