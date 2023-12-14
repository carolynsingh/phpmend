PREDIS
========

Github Link = https://github.com/predis/predis

A `Redis` client for PHP 7.2 and newer.

``Redis`` is an open source, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, and sorted sets.

To use ``Redis`` with ``Laravel`` one needs to install ``predis`` library, an alternate is also there which is `phpredis`


PREREQUISITES
#################

1. Setup ``Redis`` on local using docker

.. code-block:: bash

        docker run  -v /docker/redis-data:/data -p 6379:6379 --name redis -d redis  redis-server --appendonly yes  --requirepass "qwerty"

Here,

.. code-block:: bash

        `Host` : localhost,

        `Port` : 6379,

        `Password` : qwerty


2. For UI one can download ``RedisInsight``

https://redislabs.com/redis-enterprise/redis-insight/


INSTALLATION
##############

Install the ``predis/predis`` package via Composer:

.. code-block:: bash

        composer require predis/predis


WORKFLOW OF INTEGRATION OF LIBRARY
#############################################

.. image:: images/predisWorkflow.png


HELLO WORLD/POC
#################

1. Create a new laravel project

.. code-block:: bash

        composer create-project laravel/laravel predis-demo --prefer-dist

2. Install the ``predis/predis`` package via Composer:

.. code-block:: bash

        composer require predis/predis


3. Configure redis connection settings in ``.env`` file

.. code-block:: bash

        REDIS_HOST=127.0.0.1
        REDIS_PASSWORD=qwerty
        REDIS_PORT=6379
        QUEUE_CONNECTION=redis
        REDIS_CLIENT=predis

4. Create a UserController class

.. code-block:: bash

        php artisan make:controller UserController

Lets store a key-value pair in the redis database using ``predis`` library

.. code-block:: php

        <?php

        namespace App\Http\Controllers;


        class UserController extends Controller
        {
            public function store(){

                // setup redis connection
                $redis = app()->make("redis");

                // sets message to contian "Hello world"
                $redis->set('message', 'Hello world');

                // gets the value of message
                $value = $redis->get('message');

                print($value);
            }
        }

5. Register route in ``web.php`` file

.. code-block:: bash

        Route::get('/demo',[\App\Http\Controllers\UserController::class,'store']);


6. Execute the code

.. code-block:: bash

        php artisan serve

http://localhost:8000/demo

Browser Output:

.. image:: images/redisBrOutput.png


One can check the redis database


.. image:: images/redisInsight1.png






