---
layout: post
title: "Login and registration app using pure PHP - Part 2: Handling user data"
---

In the previous tutorial, we created a registration form to retrieve data from user input and log it on the browser. That's all well and good but we need to actually save that data so we can move on to the login part.

Before continiung, again, I would like to mention that we are using PHP's built-in server. This is very useful if want quickly to spin up a webserver and get to writing PHP and test your code on the fly. However, in a production environment, this would not be an ideal setup. We'll cover more of this in another series. But for now, let get our heads into development and start building our own database storage from scratch.

In this tutorial, we will be using JSON files as our main storage for user data. We will create validations for our user input to add a bit of security in our forms.

### Contents

- <a href="#set-up-user-data-handler">Set up user data handler</a>
- <a href="#validating-user-input">Validating user input</a>
- <a href="#using-session-variables-for-displaying-errors">Using session variables for displaying errors</a>
- <a href="#utilize-user-data-handler-on-registration-process">Utilize user data handler on registration process</a>

### Set up user data handler

Let's create a new folder in our project directory called `lib`. Inside it, create a file called `user_data_handler.php`

<br>

**`lib/user_data_handler.php`**:

```php
<?php

define('USERS_DATA_PATH', __DIR__ . '/../data/users.json');

function save_json_file($file_path, $data) {
	file_put_contents($file_path, json_encode($data, JSON_PRETTY_PRINT));
}

function get_json_data($file_path) {
	$json_data = file_get_contents($file_path);
	return json_decode($json_data, TRUE);
}

function get_users_data() {
	return get_json_data(USERS_DATA_PATH);
}

function save_user_data($user) {
	$users_data = get_users_data();
	$new_user = [
		'id' => count($users_data) + 1,
		'username' => $user['username'],
		'password' => md5($user['password']),
		'created_at' => date('Y-m-d H:i:s')
	];

	$users_data[] = $new_user;

	save_json_file(USERS_DATA_PATH, $users_data);

	return $new_user;
}

function get_user_by_id($id) {
	$users_data = get_users_data();
	$user = null;

	foreach ($users_data as $user) {
		if ($user['id'] == $id) {
			break;
		}
	}

	return $user;
}

?>
```

The code above contains functions for handling user data.

**1) save_json_file** - wraps the functionality of PHP's <a href="https://www.w3schools.com/php/func_filesystem_file_put_contents.asp" target="_blank">`file_put_contents`</a> to exclusively write JSON data based on file path and data provided as arguments.

**2) get_json_data** - wraps the functionality of PHP's <a href="https://www.w3schools.com/php/func_filesystem_file_get_contents.asp" target="_blank">`file_get_contents`</a> to exclusively get JSON data from `users.json` file located at `<project-root-directory>/data/users.json`.

**3) get_users_data** - calls the `get_json_data()` function to get the contents of `users.json` file.

**4) save_user_data** 

- retrieves the contents of `users.json` file using `get_users_data()`
- constructs a user array with the necessary key/value pair:
  - id (auto incremented)
  - username
  - encrypted user password
  - adds datetime of when user was created *(you might need to <a href="https://www.w3schools.com/php/func_date_default_timezone_set.asp" target="_blank">set timezone</a> on your server if needed)*
- uses `save_json_file()` to save new user to `users.json`
- returns the created user array

**5) get_user_by_id** - searches for user based on its id from `get_users_data()`

<br>

Also, if you notice on the top of the file we have `define('USERS_DATA_PATH', __DIR__ . '/../data/users.json');`. We defined the path to the `users.json` file as a constant because there's no need to change it in the code. If we have a new path, we just replace the constant with a new one instead of storing it in a variable.

### Validating user input

Let's go back to our `process_registration.php` file and let`s add the validations to the following fields:

- username
- password

<br>

**1) Username validation**

- Username should not be blank
- Username should have a minimum length of 4 characters

Let's convert these conditions into code:

```php
$errors = [];

if ($username === '') {
	$errors[] = 'Username should not be blank';
} else if (strlen($username) < 4) {
	$errors[] = 'Username should have a minimum length of 4 characters';
}
```

<br>

**2) Password validation**

For the password, we're just doing the same as the username and keep things simple. You can definitely add a <a href="https://stackoverflow.com/questions/1614811/how-do-i-measure-the-strength-of-a-password" target="_blank">password strength calculator</a> if you want.

- Password should not be blank
- Password should have a minimum length of 4 characters
- Password confirmation should match entered password

```php
// Password validation
if ($password === '') {
	$errors[] = 'Password should not be blank';
} else if (strlen($password) < 4) {
	$errors[] = 'Password should have a minimum length of 4 characters';
// Password confirmation validation
} else if ($password_confirmation !== $password) { 
	$errors[] = 'Passwords do not match';
}
```

<br>

Ok. These are pretty simple validation rules. Now let's take a look at our code:

```php
<?php
// Check if a POST request was made
if (!isset($_POST) || count($_POST) === 0) {
    echo 'Page not found';
    http_response_code(404);
    exit;
}

$username = trim($_POST['username']);
$password = $_POST['password'];
$password_confirmation = $_POST['confirm_password'];

$errors = [];

// Username validation
if ($username === '') {
	$errors[] = 'Username should not be blank';
} else if (strlen($username) < 4) {
	$errors[] = 'Username should have a minimum length of 4 characters';
}

// Password validation
if ($password === '') {
	$errors[] = 'Password should not be blank';
} else if (strlen($password) < 4) {
	$errors[] = 'Password should have a minimum length of 4 characters';
}

// Password confirmation validation
if ($password_confirmation !== $password) {
	$errors[] = 'Passwords do not match';
}

if (count($errors) > 0) {
	echo implode($errors, '<br>');

	// Create error session variable
	// Redirect to register.php
} else {
	// Register user here
}
?>
```

If we try to submit, our form with blanks, we get:

```
Username should not be blank
Password should not be blank
```

### Using session variables for displaying errors

That's all good, now we need to actually report these errors in our html file. For this, we're going to use the `$_SESSION` variable. Session variables gives us the power to store data in memory and can be used at a later time. We can also destroy the session after reading it if we are not going to need it in the next request-response cycle. To learn more about sessions, I suggest you read the PHP docs:

<a href="https://www.php.net/manual/en/reserved.variables.session.php" target="_blank">https://www.php.net/manual/en/reserved.variables.session.php</a>

At the top of `process_registration.php` file, add this:

```php
session_start();
```

This function gives us read/write access to the `$_SESSION` variable. The session variable is an associative array in which we can store and retrieve values from.

Now let's store the errors on the session variable if the `$errors` array has a length `< 0`.

```php
if (count($errors) > 0) {
	$_SESSION['errors'] = $errors;
	header('Location: /register.php');
	exit;
}
```

<br>

**Get errors from `register.php` page (frontend)**

We want our frontend to be able to make sense of what happened to our form submission. We need to display the errors we got from the validation logic so our users will know what to do to correct their input.

Let's create an error section in our `register.php` file inside the root `<div class="container">`

```php
<div class="container">
	<h1>Register here</h1>

	<?php if (isset($_SESSION['errors'])): ?>

	<div class="errors">

	<?php foreach ($_SESSION['errors'] as $error): ?>
	
		<p><?php echo $error; ?></p>

	<?php endforeach; ?>

	</div>

	<?php endif; ?>
</div>
```

We also need to add a `session_start()` at the top of our `register.php` file and as well as a `session_destroy()` at the bottom. This tells PHP that we only want to access the session variable after redirecting from `process_registration.php` and delete it afterwards since we don't want the display the errors all night long :D 

If we try to submit our form with blanks now, we would get nice validation errors:

![registration-form-4.png](/assets/posts/2020-6-20-login-registration-app-using-pure-php-part-1/registration-form-4.png)


### Utilize user data handler on registration process

Now we're done with validation, it's time to save some user data!

First create a folder named `data` inside your project directory and create file called `users.json` inside it.

The `users.json` file should contain an empty array:

```json
[]
```

Then let's include the `user_data_handler.php` into our `process_registration.php` file:

```php
include_once(__DIR__ . '/lib/user_data_handler.php');
```

And add the logic for saving a user if there are no errors in validation:

```php
if (count($errors) > 0) {
	$_SESSION['errors'] = $errors;
	header('Location: /register.php');
	exit;
}

// Save user data
save_user_data($_POST);

// Send success to frontend
$_SESSION['success'] = true;

header('Location: /register.php');
exit;
```

Looking at the code above, we have added 3 steps:

1. Save a new user from `$_POST` variable using `save_user_data()` which returns the associative array for the created user.
2. Set a boolean (true) to `$_SESSION['success']` variable.
3. Redirect to `/register.php` and exit the code.

We have 1 final step in the frontend to notify the user if the registration was successful. Let's add a message just below the errors div in `register.php`

```php
<?php if (isset($_SESSION['success']) && $_SESSION['success'] === true): ?>

<p>Registration is successful</p>

<?php endif; ?>
```

Now if you try to submit the form with valid input. You'll a get a success message and if you check the `users.json` file. It should now have 1 entry.

Congratulations! You're done with the registration part of the project. You've successfully created a registration form using pure PHP with no database or webserver set up.

Next comes the final piece of the puzzle. See you there!

<br>

**<a href="javascript:void(0);">Part 3 - Login</a>**

<br>