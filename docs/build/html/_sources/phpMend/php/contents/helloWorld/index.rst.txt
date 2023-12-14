HELLO WORLD
=============
Below, we have an example of a simple PHP file, with a PHP script that uses a built-in PHP function "echo" to output the text "Hello World!" on a web page :

.. code-block:: php

   <!DOCTYPE html>
   <html>
   <body>

   <h1>My first PHP page</h1>

   <?php
   echo "Hello World!";
   ?>

   </body>
   </html>



.. note::
   PHP statements end with a semicolon (;).


Boot up the server :

.. code-block:: bash

   php -S localhost:8888

.. image:: images/img.png

.. note::
   In PHP, keywords (e.g. if, else, while, echo, etc.), classes, functions, and user-defined functions are not case-sensitive.
   However; all variable names are case-sensitive.