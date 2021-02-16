# Posts Laravel REST API

## Set up project & database

1.  create project

    ```bash
    laravel new posts-laravel-rest-api
    ```

1.  change databases setting in `.env` file
1.  create the database
1.  run command
    ```bash
    php artisan migrate
    ```
1.  create the model named `Post` and the migration file

    ```bash
    php artisan make:model Post -m
    ```

    two files will be created:

    1.  the model `app/Models/Post.php`
    1.  them migration `database/migrations/*_create_posts_table.php`

1.  change the model created to:

    ```php
    class Post extends Model
    {
        use HasFactory;

        protected $fillable = [
            'title', 'content'
        ];
    }
    ```

1.  change migration file to:

    ```diff
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
    ```

-              $table->string('title');
-              $table->string('content');
              $table->timestamps();
          });
    }
    ```

    ```

1.  run the migration to create `posts` table in our database

    ```bash
    php artisan migrate
    ```

1.  insert some data with `tinker`

    ```bash
    php artisan tinker
    >>> $post = new Post;
    >>> $post->title = 'My new post'
    >>> $post->content = 'My post content'
    >>> $post->save();
    ```

## Create Controller

1.  generate new controller
    ```bash
    php artisan make:controller PostsApiController
    ```
1.  change this controller

    ```php
    <?php

    namespace App\Http\Controllers;

    use App\Models\Post;
    use Illuminate\Http\Request;

    class PostsApiController extends Controller
    {
        public function index()
        {
            return Post::all();
        }

        public function show(Post $post)
        {
            return Post::find($post);
        }

        public function store()
        {
            request()->validate([
                'title' => 'required',
                'content' => 'required',
            ]);

            return Post::create([
                'title' => request('title'),
                'content' => request('content'),
            ]);
        }

        public function update(Post $post)
        {
            request()->validate([
                'title' => 'required',
                'content' => 'required',
            ]);

            $success = $post->update([
                'title' => request('title'),
                'content' => request('content'),
            ]);

            return [
                'success' => $success
            ];
        }

        public function destroy(Post $post)
        {
            $success = $post->delete();

            return [
                'success' => $success
            ];
        }
    }
    ```

## Create API's Routes

in your `routes\api.php` file add this items

1.  import class `use App\Models\Post;`

1.  add route to your `routes/api.php` file
    ```php
    Route::get('/posts', [PostsApiController::class, 'index']);
    ```
1.  add route to fetch one element

    ```php
    Route::get('/posts/{post}', [PostsApiController::class, 'show']);
    ```

1.  add creation route

    ```php
    Route::post('/posts', [PostsApiController::class, 'store']);
    ```

1.  add update route

    ```php
    Route::put('/posts/{post}', [PostsApiController::class, 'update']);
    ```

1.  add delete route
    ```php
    Route::delete('/posts/{post}', [PostsApiController::class, 'destroy']);
    ```
