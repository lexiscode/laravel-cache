# Laravel Event Listeners

In Laravel, events and listeners provide a way to decouple various components of your application. Events are used to notify other parts of your application when a certain action or occurrence takes place, while listeners are responsible for handling those events. Here's a basic overview of how events and listeners work in Laravel:

Let's create a model, migration, controller, route and view for Newsletter:

Therefore let's start by first creating a model, we will run: ```php artisan make:model Newsletter```. Inside our app/Model/Newsletter:

```ruby
<?php
...

class Newsletter extends Model
{
    use HasFactory;

    protected $fillable = ['email'];
}
```

Next we will create a migration file, run: ```php artisan make:migration create_newsletters_table```. Inside our app/database/migrations/our-file.php:

```
<?php
...

return new class extends Migration
{
    // Run the migrations.
    public function up(): void
    {
        Schema::create('newsletters', function (Blueprint $table) {
            $table->id();
            $table->string('email');
            $table->timestamps();
        });
    }

    // Reverse the migrations.
    public function down(): void
    {
        Schema::dropIfExists('newsletters');
    }
};
```
Now we can migration our Newsletter model into our database by running: ```php artisan migrate```. Now let's move on to creating our controller and route. 

To create the controller, we will run ```php artisan make:controller NewsletterController```.

```ruby
<?php
...

class NewsletterController extends Controller
{
    public function index()
    {
        return view('index.blade.php');
    }
}
```

Next, go to the routes/web.php:

```ruby
Route::get('/', [NewsController::class, 'index']);
```

Next, another thing we need to create is our index.blade.php file inside our views, navigate to resources/views/index.blade.php:

```ruby
<html>
<head>
    <title>Document</title>
    <link rel="stylesheet" href="{{ mix('css/app.css') }}">
</head>
<body>
    <h1> Newsletter </h1>
    <form action="/subscribe" method="post">
        @crsf
        <input type="text" name="email" placeholder="Enter your email address here...">
        <button type="submit">Subscribe</button>
    </form>
</body>
<html>
```

Now looking at the html document inside our view file, obviously we will need another route and an extra method inside our NewsletterController. Therefore, we will update our routes/web.php file:

```ruby
Route::get('/', [NewsController::class, 'index']);
Route::post('/subscribe', [NewsController::class, 'subscribe']);
```

Then this will be our updated NewsController code file:

```ruby
<?php
...

class NewsletterController extends Controller
{
    public function index()
    {
        return view('index.blade.php');
    }

    public function subscribe()
    {
        // brb, we will add something here soon
    }
}
```


