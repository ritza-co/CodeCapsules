# Developing a Persistent Sleep Tracker Part 1: Creating a Login Management System with Flask Login

## Introduction

In this two-part tutorial series, we'll learn how to create a sleep tracker web-application hosted on Code Capsules. Users will register an account with the sleep tracker and log in. To track their sleep data, users will enter a date and amount of hours slept. We'll present users with a graph showing the sleep data they've logged, so users can get a better picture of their sleep habits. 

Throughout this tutorial series, we'll use many tools to create an interactive experience. We'll learn how to: create a user login and register system with Python's [Flask](https://flask.palletsprojects.com/en/1.1.x/), use a [MongoDB](https://www.mongodb.com/what-is-mongodb) to store user data, and create interactive [Plotly](https://plotly.com/python/) graphs showing their sleep data. 

This tutorial series is best suited for those with some Python, HTML, and Flask experience. If you feel you don't have too much experience with these - don't worry. We'll walk through this application step-by-step. Let's get started! 

## MongoDB Atlas

One of the most important aspects of this tutorial is using a Mongo Database (MongoDB). With this MongoDB, we can track users login information and their sleep data. MongoDB's are _NoSQL_ databases that allow for easy data tracking, without having to adhere to a specific database structure. If you're unfamiliar with NoSQL databases or MongoDB in general, make sure to [read this article](https://www.mongodb.com/nosql-explained) by the Mongo DB organisation. 

MongoDB Atlas clusters are free to use and quick to make. To create a MongoDB Atlas cluster that'll host all of the user data, **follow** [this short tutorial](link-to-how-to-setup-mongo-atlas.io). This step is **extremely** important. 

Once you've set up a MongoDB Atlas cluster, continue.

## Requirements

In addition to creating a MongoDB Atlas Cluster following [this tutorial](link-to-tut-here.io), make sure to have:

- [Git](https://git-scm.com/) installed and a registered [GitHub](www.github.com) account.
- [Virtualenv](https://pypi.org/project/virtualenv/) installed  
- A registered [Code Capsules](www.codecapsules.io) account.

## Project Set Up and Introduction

Creating this sleep tracker will be a two-part process:

- A login and register page
  ![login](images/login.png)

- A page where users enter their sleep data and view a graph. 
  ![sleep](images/sleep.png)


This tutorial will focus on the first part - creating a page where users can log in or register with our sleep tracker. 

To start, create a `sleep-tracker` directory somewhere on your computer. All of our project's files will be in this directory. After, continue on to set up a virtual environment.

### Setting up Virtual Env

With our `sleep-tracker` directory created, we need to set up a [virtual environment](https://realpython.com/python-virtual-environments-a-primer/). Setting up a virtual environment will be useful when we host our web application on Code Capsules. Virtual environments ensure that only the libraries used in the development of our sleep tracker application will be installed by Code Capsule's servers. 

To create a virtual environment, navigate to the `sleep-tracker` directory in a terminal and enter `virtualenv env`.

Then, activate the virtual environment with:

**Linux/MacOSX:** `source env/bin/activate`

**Windows:** `\env\Scripts\activate.bat`

If the virtual environment activated correctly, you'll notice `(env)` to the left of your name in the terminal. Keep this terminal open – we'll install the project dependencies next.

### Installing Requirements

Our sleep tracker will use the following Python libraries:

- [Flask](https://flask.palletsprojects.com/en/1.1.x/) is a lightweight Python web development framework.

- [Flask Login](https://flask-login.readthedocs.io/en/latest/) is a user session management tool for Flask. With Flask-login, we can quickly implement a login and register system without having to create one from scratch. 

- [Gunicorn](https://gunicorn.org/) is the [WSGI server](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface) we'll use to host our application on Code Capsules.

- [Pymongo](https://pymongo.readthedocs.io/en/stable/) is a Python library that has tools for interacting with MongoDB's. We'll use Pymongo to connect and send data to our MongoDB hosted on MongoDB Atlas.

To install these libraries, **make sure** to activate the virtual environment in your terminal. Then, type `pip3 install flask flask-login gunicorn pymongo`. 

With these libraries installed, we need to do one more thing before building our project. We'll create all the files and directories that we'll use throughout this and the [next tutorial in this series](linktonexttutorialhere.io). 

### Creating the File Structure

Because we'll use Flask to render our HTML files and serve static content, we need to have a specific project structure. Flask will only render HTML files found in directories named `templates`. Similarly, Flask only serves static content such as CSS stylesheets and images in directories named `static`. In the `sleep-tracker` directory we created, create a directory named `templates` and a directory named `static`.

Our `templates` directory will contain all of the HTML code for our sleep tracker web application. Open the `templates` directory and create three files: `base.html`, `login.html`, and `main.html`. 

- `login.html` will contain the HTML code for the main page of our web-application. Here, users will log in or register an account with our sleep tracker.

- `main.html` will contain the HTML code for the page that will allow users to enter in their sleep data and view their sleep data graph.

- The `base.html` file is a little more complicated - we'll talk about that file in the [next section](#creating-the-base.html-file). 

Next, open the `static` directory, and create a file named `style.css`. This is the only file we'll need in our `static` directory. We'll use this `style.css` file to style all of our HTML code.

After creating the `templates` and `static` directories and the `base.html`, `login.html`, `main.html`, and `style.css` files, we need to create one last file. In the `sleep-tracker` directory, create a file named `app.py`. This file will contain all of the Python code for our application. Here, we'll use Flask to render our HTML files, serve our static content, and manage when users log in or register with our sleep tracker. 

With these files created, let's take a closer look at the `base.html` file we created in the `templates` directory.

## Creating the Base.html File

Flask uses the [Jinja](https://jinja.palletsprojects.com/en/2.11.x/templates/) templating library to allow us to embed Python code in HTML. This Python code will execute on the Code Capsules web server before any content (like an HTML file) serves to the user. Jinja makes developer's lives easier when creating full-stack applications like our sleep tracker. The `base.html` file will show us how much easier Jinja and Flask can make full-stack development.

The `base.html` file will contain all of the HTML code common throughout our application. This will act as a sort of "skeleton" - our `login.html` and `main.html` files will _inherit_ the code in our `base.html` file. This way, we won't have to enter identical code in two files. How this works will become clear as we implement our sleep tracker. 

Open the `base.html` file and enter the following markup:

```html
<!DOCTYPE html>

<html>
  <head>
    <meta charset="utf-8">
    <title> Sleep Tracker </title>
    <meta name="author" content="your-name-here">
    <meta name="description" content="This web-application
    helps you track your sleep!">
    <link rel="stylesheet" href="{{url_for('static',filename='style.css')}}">
  </head>

<body>
{% block content %}{% endblock %}

</body>
</html>

```

This is our "skeleton". Both our `login.html` and  `main.html` files will inherit this code. 

Take a look at the 

```html
<link rel="stylesheet" href="{{url_for('static',filename='style.css')}}">
```
line. The code enclosed in `{{}}` is Jinja syntax. Flask reads any code enclosed in `{{}}` and evaluates it before [rendering](https://flask.palletsprojects.com/en/1.1.x/tutorial/templates/) HTML files and serving them to users. 

In this example, we use Jinja syntax and call the Flask `url_for()` function. To link our stylesheet, Flask uses the `url_for()` function to find the `style.css` file in the `static` directory. 

Take another look at the code enclosed in the `<body>...</body>` tags. The `{% block content %}{% endblock %}` designates the location of the markup unique to the `login.html` and `main.html`. Let's create implement our `login.html` file to see how this works. 

## Creating the Login Page

Now that we've created the `base.html` file, we can implement our `login.html` file. Navigate to the `templates` directory and open the `login.html` file. Add the following markup:

```html
{% extends "base.html" %}

{% block content %}
<h2>Login or register here to track your sleep!</h2>
<form action="" method="POST">
  <ul>
    <li> 
      <label for="username">Username:</label>
    <input type="text" id="name" name="user_name">
    </li>
    <li>
      <label for="password">Password:</label>
    <input type="password" id="password" name="user_pw">
    </li>
    <li class = "button">
      <input type="submit" name='login' value='Login'>
    <input type="submit" name='register' value='Register'>
    </li>
  </ul>
</form>
{% endblock %}

```

That's it - with the line `{% extends "base.html" %}`, Flask will know to "copy and paste" all of the code in the `base.html` file into our `login.html` file.

The code in between the `{% block content %}` and the `{% endblock %}` lines will replace the `{% block content %}{% endblock %}` lines found in our `base.html` file. In the HTML above, we create an area where users can enter their username and password, and a login and register button. Note the `POST` HTTP method. We'll use this `POST` method in our Python code to know when a user has clicked one of our buttons.

Before we take a look at our work so far, let's implement our `style.css` file to give our HTML files some structure.

### Adding Styles

Open the `style.css` file in the `static` directory and add the following:

```css
form {
  margin: auto;
  width: 500px;
}

form li + li {
  margin-top: 1em;
}

ul {
  /* Remove unordered list dots */
  list-style: none;
  padding: .1;
  margin: .1;
}

label {
  display: inline-block;
  width: 100px;
  text-align: right;
}

input,
textarea {
  font: "Times New Roman", Times, serif;
  /* Change border & width of textarea */
  border: 1px solid #000000;
  width: 300px;
  box-sizing: border-box;
}


.button {
  /* Align w/ text box */
  padding-left: 105px; 
}

h2 {
  text-align: center;
  font: "Times New Roman", Times, serif;
}

```

Our sleep tracker's styling is kept deliberately simple - feel free to alter it. Now that we've implemented the `style.css` file, let's test out our `login.html` file by rendering it with Flask. 

### Testing What We Have

Let's add some initial functionality to our `app.py` file. With our `base.html`, `login.html` and `style.css` files created, we can render our `login.html` and `main.html` files with Flask. Open the `app.py` file and enter the following code:

```python
from flask import Flask, render_template, url_for

app = Flask(__name__)

## Login/Register page
@app.route('/')
def login():
	return render_template('login.html')

@app.route('/main')
def main():
	return render_template('main.html')

```

Here we create the root page (signified by the `'/'`) for our application) with the 

```python
@app.route("/")
```

line. Our root page contains the rendered `login.html` file. The  `login.html` file renders using Flasks `render_template()` function. This `render_template()` function knows to read all of the Jinja code in our `base.html` and `index.html` files. 

The lines below

```python
@app.route('/main')
```

work the same way. In the `/main` route, we'll be able to view our `main.html` file (which isn't implemented yet).

View the application by opening a terminal in the `sleep-tracker` directory and running the `app.py` file. Run this file by entering `flask run`. 

After running the application, Flask will provide you with an IP address that you should enter in your web browser. After entering the IP address, you'll see the login screen. Right now our buttons don't do anything - let's implement those now.

## Creating the Log-in Management System

Before we can implement the login and register buttons in our `login.html` file, we need to create a working login system. We'll use the Flask-Login library that we installed earlier. 

Flask-Login provides tools that'll help us easily implement a login-management system. First, let's import `flask-login` and all of the functions we'll use for our login-management system. Open the `app.py` file again add the following at the top of the file:

```python
from flask_login import LoginManager, UserMixin, login_required, login_user, logout_user, current_user
```

Now we can instantiate our `LoginManager` class and set a secret key for our application (read more about why Flask-Login requires a secret key [here](https://flask-login.readthedocs.io/en/latest/)). Add:

```python
app.config['SECRET_KEY'] = 'your-secret-key-here' 
login_manager = LoginManager(app)
```

We've created our `LoginManager`. The `LoginManager` does exactly what is says - manages logged in users and communicates any necessary information about a user to Flask. Make sure to replace `your-secret-key-here` with any text of your choice.

Next, we need to hook up the database we created [previously](#mongodb-atlas). When a user registers with our sleep tracker, we'll create a new entry in our MongoDB with the user's username and password. That way, when a user logs in to our sleep tracker, we can see if the information they entered matches the information in our MongoDB.

Beneath the last line we added, add the following:

```python
import pymongo

client = pymongo.MongoClient('mongodb+srv://YOURUSERNAME:<password>@cluster0.e2fw3.mongodb.net/<dbname>?retryWrites=true&w=majorhostity')
db = client.user_login
```

Here we import the `pymongo` library. With the `pymongo` library we can connect to our MongoDB hosted on MongoDB Atlas. Make sure to replace `YOURUSERNAME` and `<password>` with your MongoDB Atlas user account information created in [this tutorial](link-to-mongo-db-tut.io).

Now we can creat the login system.

### Create the User Class 

For Flask-Login to work, we need to create a `User` class. This `User` class will contain information pertaining to the user that is currently logged in to our sleep tracker.

Flask-Login expects us to implement four generic functions for our `User` class: `is_authenticated`, `is_active`, `is_anonymous` and `get_id`. This is where the `UserMixin` class that we imported earlier will come in handy. `UserMixin` is a Flask-Login class that provides generic implementations for these functions. We'll also need to implement a `check_password` function and a `load_user` function.

Below the line:

```python
db = client.user_login
```

Enter the following:

```python
## Define flask-login User class ## 
class User(UserMixin):
    def __init__(self, username):
        self.username = username

    def get_id(self):
        return self.username

    @staticmethod
    def check_password(password_entered, password):
        if password_entered == password:
            return True
        return False

    @login_manager.user_loader
    def load_user(username):
        user = db.users.find_one({"username": username})
        if user is None:
            return None
        return User(username=user['username'])
```


Because we extended `User` with the `UserMixin` class, we didn't need to implement the `is_authenticated`, `is_active` or `is_anonymous` functions. For the `get_id` function, we simply return the users username.

The `check_password` function will come in handy when a user clicks the login button. We'll use this function to verify that the password a user entered matches the corresponding entry in the MongoDB.

With the line `@login_manager.user_loader`, our `login_manager` will know to make use of the `load_user` function. When a user logs in, the login_manager will store a `User` object containing the `username` that the user entered.

That's all we need for our login-management system. Now we can implement our login and register buttons. 

### Add Functionality to the Login and Register Buttons

When a user clicks the Register button, we want to send their username and password to our MongoDB. When a user clicks login, we want our sleep tracker to make sure their login information corresponds with a username and password in our MongoDB. If the information they've entered is accurate, we want to redirect them to the `main.html` file. 

To do this, we'll create a new function that gives our login and register buttons functionality. Add the following code below the `def login()` function:

```python
@app.route("/", methods = ["POST"])
def login_or_register():
    if request.method == 'POST':
        name_entered = request.form.get('user_name') # Get username and password from form
        pw_entered = request.form.get('user_pw')

        if request.form.get('login'): # Log in logic
            user = db.users.find_one({'username':name_entered})
            if user and User.check_password(user['password'],pw_entered):
                usr_obj = User(username=user['username'])
                login_user(usr_obj)
                return redirect(url_for('main'))
            else:
                return redirect(url_for('login'))

        elif request.form.get('register'): # Register logic
            new_user = {'username':name_entered,'password':pw_entered}
            db.users.insert_one(new_user) # insert new user to db
            return redirect(url_for('login')) # redirect after register
```

First, note the `POST` method. Recall that when we created our `login.html` file, we added a `POST` method in the HTML. By adding the `POST` method in our Flask application, Flask will know to look for any `POST` requests when either the login or register buttons are clicked. If a `POST` request is found, our `login_or_register` function will run.

The first portion to look at is the "login" logic. If the login button is pressed (`if request.form.get('login')`), we check our MongoDB for a username that matches the one entered. Then we check if the password that the user entered is the same password as the user in the MongoDB. If `check_password` evaluates to true, we log the user in and redirect to a `main` route (which we'll create in the [next part of this series](linktonexttutoorial.io). 

To redirect users, we use Flasks `redirect` function and the `url_for` function. The `url_for` function finds the `main` route, and the `redirect` function sends users to that route. 

If a user clicks register, we create a `new_user` and insert it into our MongoDB, then redirect the user back to the main `login.html` page.  

### Trying Out the Login System

We've implemented the login and register buttons. Try running the program by opening a terminal in the `sleep-tracker` directory and entering `flask run`. Here you can test out registering an account, and logging in. Remember, we haven't implemented any HTML code in our `main.html` file - this means, when you log in, **you'll see an error**. Don't worry, everything is working! 

Since users can now log in and register, in the [next tutorial](link-to-next-tutorial) we'll implement the rest of the sleep tracker web-application. This means implementing the `main.html` file, and learning how we can store user sleep data in our MongoDB. 

## Further Reading

We covered a lot in this tutorial. If you're confused about Flask-Login or Jinja, check out the following links:

- If anything confused you about the Flask-Login package, take a look at [their documentatio](https://flask-login.readthedocs.io/en/latest/).

- For more information on the Jinja templating language, [try here](https://flask-login.readthedocs.io/en/latest/)

Otherwise, when you're ready, finish the sleep tracker application by [following the second tutorial in this series](link-to-second-tut-here.io)
