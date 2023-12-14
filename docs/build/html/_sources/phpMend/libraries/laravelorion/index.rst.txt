LARAVEL ORION
==================

INTRODUCTION
---------------
Laravel Orion allows you to build a fully featured REST API based on your Eloquent models and relationships with simplicity of Laravel as you love it.

FEATURES
------------

* REST API for models and relationships with batch operations support
* Advanced searching capabilities with sorting, filtering, and keyword search
* Automatic OpenAPI specifications generation
* Comprehensive set of endpoint hooks
* Relations inclusion and soft deletes via query parameters
* Straightforward authorization and validation
* Responses transformation via API resources

OPERATIONS PROVIDED BY ORION REST APIs
------------------------------------------

* index
* search
* show
* store
* update
* destroy
* restore
* batchStore
* batchUpdate
* batchDestroy
* batchRestore

REQUIREMENTS
----------------

PHP >= 7.3
Laravel >= 5.7

CREATE LARAVEL PROJECT
------------------------

Create a new project using composer :

.. code-block:: bash

   composer create-project --prefer-dist laravel/laravel your_project_name

INSTALLATION
--------------
Laravel Orion can be installed into a new or existing project, simply by adding a composer dependency :

.. code-block:: bash

   composer require tailflow/laravel-orion

   php artisan vendor:publish --tag=orion-config

SIMPLE CRUD
-------------

1. Create a Model with it's controller, migration, factory and seeder :

.. code-block:: bash

   php artisan make:model Post -a

.. image:: images/img_3.png

2. Populate the database Post table

Add two columns in the CreatePostsTable migration :

.. code-block:: php

    $table->string('name');
    $table->text('description');

.. image:: images/img.png

Add some fake data in the database posts table using factories

In the database/factories/PostFactory.php add the following code in the return array :

.. code-block:: bash

    return [
           'name' => $this->faker->name,
           'description'=> $this->faker->text
        ];

Now call the PostFactory from the Database Seeder :

.. code-block:: bash

    \App\Models\Post::factory(10)->create();

Run the following command to run the migrations and Seeders :

.. code-block:: bash

   php artisan migrate --seed

.. Note:: If the table already exists, drop the tables and rerun the migrations by running

   .. code-block:: bash

      php artisan migrate:fresh --seed

3. Make the app/Http/Controllers/PostController.php extend Orion\Http\Controllers\Controller

And define protected $model property :

.. code-block:: php

   <?php

   namespace App\Http\Controllers;


   use Orion\Http\Controllers\Controller;
   use Orion\Concerns\DisableAuthorization;
   use App\Models\Post;

   class PostController extends Controller

   {
    use DisableAuthorization;
    /**
     * Fully-qualified model class name
     */
    protected $model = Post::class; // or "App\Models\Post"
   }

4. Register the route in api.php by calling Orion::resource

.. code-block:: php

   <?php

   use App\Http\Controllers\PostController;
   use Illuminate\Support\Facades\Route;
   use Orion\Facades\Orion;


   Route::group(['as' => 'api.'], function() {
    Orion::resource('posts', PostController::class);
   });

5. In the terminal run the following command to list all the routes

.. code-block:: bash

   php artisan route:list

.. image:: images/img_1.png

Hit of the GET route

.. image:: images/img_2.png

|

WORKING OF LARAVEL ORION
-------------------------------

.. image:: images/img_4.png