NAMESPACES
===========

Namespaces are declared at the beginning of a file using the namespace keyword :

Declare a namespace called Html :

.. code-block:: php

   <?php
   namespace Html;
   ?>

.. note::

   A namespace declaration must be the first thing in the PHP file. The following code would be invalid :

   <?php

   echo "Hello World!";

   namespace Html;

   ...

   ?>

Declare a namespace called Html inside a namespace called Code :

.. code-block:: php

   <?php
   namespace Code\Html;
   ?>