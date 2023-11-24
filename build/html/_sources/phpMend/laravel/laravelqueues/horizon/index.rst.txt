============
HORIZON
============

PREREQUISITES
===============

https://phoenixnap.com/kb/docker-redis

https://laravel.com/docs/8.x/queues

INTRODUCTION AND FEATURES
=============================

| Horizon provides a beautiful dashboard and code-driven configuration for your Laravel powered Redis queues.
| Horizon allows you to easily monitor key metrics of your queue system such as job throughput, runtime, and job failures.

| All of your worker configuration is stored in a single, simple configuration file, allowing your configuration to stay
| in source control where your entire team can collaborate.

Src: https://laravel.com/docs/8.x/horizon

CONCEPTS
=============

1. Laravel Queues
2. Redis queue driver
3. Horizon Dashboard

Queue Docs : https://laravel.com/docs/8.x/queues

WORKING OF HORIZON
=======================

.. image:: images/workflows.png

INSTALLATION & GETTING STARTED
================================

You should ensure that your queue connection is set to redis in your queue configuration file.
You may use Composer to install Horizon into your Laravel project:

.. code-block:: bash

	composer require laravel/horizon


After installing Horizon, publish its assets using the horizon install Artisan command:

.. code-block:: bash

	php artisan horizon:install

| Once you have configured your workers in the config/horizon.php configuration file,
| you may start Horizon using the horizon Artisan command.
| This single command will start all of your configured workers:

.. code-block:: bash

    php artisan horizon

You may check the current status of the Horizon process using the horizon:status Artisan command:

.. code-block:: bash

    php artisan horizon:status


LIST OF FUNCTIONALITIES PROVIDED
====================================

**Tags**
| Horizon allows you to assign “tags” to jobs, including mailables, event broadcasts, notifications, and queued event listeners.
| In fact, Horizon will intelligently and automatically tag most jobs depending on the Eloquent models that are attached to the job.

**Notifications**

| If you would like to be notified when one of your queues has a long wait time,
| you may use the Horizon::routeMailNotificationsTo, Horizon::routeSlackNotificationsTo,
| and Horizon::routeSmsNotificationsTo methods. You may call these methods from your application's HorizonServiceProvider.

.. code-block:: bash

    Horizon::routeMailNotificationsTo('example@example.com');
    Horizon::routeSlackNotificationsTo('slack-webhook-url', '#channel');
    Horizon::routeSmsNotificationsTo('15556667777');

**Metrics**
Horizon includes a metrics dashboard which provides information on your job and queue wait times and throughput.
In order to populate this dashboard, you should configure Horizon's snapshot Artisan command to run
every five minutes via your application's scheduler.

.. code-block:: php

    protected function schedule(Schedule $schedule){
        $schedule->command('horizon:snapshot')->everyFiveMinutes();
    }

CONFIGURATION
==============

| After publishing Horizon's assets, its primary configuration file will be located at :pink:`config/horizon.php.`
| This configuration file allows you to configure your worker options and each configuration option includes a description
| of its purpose, so be sure to thoroughly explore this file.

**Balance Options:**

| Horizon allows you to choose from three balancing strategies: simple, auto, and false. The simple strategy,
| which is the configuration file's default, splits incoming jobs evenly between processes:
| 'balance' => 'simple'
| The auto strategy adjusts the number of worker processes per queue based on the current workload of the queue.

**Job Trimming:**
| The horizon configuration file allows you to configure how long recent and failed jobs should be persisted (in minutes).
| By default, recent jobs are kept for one hour while failed jobs are kept for a week:

.. code-block:: bash

	'trim' => [
        'recent' => 60,
        'failed' => 10080,
    ]

HELLO WORLD/POC
==================

1. Create a new laravel project

.. code-block:: bash

    $ composer create-project --prefer-dist laravel/laravel horizonSample
    $ cd horizonSample

2. Configure Horizon

.. code-block:: bash

    $ composer require laravel/horizon
    $ php artisan horizon:install
    $ php artisan queue:table

3. Configure mysql Database .env file. (start mysql container)

.. code-block:: bash

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=test
    DB_USERNAME=root
    DB_PASSWORD=Qwerty@321

4. Configure Redis in .env file

.. code-block:: bash

    QUEUE_CONNECTION=redis
    REDIS_CLIENT=predis

5. Install predis requirement.

.. code-block:: bash

    $ composer require predis/predis

7. start redis docker container

.. code-block:: bash

    $ docker run --name redisContainer -p 6379:6379 --restart always --detach redis

6. Create a sample job class.

.. code-block:: bash

    $ php artisan make:job SampleJob

7. Create a route to start the sample job

.. code-block:: bash

    Route::get('/startJob', function () {

        \App\Jobs\SampleJob::dispatch();

        return "job started";
    });

8. Start horizon service and php server

.. code-block:: bash

    $ php artisan horizon
    $ php artisan serve

9. Hit api call http://127.0.0.1:8000/startJob to dispatch the sample job.

10. Open http://127.0.0.1:8000/horizon/dashboard to view the horizon dashboard.

11. View dashboard to see the status of the job processing.

.. image:: images/horizon_dashboard.png

12. Jobs can also be associated with queues. Below method can be used to assign queue to a job.

.. code-block:: bash

    \App\Jobs\SampleJob::dispatch()->onQueue('sampleQueue');

13. Configure horizon.php to add queue details.

.. code-block:: bash

    'defaults' => [
            'supervisor-1' => [
                'connection' => 'redis',
                'queue' => ['default','sampleQueue'],
                'balance' => 'auto',
                'maxProcesses' => 1,
                'tries' => 1,
                'nice' => 0,
            ],
        ]

14. Restart the server, after updating the configs

.. code-block:: bash

    $ php artisan horizon
    $ php artisan serve

15. Check horizon dashboard to check the queue details.

.. image:: images/horizon_queue.png


.. todo::

     List of APIs that consume this library in podium

     Integration & Unit Tests


