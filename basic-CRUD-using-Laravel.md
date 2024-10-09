# Implementing Basic CRUD Functionality Using Laravel

The following practical instructions are a quick start. You should refer to the Laravel website for comprehensive info on building laravel applications, or https://laracasts.com/series/30-days-to-learn-laravel-11 for a more in-depth introduction.

Make sure you have followed the instructions for installing Composer and creating a Laravel project getting_started.md.

Open the Laravel project folder in VS Code. You should see lots of folders and files in the explorer panel (The top one should be App).

## Routes

The first thing to understand when working with Laravel is how routing works, how the URL in the browser maps to code that will be executed. Open _routes/web.php_. This is where the routes for an application are defined. Add the following route at the end of this file.

```php
Route::get('/films', function () {
    return "Display all the films.";
});
```

In a browser enter _http://localhost/films_, you should see the 'Display all the films.' message. Next, add a few more routes:-

```php

Route::get('/films/create', function () {
    return "Add a new film";
});

Route::get('/films/about', function () {
    return "About the app";
});


```

This routes file is similar to the front controller we used previously. There are some key differences:-

- OO approach
- Cleaner URIs
- Patch and delete methods

This code is fine for testing how routes work, but really we want to call a controller from the routes file.

## Controller

Before creating a controller, we need to know about Artisan.

### Artisan

Artisan is a CLI (Command Line Interface) that comes with Laravel. It is used to automate certain tasks for us. One task it can automate is creating controller classes. Here's how to use Artisan.

- From the XAMPP control panel click 'shell'. A command prompt should appear.
- We need to navigate to the laravel installation directory. To do this enter the following commands:

```
cd htdocs
```

```
cd film-app
```

- Next we will instruct artisan to make a controller for us. Enter the following command:

```
php artisan make:controller FilmController
```

- If you look inside the _app/Http/Controllers_ folder you should see a _FilmController.php_ file.
- Add `index()`, `create()` and `about()` methods in the controller i.e.

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class FilmController extends Controller
{
    function index()
    {
        return "Display all the films";
    }

    function create()
    {
        return "Add a new film";
    }

    function about()
    {
        return "About the amazing film app";
    }
}
```

Change the routes file so that it calls the `FilmController` methods.

```php
use App\Http\Controllers\FilmController;
use Illuminate\Support\Facades\Route;

Route::get('/films', [FilmController::class, 'index']);
Route::get('/films/create', [FilmController::class, 'create']);
Route::get('/films/about', [FilmController::class, 'about']);
```

Test this works, the messages are now coming from the controller and not the routes file

> What is `::class`. It's a PHP feature that makes it easy to get hold of class names see https://stackoverflow.com/questions/30770148/what-is-class-in-php for more information.

## Views

In an MVC structure we don't want our controllers to generate output, instead we should be using views.

- In the _resources/views_ folder, add a new folder, call it _films_.
- Create a new file, name it _index.blade.php_, and save it in the _films_ folder. We must use the _.blade.php_ suffix bit.
  > In Laravel, views are written using the Blade templating language. This makes it easier to integrate PHP code into our view files.
- Paste in the following code into _index.blade.php_.

```HTML
<!DOCTYPE HTML>
<html>
<head>
<title>List the films</title>
<meta http-equiv="content-type" content="text/html;charset=utf-8">
<link href="{{asset('CSS/style.css')}}" type="text/css" rel="stylesheet">
</head>
<body>
<nav>
    <ul>
    <li><a href="index.php">Home</a></li>
    <li><a href="create.php">Add new film</a></li>
    <li><a href="about.php">About</a></li>
</ul>
</nav>

<h1>Here's a list of films</h1>

<p>The films will appear here.</p>

</body>
</html>
```

- Save this file.
- In _FilmController_ change the `index()` method so it loads this view i.e.

```php
    function index()
    {
        return view('films.index');
    }
```

- If you visit localhost/films, your view file should be loaded.

### Adding some CSS

Find the public folder, inside this folder add a new folder and name it css.

Create a new file style.css
Save it in this css folder
Add the following simple CSS code to style.css

```css
body {
  background-color: lightsalmon;
  font-family: Arial, Helvetica, sans-serif;
}
button {
  text-transform: uppercase;
}
```

- Refresh localhost/films. Your page should now have some styling.

Have a look in index.blade.view at the link element

<link href="{{asset('css/style.css')}}" type="text/css" rel="stylesheet">

This uses blade to load the CSS file. The double curly braces are used to call PHP code and echo it. asset is a helper function in Laravel that is used to load assets.

## Creating a Layout File

There are a number of different ways to do this, the most recent way is to use components - resuable user interface elements. We will create a layout component. In the views folder, add a new folder and name it components.
Create a new file, name it layout.blade.php and save it in the components folder.

Paste the following into layout.blade.php

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{$title}}</title>
    <meta http-equiv="content-type" content="text/html;charset=utf-8" />
    <link href="{{asset('css/style.css')}}" type="text/css" rel="stylesheet" />
  </head>
  <body>
    <nav>
      <ul>
        <li><a href="/films">Home</a></li>
        <li><a href="/films/create">Add new film</a></li>
        <li><a href="/films/about">About</a></li>
      </ul>
    </nav>
    {{$slot}}
  </body>
</html>
```

This is like a master page or template that we will base all the other pages one. It is plain HTML but with two variables, $title which will displaya custom page title and $slot which will display the custom content.

Go back to index.blade.php and Change it to

```html
<x-layout title="List the films">
  <h1>Here's a list of films</h1>
  <p>The films will appear here.</p>
</x-layout>
```

Any tags that start with `<x-` are components in Laravel. We then use the file name (layout) to specify the component.
Anything between the opening and clsoing `</x-layout>` tags will be passed into `$slot`. This is a default variable.
We have also added a custom attribute title which is used to specify a page title.

Save the files and test the view still works.

### Adding the create View

Add a create view

```html
<x-layout title="Add new film">
  <h1>Add a new film</h1>

  <form method="POST" action="/films">
    @csrf
    <div>
      <label for="title">Title:</label>
      <input type="text" id="title" name="title" />
    </div>
    <div>
      <label for="year">Year:</label>
      <input type="text" id="year" name="year" />
    </div>
    <div>
      <label for="duration">Duration:</label>
      <input type="text" id="duration" name="duration" />
    </div>
    <div>
      <button type="submit">SAVE THE FILM</button>
    </div>
  </form>
</x-layout>
```

TODO CSRF

Back in FilmController.php, load this view from the create() method

```php
    function create()
    {
        return view('films.create');
    }


```

The form won't work yet, but you should be able to access the create view.

- On your own add a view for the About page. Make sure this view is loaded from the FilmController.

## Setting up the Database

First we need to delete our exsiting films table from our database.
In phpmyadmin, select your films table
Select operations
Scroll down to the bottom of the page and select drop to delete the table.

### Specifiying the database settings

Back in VS Code, open the .env file.
Find the database settings in this file and edit them to specify mysql as the database, and your database, username and passord e.g.

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=cht2520
DB_USERNAME=cht2520
DB_PASSWORD=letmein
```

Save this file

### Creating a Database Migration

Laravel allows us to define database schemas using PHP code. We call these migrations. This makes it very easy to undo changes to the structure of our database and tables, and to completely re-design and re-create our tables without having to import/export .sql files.

We will use Artisan to generate our migrations. Make sure the XAMPP shell is open and you are in the films-app project directory. Enter the following command:

```
php artisan make:migration create_films_table --create=films
```

Look in the database/migrations folder, open the create_films_table.php file. You should see that an id column has been defined for us already.

Modify this file to add definitions for title, year and duration columns. Your create_films_table.php file should then look like the following:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('films', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->smallInteger('year');
            $table->smallInteger('duration');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('films');
    }
};
```

Essentially this code creates a films table for us and defines four columns. The timestamps column is optional but is included by default in every Laravel migration. To run the migration enter the following into the shell:

```
php artisan migrate
```

Back in phpmyadmin confirm that the films (and some other tables) have been created.
Laravel creates lots of tables for us e.g. users for storing user details. We don't need to worry about these at the moment.

### Seeding the Database

In addition to creating migrations we can also seed the database, Laravel will populate our database tables with some sample data.

Again, we will use Artisan, this time to create a seeder. Enter the following command into the XAMPP shell window.

```
php artisan make:seeder FilmSeeder
```

Have a look in the database/seeders folder and open FilmSeeder.php.

Modify this file to specify some films for the database. Here's an example:

```php
namespace Database\Seeders;

use Illuminate\Support\Facades\DB;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class FilmsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run()
    {
        DB::table('films')->insert(['title' => 'Jaws', 'year' => '1975', 'duration' => 124]);
        DB::table('films')->insert(['title' => 'Winter\'s Bone', 'year' => '2010', 'duration' => 100]);
        DB::table('films')->insert(['title' => 'Do The Right Thing', 'year' => '1989', 'duration' => 120]);
    }
}
```

Next, we need to tell the DatabaseSeeder class to run the FilmsTableSeeder. From the seeds folder open DatabaseSeeder.php.
Modify it so that it runs our FilmSeeder i.e.

```php
namespace Database\Seeders;

use App\Models\User;
// use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     */
    public function run(): void
    {
        $this->call([
            FilmSeeder::class,
        ]);
    }
}
```

Back in Artisan run the following command

```
php artisan db:seed
```

Back in phpmyadmin check your films table has some films in it.

### TODO

How to rollback and reset the database.

## Models

Again, we will use Artisan to generate our model classes. In the XAMPP shell enter the following:

```
php artisan make:model Film
```

- Have a look in the app/Models folder, you should find a Film class has been generated.
- Laravel uses something called Eloquent ORM for object-relational mapping. As we will see, it is based on the Active Record pattern.

Convention over Configuration

> Laravel works on conventions. If we create a model class called Film, it will assume that this maps to a database table called 'films'. We don't have to do anything else to set up the object-relational mapping.

## Tying everything together

### Displaying all the films

- Open _FilmController.php_.
- Add a use statement to import the Film class.

```php
use App\Models\Film;
```

- Modify the index method to use the Film class.

```php
function index()
{
    $films = Film::all();
    return view('film/list-view',['films' => $films]);
}
```

- Hopefully, you can see how the active record pattern is working. We want a list of all the films, so we call the `all()` method. This has been defined automatically for us by the Eloquent ORM (https://laravel.com/docs/eloquent).
- Also note that the `all()` method returns an array of film objects, and this array is passed to the view. Open _index.blade.php_ (from the resources/views/films folder). Modify it so that it uses this array of film objects:

```php
<x-layout title="List the films">
    <h1>Here's a list of films</h1>
    @foreach ($films as $film)
    <p>
        <a href="/films/{{$film->id}}">
            {{$film->title}}
        </a>
    </p>
    @endforeach
</x-layout>
```

- To echo out the value of a variable we simply use the double curly brackets {{...}}
- Note the use of @foreach and @if. These are part of the blade templating engine, and make it easy for us to include control structures in our views.
  Test this works. You should see a list of films.

### Getting create to work

If you look in create.blade.php you should see that the form will be submitted to a route of _films_ using the POST method.

```
<form method="POST" action="/films">
    @csrf
    <div>
```

The @crsf directive is needed everytime we create a form. This protects us from Cross-site request forgery. Read about it here - https://laravel.com/docs/11.x/csrf#main-content.

#### Adding a new route

Back in routes.web add another route for storing the new film

```php
Route::post('/films', [FilmController::class, 'store']);
```

We can use the same route (_/films_) because we are using a different method (POST) Laravel can distinguish between the two.

#### Adding a store() method to the controller

Open _FilmController.php_.
Add a `store()` method.

```php
function store()
    {

        $film = new Film();
        $film->title = request('title');
        $film->year = request('year');
        $film->duration = request('duration');
        $film->save();
        return redirect('/films');
    }
```

The Laravel `request()` function allows us to access the form data. We then simply ask the Film model to save the film, and redirect to the home page.

## Implementing show

Add the following route to the routes/web file

```php
Route::get('/films/{id}', [FilmController::class, 'show']);
```

Add the following `show()` method to the controller

```php
function show($id)
{
    $film = Film::find($id);
    return view('films.show', ['film' => $film]);
}
```

Add the following _show.blade.php_ view to the _resources/views/films_ folder

```php
<x-layout title="Show the details for a film">
    <h1>{{$film->title}}</h1>
    <p>Year:{{$film->year}}</p>
    <p>Duration:{{$film->duration}}</p>

    <a href='/films/{{$film->id}}/edit'>
        <button>Edit</button>
    </a>

    <form method='POST' action='/films'>
        @csrf
        @method('DELETE')
        <input type="hidden" name="id" value="<?php echo $film['id']; ?>">
        <button type='submit'>Delete</button>
    </form>
</x-layout>
```

- Check this works

## Implementing Edit

First set-up the route

```php
Route::get('/films/{id}/edit', [FilmController::class, 'edit']);
```

Next, add the edit method to the FilmController

```php
    function edit($id)
    {
        $film = Film::find($id);
        return view('films.edit', ['film' => $film]);
    }
```

Next, create the edit view

```php
<x-layout title="Edit a film">
    <h1>Edit the details for {{$film->title}}</h1>
    <form action="/films" method="POST">
        @csrf
        @method('PATCH')
        <!--
	A hidden field (not visible to the user) inspect the page or view source in the HTML page to see it.
	The field contains the id number of the film.
-->
        <input type="hidden" name="id" value="{{$film->id}}">
        <div>
            <label for="title">Title:</label>
            <!--
    The text boxes are populated with values from the database ready for the user to edit
-->
            <input type="text" id="title" name="title" value="{{$film->title}}">
        </div>
        <div>
            <label for="year">Year:</label>
            <input type="text" id="year" name="year" value="{{$film->year}}">
        </div>
        <div>
            <label for="duration">Duration:</label>
            <input type="text" id="duration" name="duration" value="{{$film->duration}}">
        </div>
        <div>
            <button type="submit">Save Changes</button>
        </div>
    </form>
</x-layout>
```

Save this in the resources/views/films folder as edit.blade.php

Note that we specify a method of PATCH

```
@method('PATCH')
```

Previously we have used GET and POST http methods to pass data. There are many more http methods e.g. PUT, PATCH, DELETE. However, browsers only support GET and POST. This line of code allows us to tell Laravel we are wanting to perform a PATCH request (equivalent to update). See https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH

The advantage is we can have cleaner URIs.

## Implementing Update

Add a route to handle the PATCH request

```php
Route::patch('/films', [FilmController::class, 'update']);
```

Notice the use of `patch`. We can use the same URI i.e. localhost/films, but depending on the method (GET,POST,PATCH) we call different controller methods.

Add the `update()` method to the controller

```php
function update()
{
    $film = Film::find(request('id'));
    $film->title = request('title');
    $film->year = request('year');
    $film->duration = request('duration');
    $film->save();
    return redirect('/films');
}
```

- Test this works

On Your Own

Add you implement the delete functionality.

Have a look in the show.blade.php file. See how we have specifed a method of delete for the delete form

```

```

Add a route that will handle this request
Add a destroy() method to the controller
This need to get hold of the id number of the selected film
Use eloquent to find the film
and then delete it e.g. https://laravel.com/docs/11.x/eloquent#deleting-models
redirect back to the homepage
