AUTHENTICATION
==================

In this documentation we will implement Laravel custom authentication login and registeration.

CREATE LARAVEL PROJECT
--------------------------

**Step 1**

Run the following command to create laravel project :

.. code-block:: bash

   composer create-project --prefer-dist laravel/laravel authentication

SETTING UP DATABASE CREDENTIALS
----------------------------------

**Step 2**

Update database credentials in .env file :

.. code-block::

   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3307
   DB_DATABASE=laravel
   DB_USERNAME=root
   DB_PASSWORD=Qwerty@321

Update your user table migration which can be found in > {project_folder}/database/migrations/2014_10_12_000000_create_users_table.php and add the username field then update the name field to nullable so that in registration we require the email, username, and password.

.. code-block:: php

   Schema::create('users', function (Blueprint $table) {
     $table->id();
     $table->string('name')->nullable();
     $table->string('email')->unique();
     $table->string('username')->unique();
     $table->timestamp('email_verified_at')->nullable();
     $table->string('password');
     $table->rememberToken();
     $table->timestamps();
   });


After updating run the following command :

.. code-block:: bash

   php artisan migrate:fresh --seed


SETTING UP MODEL
------------------

**Step 3**

Next, we need to modify our App/Models/User.php model and add the username as fillable.

.. code-block:: php

   /**
    * The attributes that are mass assignable.
    *
    * @var array
   */
   protected $fillable = [
  'name',
  'email',
  'username',
  'password',
   ];

Then next, we will add this method setPasswordAttribute so that for every user registered, the password will always be encrypted.

.. code-block:: php

   /**
    * Always encrypt the password when it is updated.
    *
    * @param $value
    * @return string
    */
   public function setPasswordAttribute($value)
   {
   $this->attributes['password'] = bcrypt($value);
   }

SETTING UP CONTROLLERS AND REQUEST CLASSES
--------------------------------------------

**Step 4: Setup Registration Controller**

To set up the registration controller :

.. code-block:: bash

   php artisan make:controller RegisterController

Add the following code in RegisterController :

.. code-block:: php

   <?php

   namespace App\Http\Controllers;

   use App\Models\User;
   use Illuminate\Http\Request;
   use App\Http\Requests\RegisterRequest;

   class RegisterController extends Controller
   {
    /**
     * Display register page.
     *
     * @return \Illuminate\Http\Response
     */
    public function show()
    {
        return view('auth.register');
    }

    /**
     * Handle account registration request
     *
     * @param RegisterRequest $request
     *
     * @return \Illuminate\Http\Response
     */
    public function register(RegisterRequest $request)
    {
        $user = User::create($request->validated());

        auth()->login($user);

        return redirect('/')->with('success', "Account successfully registered.");
    }
   }

**Step 5: Setup Registration Request**

We need to separate our request validation for our registration process, create RegisterRequest :

.. code-block:: bash

   php artisan make:request RegisterRequest


Add the following code in RegisterRequest :

.. code-block:: php

   <?php

   namespace App\Http\Requests;

   use Illuminate\Foundation\Http\FormRequest;

   class RegisterRequest extends FormRequest
   {
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'email' => 'required|email:rfc,dns|unique:users,email',
            'username' => 'required|unique:users,username',
            'password' => 'required|min:8',
            'password_confirmation' => 'required|same:password'
        ];
    }
   }

**Step 6: Setup Login Controller**

To set up the login controller :

.. code-block:: bash

   php artisan make:controller LoginController


Add the following code in LoginController :

.. code-block:: php

   <?php

   namespace App\Http\Controllers;

   use Illuminate\Http\Request;
   use App\Http\Requests\LoginRequest;
   use Illuminate\Support\Facades\Auth;

   class LoginController extends Controller
   {
    /**
     * Display login page.
     *
     * @return Renderable
     */
    public function show()
    {
        return view('auth.login');
    }

    /**
     * Handle account login request
     *
     * @param LoginRequest $request
     *
     * @return \Illuminate\Http\Response
     */
    public function login(LoginRequest $request)
    {
        $credentials = $request->getCredentials();

        if(!Auth::validate($credentials)):
            return redirect()->to('login')
                ->withErrors(trans('auth.failed'));
        endif;

        $user = Auth::getProvider()->retrieveByCredentials($credentials);

        Auth::login($user);

        return $this->authenticated($request, $user);
    }

    /**
     * Handle response after user authenticated
     *
     * @param Request $request
     * @param Auth $user
     *
     * @return \Illuminate\Http\Response
     */
    protected function authenticated(Request $request, $user)
    {
        return redirect()->intended();
    }
    }


**Step 7: Setup Login Request**

Create LoginRequest :

.. code-block:: bash

   php artisan make:request LoginRequest


Add the following code in LoginRequest :

.. code-block:: php

   <?php

   namespace App\Http\Requests;

   use Illuminate\Foundation\Http\FormRequest;
   use Illuminate\Contracts\Validation\Factory as ValidationFactory;

   class LoginRequest extends FormRequest
   {
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return boolNow, you already generated our
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'username' => 'required',
            'password' => 'required'
        ];
    }

    /**
     * Get the needed authorization credentials from the request.
     *
     * @return array
     * @throws \Illuminate\Contracts\Container\BindingResolutionException
     */
    public function getCredentials()
    {
        // The form field for providing username or password
        // have name of "username", however, in order to support
        // logging users in with both (username and email)
        // we have to check if user has entered one or another
        $username = $this->get('username');

        if ($this->isEmail($username)) {
            return [
                'email' => $username,
                'password' => $this->get('password')
            ];
        }

        return $this->only('username', 'password');
    }

    /**
     * Validate if provided parameter is valid email.
     *
     * @param $param
     * @return bool
     * @throws \Illuminate\Contracts\Container\BindingResolutionException
     */
    private function isEmail($param)
    {
        $factory = $this->container->make(ValidationFactory::class);

        return ! $factory->make(
            ['username' => $param],
            ['username' => 'email']
        )->fails();
    }
   }

**Step 8: Setup Logout Controller**

To set up the logout controller :

.. code-block:: bash

   php artisan make:controller LogoutController

Add the following code in LogoutController :

.. code-block:: php

   <?php

   namespace App\Http\Controllers;

   use Illuminate\Http\Request;
   use Illuminate\Support\Facades\Auth;
   use Illuminate\Support\Facades\Session;

   class LogoutController extends Controller
   {
    /**
     * Log out account user.
     *
     * @return \Illuminate\Routing\Redirector
     */
    public function perform()
    {
        Session::flush();

        Auth::logout();

        return redirect('login');
    }
   }

**Step 9: Setup Home Controller**

To set up the home controller :

.. code-block:: bash

   php artisan make:controller HomeController

Add the following code in HomeController :

.. code-block:: php

   <?php

   namespace App\Http\Controllers;

   use Illuminate\Http\Request;

   class HomeController extends Controller
   {
    public function index()
    {
        return view('home.index');
    }
   }

SETTING UP ROUTES
--------------------

**Step 10: Setup Routes**

Set up routes in routes/web.php :

.. code-block:: php

   <?php

   use Illuminate\Support\Facades\Route;

   /*
   |--------------------------------------------------------------------------
   | Web Routes
   |--------------------------------------------------------------------------
   |
   | Here is where you can register web routes for your application. These
   | routes are loaded by the RouteServiceProvider within a group which
   | contains the "web" middleware group. Now create something great!
   |
   */

  Route::group(['namespace' => 'App\Http\Controllers'], function()
   {
    /**
     * Home Routes
     */
    Route::get('/', 'HomeController@index')->name('home.index');

    Route::group(['middleware' => ['guest']], function() {
        /**
         * Register Routes
         */
        Route::get('/register', 'RegisterController@show')->name('register.show');
        Route::post('/register', 'RegisterController@register')->name('register.perform');

        /**
         * Login Routes
         */
        Route::get('/login', 'LoginController@show')->name('login.show');
        Route::post('/login', 'LoginController@login')->name('login.perform');

    });

    Route::group(['middleware' => ['auth']], function() {
        /**
         * Logout Routes
         */
        Route::get('/logout', 'LogoutController@perform')->name('logout.perform');
    });
   });

SETTING UP VIEWS
------------------

**Step 11: Setup Views**

You need to create a layouts folder inside resources/views then create a file resources/views/layouts/auth-master.blade.php and add the following code :

.. code-block:: html

   <!doctype html>
   <html lang="en">
   <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="Mark Otto, Jacob Thornton, and Bootstrap contributors">
    <meta name="generator" content="Hugo 0.87.0">
    <title>Signin Template · Bootstrap v5.1</title>

    <!-- Bootstrap core CSS -->
    <link href="{!! url('assets/bootstrap/css/bootstrap.min.css') !!}" rel="stylesheet">
    <link href="{!! url('assets/css/signin.css') !!}" rel="stylesheet">

    <style>
      .bd-placeholder-img {
        font-size: 1.125rem;
        text-anchor: middle;
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;
      }

      @media (min-width: 768px) {
        .bd-placeholder-img-lg {
          font-size: 3.5rem;
        }
      }
    </style>


    <!-- Custom styles for this template -->
    <link href="signin.css" rel="stylesheet">
   </head>
   <body class="text-center">

    <main class="form-signin">

        @yield('content')

    </main>


   </body>
   </html>


Next, create a file resources/views/layouts/app-master.blade.php and add the following code :

.. code-block:: html


   <!doctype html>
   <html lang="en">
    <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="Mark Otto, Jacob Thornton, and Bootstrap contributors">
    <meta name="generator" content="Hugo 0.87.0">
    <title>Fixed top navbar example · Bootstrap v5.1</title>

    <!-- Bootstrap core CSS -->
    <link href="{!! url('assets/bootstrap/css/bootstrap.min.css') !!}" rel="stylesheet">

    <style>
      .bd-placeholder-img {
        font-size: 1.125rem;
        text-anchor: middle;
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;
      }

      @media (min-width: 768px) {
        .bd-placeholder-img-lg {
          font-size: 3.5rem;
        }
      }
    </style>


    <!-- Custom styles for this template -->
    <link href="{!! url('assets/css/app.css') !!}" rel="stylesheet">
   </head>
   <body>

    @include('layouts.partials.navbar')

    <main class="container">
        @yield('content')
    </main>

    <script src="{!! url('assets/bootstrap/js/bootstrap.bundle.min.js') !!}"></script>

   </body>
   </html>


Next, create a new folder inside resources/views/layouts called partials folder then create a file resources/views/layouts/partials/navbar.blade.php and add the following code :

.. code-block:: html


   <header class="p-3 bg-dark text-white">
   <div class="container">
    <div class="d-flex flex-wrap align-items-center justify-content-center justify-content-lg-start">
      <a href="/" class="d-flex align-items-center mb-2 mb-lg-0 text-white text-decoration-none">
        <svg class="bi me-2" width="40" height="32" role="img" aria-label="Bootstrap"><use xlink:href="#bootstrap"/></svg>
      </a>

      <ul class="nav col-12 col-lg-auto me-lg-auto mb-2 justify-content-center mb-md-0">
        <li><a href="#" class="nav-link px-2 text-secondary">Home</a></li>
        <li><a href="#" class="nav-link px-2 text-white">Features</a></li>
        <li><a href="#" class="nav-link px-2 text-white">Pricing</a></li>
        <li><a href="#" class="nav-link px-2 text-white">FAQs</a></li>
        <li><a href="#" class="nav-link px-2 text-white">About</a></li>
      </ul>

      <form class="col-12 col-lg-auto mb-3 mb-lg-0 me-lg-3">
        <input type="search" class="form-control form-control-dark" placeholder="Search..." aria-label="Search">
      </form>

      @auth
        {{auth()->user()->name}}
        <div class="text-end">
          <a href="{{ route('logout.perform') }}" class="btn btn-outline-light me-2">Logout</a>
        </div>
      @endauth

      @guest
        <div class="text-end">
          <a href="{{ route('login.perform') }}" class="btn btn-outline-light me-2">Login</a>
          <a href="{{ route('register.perform') }}" class="btn btn-warning">Sign-up</a>
        </div>
      @endguest
     </div>
   </div>
   </header>


Then next, create a file resources/views/layouts/partials/messages.blade.php and add the following code :

.. code-block:: html


   @if(isset ($errors) && count($errors) > 0)
    <div class="alert alert-warning" role="alert">
        <ul class="list-unstyled mb-0">
            @foreach($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
   @endif

   @if(Session::get('success', false))
    <?php $data = Session::get('success'); ?>
    @if (is_array($data))
        @foreach ($data as $msg)
            <div class="alert alert-warning" role="alert">
                <i class="fa fa-check"></i>
                {{ $msg }}
            </div>
        @endforeach
    @else
        <div class="alert alert-warning" role="alert">
            <i class="fa fa-check"></i>
            {{ $data }}
        </div>
    @endif
   @endif

Then next, create  auth folder inside resources/views then create a file resources/views/auth/register.blade.php and add the following code :

.. code-block:: html

   @extends('layouts.auth-master')

   @section('content')
    <form method="post" action="{{ route('register.perform') }}">

        <input type="hidden" name="_token" value="{{ csrf_token() }}" />
        <img class="mb-4" src="{!! url('images/bootstrap-logo.svg') !!}" alt="" width="72" height="57">

        <h1 class="h3 mb-3 fw-normal">Register</h1>

        <div class="form-group form-floating mb-3">
            <input type="email" class="form-control" name="email" value="{{ old('email') }}" placeholder="name@example.com" required="required" autofocus>
            <label for="floatingEmail">Email address</label>
            @if ($errors->has('email'))
                <span class="text-danger text-left">{{ $errors->first('email') }}</span>
            @endif
        </div>

        <div class="form-group form-floating mb-3">
            <input type="text" class="form-control" name="username" value="{{ old('username') }}" placeholder="Username" required="required" autofocus>
            <label for="floatingName">Username</label>
            @if ($errors->has('username'))
                <span class="text-danger text-left">{{ $errors->first('username') }}</span>
            @endif
        </div>

        <div class="form-group form-floating mb-3">
            <input type="password" class="form-control" name="password" value="{{ old('password') }}" placeholder="Password" required="required">
            <label for="floatingPassword">Password</label>
            @if ($errors->has('password'))
                <span class="text-danger text-left">{{ $errors->first('password') }}</span>
            @endif
        </div>

        <div class="form-group form-floating mb-3">
            <input type="password" class="form-control" name="password_confirmation" value="{{ old('password_confirmation') }}" placeholder="Confirm Password" required="required">
            <label for="floatingConfirmPassword">Confirm Password</label>
            @if ($errors->has('password_confirmation'))
                <span class="text-danger text-left">{{ $errors->first('password_confirmation') }}</span>
            @endif
        </div>

        <button class="w-100 btn btn-lg btn-primary" type="submit">Register</button>

        @include('auth.partials.copy')
     </form>
   @endsection

Then next, create a file resources/views/auth/login.blade.php and add the following code :

.. code-block:: html

   @extends('layouts.auth-master')

   @section('content')
    <form method="post" action="{{ route('login.perform') }}">

        <input type="hidden" name="_token" value="{{ csrf_token() }}" />
        <img class="mb-4" src="{!! url('images/bootstrap-logo.svg') !!}" alt="" width="72" height="57">

        <h1 class="h3 mb-3 fw-normal">Login</h1>

        @include('layouts.partials.messages')

        <div class="form-group form-floating mb-3">
            <input type="text" class="form-control" name="username" value="{{ old('username') }}" placeholder="Username" required="required" autofocus>
            <label for="floatingName">Email or Username</label>
            @if ($errors->has('username'))
                <span class="text-danger text-left">{{ $errors->first('username') }}</span>
            @endif
        </div>

        <div class="form-group form-floating mb-3">
            <input type="password" class="form-control" name="password" value="{{ old('password') }}" placeholder="Password" required="required">
            <label for="floatingPassword">Password</label>
            @if ($errors->has('password'))
                <span class="text-danger text-left">{{ $errors->first('password') }}</span>
            @endif
        </div>

        <button class="w-100 btn btn-lg btn-primary" type="submit">Login</button>

        @include('auth.partials.copy')
    </form>
   @endsection


Then create a partials folder inside auth folder. Then create a file resources/views/auth/partials/copy.blade.php and add the following code :

.. code-block:: html

   <p class="mt-5 mb-3 text-muted">&copy; {{date('Y')}}</p>

Then last create a home folder inside the resources/views folder then create a file resources/views/home/index.blade.php and add the following code :

.. code-block:: html

   @extends('layouts.app-master')

   @section('content')
    <div class="bg-light p-5 rounded">
        @auth
        <h1>Dashboard</h1>
        <p class="lead">Only authenticated users can access this section.</p>
        <a class="btn btn-lg btn-primary" href="https://codeanddeploy.com" role="button">View more tutorials here &raquo;</a>
        @endauth

        @guest
        <h1>Homepage</h1>
        <p class="lead">Your viewing the home page. Please login to view the restricted data.</p>
        @endguest
    </div>
   @endsection

Now our views are already set up. Next, we need to download bootstrap and save it inside the public/assets directory.

.. image:: images/img_3.png

Now our Laravel authentication with username or email login is ready but we have another small customization the redirect default after login. Which we can find in App/Providers/RouteServiceProvider.php then change the original code below:

.. code-block:: php


   /**
   * The path to the "home" route for your application.
   *
   * This is used by Laravel authentication to redirect users after login.

   * @var string
   */
   public const HOME = '/home';


to this code.

.. code-block:: php


   /**
   * The path to the "home" route for your application.
   *
   * This is used by Laravel authentication to redirect users after login.
   *
   * @var string
   */
   public const HOME = '/';

Update the app/Http/Middleware/Authenticate.php file

.. code-block:: php

   <?php

   namespace App\Http\Middleware;

   use Illuminate\Auth\Middleware\Authenticate as Middleware;
   use Illuminate\Http\Request;

   class Authenticate extends Middleware
   {
    /**
     * Get the path the user should be redirected to when they are not authenticated.
     */
    protected function redirectTo(Request $request): ?string
    {
        return $request->expectsJson() ? null : route('login');
    }
   }


RUN THE SERVER
-------------------


**Step 12 : Run The Development Server**


You will see the homepage if not yet authenticated and will see Login and Sign up menu as shown below

.. code-block:: bash

   php artisan serve

.. code-block::

   http://127.0.0.1:8000/

.. image:: images/img.png

|

But if you are already logged in and authenticated , you will see the dashboard as shown below :

.. image:: images/img_1.png



