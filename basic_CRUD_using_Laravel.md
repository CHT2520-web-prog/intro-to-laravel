# Implementing Basic CRUD Functionality Using Laravel

The following practical instructions are a quick start. You should refer to the Laravel website (https://laravel.com/) for comprehensive info on building Laravel applications or https://laracasts.com/series/30-days-to-learn-laravel-11 for a more in-depth introduction.

Make sure you have followed the instructions for [installing Composer](installing_composer.md) before attempting the following.

## Preparing your database
- First we need to delete our existing _films_ table from our database (Laravel will create it for us later).
- In phpmyadmin, select your _films_ table
- Select operations
- Scroll down to the bottom of the page and select 'drop' to delete the table.

## Creating a Laravel Project

- Open the XAMPP shell (if using a USB) or terminal (if on your own machine).
- Change directory to the _htdocs_ folder e.g.

```
cd htdocs
```

- To install Laravel enter the following composer command (this will create a Laravel project called _film-app_)

```
composer create-project laravel/laravel film-app
```

- Composer should now download Laravel and download Laravel's dependencies (this may take a bit of time).

  > What if I get a timeout error? Occasionally I have experienced Composer timing out when setting up a Laravel project. If this happens, change the timeout duration by entering the following command
  > `composer config --global process-timeout 2000`. Then try and create your project again.

  > On XAMPP on a PC you might get an error about *The zip extension and unzip command are missing*. To fix this:
  >   - Open your *php.ini* file. You can find this in the PHP folder on XAMPP.
  >   - Find the line (ctrl+f) `;extension=zip`.
  >   - This line is commented out. To uncomment it, delete the semi-colon at the start of this line.
  >   - Save the file.
  >   - Restart Apache and try again to create the Laravel project.

  > On a Mac you might get problems with the permissions on the storage folder in the film-app folder. Change the permissions on this folder to allow read and write access for everyone.
- Once Composer has finished setting up your Laravel project, you can then close the shell window.

- Open the Laravel project folder in VS Code. You should see lots of folders and files in the explorer panel (The top one should be _App_).

## Changing the _DocumentRoot_

- We will change the Apache server settings so that web directory route is the _public_ folder of our Laravel project.
- From the XAMPP control panel, next to Apache, select config. This will open a file called _httpd.conf_.
- In _httpd.conf_ we want to change the _DocumentRoot_ setting. This should be at about line 250 and will look like:-

```
DocumentRoot "/xampp/htdocs"
<Directory "/xampp/htdocs">
```

This specifes the _DocumentRoot_ is the _htdocs_ folder. When the user enters http://localhost Apache runs pages in the _htdocs_ folder.
Change this to:-

```
DocumentRoot "/xampp/htdocs/film-app/public"
<Directory "/xampp/htdocs/film-app/public">
```
> If you are using XAMPP on a Mac you will need to use a different path. You should be able to work this out from the original setting. You wll need to change it to something like
> ```
> DocumentRoot "/Applications/XAMPP/xamppfiles/htdocs/film-app/public"
> <Directory "/Applications/XAMPP/xamppfiles/htdocs/film-app/public"> 
> ```

This changes the _DocumentRoot_ to the _public_ folder in Laravel. Now when the user enters http://localhost Apache will run _index.php_ in this folder.

- Save this file.
- Restart Apache.
- Open a browser.
- Enter http://localhost and you should see the default Laravel page. This means you have successfully created a Laravel project.

> Obviously this means you won't be able to run any of your previous PHP examples, but it is easy to change the _DocumentRoot_ back to the default if you need to do this.

## Routes

The first thing to understand when working with Laravel is how routing works, how the URI in the browser maps to code that will be executed. Open _routes/web.php_. This is where the routes for an application are defined. Add the following route at the end of this file.

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

- Check these work.

Although routing takes a more OO approach (we are calling the static `get()` method of the `Route` class), and it doesn't rely on using a query string, this routes file is similar to the front controller we used previously.

This code is fine for testing how routes work, but really we want to call a controller from the routes file.

## Creating a Controller

Before creating a controller, we need to know about Artisan.

### Artisan

Artisan is a CLI (Command Line Interface) that comes with Laravel. It is used to automate certain tasks for us. One task it can automate is creating controller classes. Here's how to use Artisan.

- From the XAMPP control panel click 'shell'. A command prompt should appear.
- We need to navigate to the Laravel installation directory. To do this enter the following commands:

```
cd htdocs
```

```
cd film-app
```

- Next we will instruct Artisan to make a controller for us. Enter the following command:

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

Test this works, you should find the messages are now coming from the controller and not the routes file.

> What is `::class`. It's a PHP feature that makes it easy to get hold of class names see https://stackoverflow.com/questions/30770148/what-is-class-in-php for more information.

## Views

In an MVC structure we don't want our controllers to generate output, instead we should be using views.

- In the _resources/views_ folder, add a new folder, call it _films_.
- Create a new file, name it _index.blade.php_, and save it in the _films_ folder (we must use the _.blade.php_ suffix).

  > In Laravel, views are written using the Blade templating language (https://laravel.com/docs/11.x/frontend#php-and-blade). Blade makes it easier to integrate PHP code into our view files.

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

<p>The films will eventually appear here.</p>

</body>
</html>
```

- Save this file.
- In _FilmController_ change the `index()` method so it loads this view.

```php
    function index()
    {
        return view('films.index');
    }
```

- If you visit http://localhost/films, your view file should be loaded.

### Adding some CSS

- Find the _public_ folder, inside this folder add a new folder and name it _css_.
- Create a new file _style.css_ and save it in this _css_ folder.
- Add the following simple rules to _style.css_.

```css
body {
  background-color: lightsalmon;
  font-family: Arial, Helvetica, sans-serif;
}
button {
  text-transform: uppercase;
}
```

- Refresh http://localhost/films. Your page should now have some styling.
- Have a look in _index.blade.view_ at the link element.

```html
<link href="{{asset('css/style.css')}}" type="text/css" rel="stylesheet" />
```

This uses blade to load the CSS file. The double curly braces are used to call PHP code and echo it. `asset()` is a helper function in Laravel that is used to load assets.

We could do the same thing using plain PHP code e.g.

```html
<link
  href="<?php echo 'http://localhost/public/css/style.css'; ?>"
  type="text/css"
  rel="stylesheet"
/>
```

However, using blade makes the code cleaner, and the `asset()` helper function makes it easier to specify the location of our CSS file.

## Creating a Layout File

There are a number of different ways to do this, the most recent way is to use components. We will create a layout component. In the _views_ folder, add a new folder and name it _components_. Your _resources_ folder will then look like the following:-

- resources
  - views
    - films
      - index.blade.php
    - components

Create a new file, name it _layout.blade.php_ and save it in the _components_ folder.

Paste the following into _layout.blade.php_.

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

This is like a master page or template that we will base all the other pages one. It is plain HTML but with two variables, `$title` which will display a custom page title and `$slot` which will display the specific content for the page.

Go back to _index.blade.php_ and change it to:-

```html
<x-layout title="List the films">
  <h1>Here's a list of films</h1>
  <p>The films will appear here.</p>
</x-layout>
```

- Any tags that start with `<x-` indicate we want to use a component. We then use the file name (layout) to specify the component we want to use.
- Anything between the opening and closing `</x-layout>` tags will be passed into `$slot`.
- We have also added a custom attribute _title_ which is used to specify a page title and will be passed to the `$title` variable in the layout file.
- Save the files and test the view still works.

### Adding the create View

- Add a create view

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
      <button type="submit">Save the Film</button>
    </div>
  </form>
</x-layout>
```

- Save this as _create.blade.php_ in the _resources/views/films_ folder
- Back in `FilmController.php`, load this view from the `create()` method

```php
    function create()
    {
        return view('films.create');
    }
```

The form won't work yet, but you should be able to access the create view.

- On your own add a view for the About page. Make sure this view is loaded from the _FilmController_.

## Setting up the Database

### Specifying the database settings
- Open the _.env_ file.
- Find the database settings in this file and edit them to specify _mysql_ as the database, and your database, username and password e.g.

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=cht2520
DB_USERNAME=cht2520
DB_PASSWORD=letmein
```

> It's a good idea to store configuration information such as database settings in a separate file that won't be publicly accessible. This is exactly what the _.env_ file is. Laravel will load these settings for us automatically.

- Save this file

### Creating a Database Migration

Laravel allows us to define database schemas using PHP code. We call these migrations. This makes it very easy to undo changes to the structure of our database and tables, and to completely re-design and re-create our tables without having to import/export .sql files.

We will use Artisan to generate our migrations. Make sure the XAMPP shell is open and you are in the _film-app_ project directory. Enter the following command:

```
php artisan make:migration create_films_table --create=films
```

Look in the _database/migrations_ folder, open the _create_films_table.php_ file. You should see that an _id_ column has been defined for us already.

Modify this file to add definitions for _title_, _year_ and _duration_ columns. Your _create_films_table.php_ file should then look like the following:

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

Essentially this code creates a _films_ table for us and defines four columns. The timestamps column is optional but is included by default in every Laravel migration. To run the migration enter the following into the shell:

```
php artisan migrate
```

Back in phpmyadmin confirm that the films (and some other tables) have been created.
Laravel creates lots of tables for us e.g. users for storing user details. We don't need to worry about these at the moment.

### Seeding the Database

In addition to creating migrations we can also seed the database. Laravel will populate our database tables with some sample data.

Again, we will use Artisan, this time to create a seeder. Enter the following command into the XAMPP shell window.

```
php artisan make:seeder FilmSeeder
```

Have a look in the _database/seeders_ folder and open _FilmSeeder.php_.

Modify this file to specify some films for the database. Here's an example:

```php
namespace Database\Seeders;

use Illuminate\Support\Facades\DB;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class FilmSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run()
    {
        DB::table('films')->insert(['title' => 'Jaws', 'year' => 1975, 'duration' => 124]);
        DB::table('films')->insert(['title' => 'Winter\'s Bone', 'year' => 2010, 'duration' => 100]);
        DB::table('films')->insert(['title' => 'Do The Right Thing', 'year' => 1989, 'duration' => 120]);
    }
}
```

Next, we need to tell the _DatabaseSeeder_ class to run the _FilmSeeder_. From the _seeders_ folder open _DatabaseSeeder.php_.
Modify it so that it runs our _FilmSeeder_ i.e.

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

- Back in the shell, run the following command

```
php artisan db:seed
```

- In phpmyadmin check your _films_ table has some films in it.

> Sometimes we want to re-build the database e.g. we've got lots of nonsense data from testing, or we've changed the schema. If we use the `migrate:fresh` command we can drop all tables and re-run the migrations and seeding e.g.
> `php artisan migrate:fresh --seed`.

## Models

Again, we will use Artisan to generate our model classes. In the XAMPP shell enter the following:

```
php artisan make:model Film
```

- Have a look in the _app/Models_ folder, you should find that a _Film_ class has been generated.
- Laravel uses something called Eloquent (https://laravel.com/docs/11.x/eloquent) for object-relational mapping. As we will see, it is based on the Active Record pattern.

> **Convention over Configuration.** Laravel works on conventions. If we create a model class called _Film_, it will assume that this maps to a database table called 'films'. We don't have to do anything else to set up the object relational mapping.

## Tying everything together - Displaying all the films

- Open _FilmController.php_.
- Add a use statement to import the Film class.

```php
use App\Models\Film;
```

- Modify the `index()` method to use the Film class.

```php
function index()
{
    $films = Film::all();
    return view('films.index',['films' => $films]);
}
```

- Hopefully, you understand how the Active Record pattern is working. We want a list of all the films, so we call the `all()` method. This method has been defined automatically for us by the Eloquent ORM (https://laravel.com/docs/eloquent).
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

- To echo out the value of a variable we use the double curly brackets `{{...}}`.
- Note the use of `@foreach` and `@if`. These are part of the blade templating engine, and make it easy for us to include control structures in our views.
- Test this works http://localhost/films. You should see a list of films.

## Tying everything together - Getting Create to Work

If you look in _create.blade.php_ you should see that the form will be submitted to a URI of _*/films*_ using the _POST_ method.

```html
<form method="POST" action="/films">
  @csrf
  <div>...</div>
</form>
```

The `@crsf` directive is needed everytime we create a form. It is a security measure to protects us from cross-site request forgery. Read about it here - https://laravel.com/docs/11.x/csrf#main-content.

### Adding a new route

Back in _routes/web.php_ add another route for storing the new film.

```php
Route::post('/films', [FilmController::class, 'store']);
```

We can use the same route (_/films_) because we are using a different method (POST). Laravel can distinguish between the two.

### Adding a `store()` Method to the Controller

- Open _FilmController.php_.
- Add the following `store()` method.

```php
function store(Request $request)
{
    $film = new Film();
    $film->title = $request->title;
    $film->year = $request->year;
    $film->duration = $request->duration;
    $film->save();
    return redirect('/films');
}
```

The Laravel _request_ object allows us to access the form data (previously we would have used the `$_POST` array e.g. `$_POST['title']`). We use these values from _request_ to set properties on a film object, we then save it, and finally redirect to the home page.

## Tying everything together - Implementing Show

Add the following route to the _routes/web.php_ file

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

- The _id_ value which is part of the route is passed as a parameter to the `show()` method.
- We can then use Eloquent to get a single film object using this _id_ value (`Film::find($id)`).
- Finally we pass this film object to a _show_ view.

- Add the following _show.blade.php_ view to the _resources/views/films_ folder

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
        <input type="hidden" name="id" value="{{$film->id}}">
        <button type='submit'>Delete</button>
    </form>
</x-layout>
```

- Check this works, you should see the details displayed for the chosen film.

## Tying everything together - Implementing Edit

- The _edit_ action is triggered from the _show_ view. Have a look at the _show_ view to make sure you understand how the edit URI is generated for the selected film.
- Set up a route to respond to this URI.

```php
Route::get('/films/{id}/edit', [FilmController::class, 'edit']);
```

- Next, add an `edit()` method to `FilmController`.

```php
    function edit($id)
    {
        $film = Film::find($id);
        return view('films.edit', ['film' => $film]);
    }
```

- Next, create an edit view.

```php
<x-layout title="Edit a film">
    <h1>Edit the details for {{$film->title}}</h1>
    <form action="/films" method="POST">
    @csrf
    @method('PATCH')
    <!--A hidden field contains the id number of the film -->
    <input type="hidden" name="id" value="{{$film->id}}">
    <div>
        <label for="title">Title:</label>
        <!-- The text boxes are populated with values from the database ready for the user to edit -->
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

- Save this in the _resources/views/films_ folder as _edit.blade.php_.
- Note that we specify a method of PATCH

```php
@method('PATCH')
```

> Previously we have used GET and POST http methods to pass data. There are many more http methods e.g. PUT, PATCH, DELETE. However, browsers only support GET and POST. This line of code is used to tell Laravel we are wanting to perform a PATCH request (equivalent to update). See https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH for more info

- The advantage of using `PATCH` is we can have cleaner URIs. We can use the same URI i.e. _localhost/films_, but depending on the method (GET,POST,PATCH) we can define different routes and call different controller methods.

## Testing your Understanding

### Implementing Update

The process is exactly the same as we did in previous weeks. The difference is you need to use Laravel to implement the update.

- Add a route to handle the PATCH _/films_ request. The route should call an `update()` method in `FilmController`.
- Add an `update()` method in `FilmController`. This should:
  - Use the `request` object to get hold of the _id_ value from the hidden field in the form. See how we used the `request` object in the `store()` method or consult the documentation (https://laravel.com/docs/11.x/requests#input) for advice.
  - Use Eloquent to _find_ the correct film using the _id_ value. See how we used the static `find()` method when showing film details for a similar example or consult the documentation (https://laravel.com/docs/11.x/eloquent#retrieving-single-models) for advice.
  - Update the properties of this object using the values from the form and then call `save()` on the film object. Again, the documentation has examples (https://laravel.com/docs/11.x/eloquent#updates).
  - Redirect the user to the homepage.

### Implementing Delete

Have a look in _show.blade.php_ and inspect this page in a browser to understand the output that has been generated. The delete button submits the form to a URI of _/films_ using the _delete_ method.

- Add a route to handle the DELETE _/films_ request. The route should call a `destroy()` method in `FilmController`.

- Add a `destroy()` method in `FilmController`. This should:
  - Use the `request` object to get hold of the _id_ value from the hidden field in the form.
  - Use Eloquent to _find_ the correct film using the _id_ value.
  - Call `delete()` on the film object. Again, the documentation has examples (https://laravel.com/docs/11.x/eloquent#deleting-models).
  - Redirect the user to the homepage.
