NAMESPACES
=============

Namespaces are qualifiers that solve two different problems :

1. They allow for better organization by grouping classes that work together to perform a task.

2. They allow the same name to be used for more than one class.

.. note::

   Namespaces are declared at the beginning of a file using the namespace keyword.

   A namespace declaration must be the first thing in the PHP file.

Example
---------

.. code-block:: php

   <?php
   namespace Html;
   class Table {
   public $title = "";
   public $numRows = 0;
   public function message() {
    echo "<p>Table '{$this->title}' has {$this->numRows} rows.</p>";
   }
   }
   $table = new Table();
   $table->title = "My table";
   $table->numRows = 5;
   $table->message();
   ?>

Result :

.. image:: images/img.png





