MIDDLEWARE
============

Middleware provides a convenient mechanism for inspecting and filtering HTTP requests entering your application.

In this example, we will create one custom middleware that will allow active users to access pages.

CREATE LARAVEL PROJECT
-----------------------

**Step 1 :**

Run the following command to create project :

.. code-block:: bash

   composer create-project laravel/laravel example-app

CREATE MIDDLEWARE
--------------------

**Step 2 :**

Create custom middleware file

.. code-block:: bash

   php artisan make:middleware IsActive

Update following code on this file.

app/Http/Middleware/IsActive.php

.. code-block:: php


   <?php

   namespace App\Http\Middleware;

   use Closure;
   use Illuminate\Http\Request;
   use Symfony\Component\HttpFoundation\Response;

   class IsActive
   {
    /**
     * Handle an incoming request.
     *
     * @param  \Closure(\Illuminate\Http\Request): (\Symfony\Component\HttpFoundation\Response)  $next
     */
    public function handle(Request $request, Closure $next): Response
    {
        if (!auth()->user()->is_active) {
            return response()->json('Your account is inactive');
        }

        return $next($request);
    }
   }

REGISTER MIDDLEWARE
-----------------------

**Step 3 :**

We need to register middleware on Kernel.php file. We will call is-active of new created middleware.

Update the following file : app/Http/Kernel.php

.. code-block:: php

   <?php

   namespace App\Http;

   use Illuminate\Foundation\Http\Kernel as HttpKernel;

   class Kernel extends HttpKernel
   {
    ....

    /**
     * The application's route middleware.
     *
     * These middleware may be assigned to groups or used individually.
     *
     * @var array
     */
    protected $routeMiddleware = [
        ....
        'is-active' => \App\Http\Middleware\IsActive::class,
    ];
   }

USE MIDDLEWARE
--------------------

**Step 4 :**

In this step, we will create one route and show you how to use middleware in route file.

Update following code in the route file : routes/web.php

.. code-block:: php

   <?php

   use Illuminate\Support\Facades\Route;

   use App\Http\Controllers\RSSFeedController;


   Route::get('/test-user', [App\Http\Controllers\HomeController::class, 'index'])->middleware(['auth', 'is-active']);

CREATE AUTH SCAFFOLDING
---------------------------

**Step 5 :**

Create auth scaffolding, install laravel ui package and generate auth scaffolding.

First you need to install laravel/ui package :

.. code-block:: bash

   composer require laravel/ui

Need to generate auth scaffolding in laravel using laravel ui command.

.. code-block:: bash

   php artisan ui bootstrap --auth

Now you need to run npm command.

Install NPM:

.. code-block:: bash

   npm install

Run NPM:

.. code-block:: bash

   npm run dev

ADD IS_ACTIVE COLUMN
---------------------

**Step 6 :**

Create new migration to add is_active column in users table.

.. code-block:: bash

   php artisan make:migration add_is_active_column

Update below code to migration file : database/migrations/add_is_active_column.php

.. code-block:: php

   <?php

   use Illuminate\Database\Migrations\Migration;
   use Illuminate\Database\Schema\Blueprint;
   use Illuminate\Support\Facades\Schema;

   return new class extends Migration
   {
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up(): void
    {
        Schema::table('users', function (Blueprint $table) {
            $table->boolean('is_active')->default(0);
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down(): void
    {

    }
   };

Run migration :

.. code-block:: bash

    php artisan migrate

Add is_active column in fillable array of user model. Update it : app/Models/User.php

.. code-block:: php

   <?php

   namespace App\Models;

   use Illuminate\Contracts\Auth\MustVerifyEmail;
   use Illuminate\Database\Eloquent\Factories\HasFactory;
   use Illuminate\Foundation\Auth\User as Authenticatable;
   use Illuminate\Notifications\Notifiable;
   use Laravel\Sanctum\HasApiTokens;

   class User extends Authenticatable
   {
    use HasApiTokens, HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array

     */
    protected $fillable = [
        'name',
        'email',
        'password',
        'is_active'
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array

     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * The attributes that should be cast.
     *
     * @var array

     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
   }

CREATE SEEDER
--------------

**Step 7 :**

Create UserSeeder to generate active and inactive users default.

.. code-block:: bash

   php artisan make:seeder UserSeeder

Put below code in UserSeeder file : database/seeders/UserSeeder.php

.. code-block:: php

   <?php

   namespace Database\Seeders;

   use Illuminate\Database\Console\Seeds\WithoutModelEvents;
   use Illuminate\Database\Seeder;
   use App\Models\User;
   use Illuminate\Support\Facades\Hash;

   class UserSeeder extends Seeder
   {
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run(): void
    {
        User::create([
            "name" => "One",
            "email" => "one@gmail.com",
            "password" => 12345678,
            "is_active" => 1
        ]);

    }
    }

Run below command to run UserSeeder

.. code-block:: bash

   php artisan db:seed --class=UserSeeder

Final Step : Run Laravel

.. code-block:: php

   php artisan serve

Go to your web browser, type the given URL and view the app output :

.. code-block:: bash

   http://localhost:8000/login

.. image:: images/img.png




