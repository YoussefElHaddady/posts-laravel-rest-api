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
    run command

    ```bash
    php artisan make:model Post -m
    ```

    two files will be created:

    1.  the model `app/Models/Post.php`
    1.  them migration `database/migrations/*_create_posts_table.php`

1.  change migration file to:

    ```php
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
    +        $table->string('title');
    +        $table->string('content');
            $table->timestamps();
        });
    }
    ```

1.  run the migration to create `posts` table in our database
    run command

    ```bash
    php artisan migrate
    ```

## Create API's Routes
