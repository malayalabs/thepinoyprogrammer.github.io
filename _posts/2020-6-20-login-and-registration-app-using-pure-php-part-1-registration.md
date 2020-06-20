---
layout: post
title: "Login and registration app using pure PHP - Part 1: Registration"
---

Have you ever wondered how you can write a login and registration feature using pure PHP? That's right! No server, no database, just plain old PHP. 

In this tutorial, we'll be focusing on the coding part on the PHP side with a minimalist setup. We'll cover the webserver topic in another tutorial. My goal is to get you writing PHP code and not worrying on server or db setup.

> ***Disclaimer:** PHP's built-in web server was designed to aid application development. It may also be useful for testing purposes or for application demonstrations that are run in controlled environments. It is not intended to be a full-featured web server. It should not be used on a public network.* 
For more info visit:
<a href="https://www.php.net/manual/en/features.commandline.webserver.php" target="_blank">https://www.php.net/manual/en/features.commandline.webserver.php</a>

### Contents

- <a href="#requirements">Requirements</a>
- <a href="#project-setup">Project Setup</a>
- <a href="#creating-an-html-registration-form">Creating an HTML registration form</a>
- <a href="#styling-the-form-using-css">Styling the form using CSS</a>
- <a href="#creating-a-backend-for-handling-registration-form">Creating a backend for handling registration form</a>

### Requirements
- PHP >= 5.4

<br>

Make sure you have PHP cli installed. Open your terminal/cmd and run:

```bash
php -v
```

You should have at least PHP version 5.4 to be able to run the PHP built-in server. Output should look something like this:
```
PHP 7.4.6 (cli) (built: May 12 2020 08:09:15) ( NTS  )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
```

### Project Setup

Now that you have PHP installed. Let's create your project folder.

```bash
mkdir my-login-registration-project
cd my-login-registration-project
```

*You can replace `my-login-registration-project` with the name of your project folder.*

In our project folder let's create and index.php and write a simple `phpinfo()` function to test our server.

```bash
# In your project folder:
touch index.php
```

Edit `index.php` and write:

```php
<?php phpinfo(); ?>
```

Save the file and in your project folder run:

```bash
php -S localhost:3000
```

Now visit <a href="http://localhost:3000" target="_blank">http://localhost:3000</a> and you should see something similar to this:

![phpinfo.png](/assets/posts/2020-6-20-login-registration-app-using-pure-php-part-1/phpinfo.png)

Great, now you've managed to create a simple PHP app using PHP CLI's built-in server!

### Creating an HTML registration form

Let's write our first registration form to allow users to register to our app and save their info. To do that, we'll start by creating our HTML form (frontend).

Go ahead and create a `register.php` file in your project folder and write *(don't copy and paste but type with your fingers!)*:

```php
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Registration page</title>
	<link rel="stylesheet" href="style.css">
</head>
<body>
	<div class="container">
		<h1>Register here</h1>

		<form action="/register.php" method="post">
			<div class="form-item">
				<label>Username</label>
				<input type="text" name="username">		
			</div>
			<div class="form-item">
				<label>Password</label>
				<input type="password" name="password">		
			</div>
			<div class="form-item">
				<label>Confirm password</label>
				<input type="password" name="confirm_password">		
			</div>
			<div class="form-item">
				<button type="submit">Register</button>
			</div>
		</form>
	</div>
</body>
</html>
```

> *The reason I don't recommend copying/pasting (especially for novice/beginners) is to encourage you to actually **CODE**. Coding is not something we just do with the mouse or CTRL+C / CTRL+V. It is through practice of typing actual code do we get better at it. It's weird but someday you'll thank me for it, because your fingers along with your brain work together to create these amazing applications! As you progress, you'll also type your own stuff in here and learn by actually doing the work.*

Now let's take a look on what we've done so far. In your browser visit <a href="http://localhost:3000/register.php" target="_blank">http://localhost:3000/register.php</a>:

![registration-form-1.png](/assets/posts/2020-6-20-login-registration-app-using-pure-php-part-1/registration-form-1.png)

### Styling the form using CSS

Not a really good looking form right now, but we have a solution to that, CSS!

Create a `style.css` file in your project folder:

```css
body {
	padding-top: 60px;
}

.container {
	margin: 0 auto;
	padding: 20px 40px;
	width: 480px;
}

.form-item {
	margin-bottom: 20px;
}

.form-item input {
	width: 100%;
}

.errors {
	margin: 20px auto;
}

.errors p {
	color: #ce3c3c;
	font-weight: bold;
}

.hidden {
	display: none;
}
```

Now your form should look a lot better:

![registration-form-2.png](/assets/posts/2020-6-20-login-registration-app-using-pure-php-part-1/registration-form-2.png)

### Creating a backend for handling registration form

If we try to submit our form, nothing will happen. That's because we haven't created a backend for our form yet.

Now we have to create a file called `process_registration.php` which will handle the registration once we submit the form.

In your `register.php` file replace the `action` in your `<form>` tag from 'register.php' to 'process_registration.php'. Your form should look something like this:

```html
<form action="/process_registration.php" method="post">
    <div class="form-item">
        <label>Username</label>
        <input type="text" name="username">		
    </div>
    <div class="form-item">
        <label>Password</label>
        <input type="password" name="password">		
    </div>
    <div class="form-item">
        <label>Confirm password</label>
        <input type="password" name="confirm_password">		
    </div>
    <div class="form-item">
        <button type="submit">Register</button>
    </div>
</form>
```

Let's create that backend file...

**process_registration.php**

```php
<?php

// Check if a POST request was made
if (!isset($_POST) || count($_POST) === 0) {
    echo 'Page not found';
    http_response_code(404);
    exit;
}

// Log user input to the browser
var_dump($_POST);

?>
```

As you may have read from the code above, we're not doing anything to the user input just yet. We're just logging what they have entered in the registration form to your browser.

**Filling out the form:**

![registration-form-3.png](/assets/posts/2020-6-20-login-registration-app-using-pure-php-part-1/registration-form-3.png)

**After submitting:**

```php
array(3) { ["username"]=> string(9) "test-user" ["password"]=> string(4) "1234" ["confirm_password"]=> string(4) "1234" } 
```

In the next part of this series, we'll explore on how we can create user data from the registration form without setting up a database server. We'll also add some validation and error handling to make our form more secure.

<br>

**<a href="/login-and-registration-app-using-pure-php-part-2-handling-user-data">Part 2 - Handling user data</a>**

<br>
