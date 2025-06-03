# Task manager
## The project
The project is a task management system. A dashboard was developed that allows the creation of multiple lists, with tasks organized within each list. Users can edit, delete, and update tasks, as well as mark them as completed or incomplete.
## Stack
* Laravel 10
* Vue JS 3
* Inertia 1.3
* MySQL
* Node 22.5

## Running the project

First config your **.ENV** with the database connection. 

Run the following commands on the terminal: 

```bash
composer install
```

```bash
npm install
```

Run your migrations

```bash
php artisan migrate
```

In one terminal, run the following command to start the web server

```bash
php artisan serve
```

In another terminal, run the following command to start the frontend

```bash
npm run dev
```

Now your application should be running and accessible in your web browser.


## Done and not done
* A user can have multiple task lists, and each task list contains multiple tasks. These relationships are defined within the model files. 
* There is no pagination, but using Laravel's built-in pagination for models would make this straightforward; most of the work would be on the frontend.

* Real-time updates have not been implemented. I would have used WebSockets for that, possibly with Laravel Echo, Pusher, or broadcasting.
