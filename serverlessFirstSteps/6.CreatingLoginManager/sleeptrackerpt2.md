# Developing a Persistent Sleep Tracker Part 2: Tracking and Graphing Sleep Data

This is the second part of our sleep tracker web application tutorial, covering the creation of the sleep tracker interface.

## Recap

In the [first part of this series](==LINKHERE==), we build a web application with user registration and login using Python's Flask web framework and a hosted NoSQL database on MongoDB Atlas for data persistence. We will now be building on the code we started on in that tutorial, so please complete it before starting this one.

In this second part of the series, we'll implement the logic that allows users to enter their sleep data and see that data on an interactive graph, generated using [Plotly](https://plotly.com/). Registered users will be able to log the number of hours slept on different days and visualise this data as a graph. 

## Creating the Sleep Tracker Front-end

At the end of our last tutorial, we saw a blank page when we logged in and were redirected to the application's `/main` page. This happened because our `main.html` file was empty, so let's add some content to it. We'll need the following: 

1. A form with fields for the date and hours slept, so users can provide sleep data for different days.
2. A way to view a graph of this data.
3. A logout button, that logs the user out. 

To do all of this, we will first need to build a `main.html` containing both static HTML and dynamic Jinja template components that will change depending on which user is logged in and what sleep data they've provided. We will also implement some front-end JavaScript code to make our sleep data graph interactive.

Let's add the sleep data logging form first. Open the `main.html` file in the `templates` directory. Add the following:

```html
{% extends "base.html" %}

{% block content %}
<h2>Hey, {{ user['username'] }}! Let's track your sleep.</h2>
<form action="" method="POST">
  <ul>
    <li> 
      <label for="time">Time Slept (hours):</label>
    <input type="text" id="time" name="time">
    </li>
    <li>
      <label for="date">Date:</label>
    <input type="date" id="date" name="date"
         value="">
    </li>
    <li class = "button">
      <input type="submit" name='submit' value='Submit'>
    <input type="submit" name='graph' value='View Graph'>
    <input type="submit" name='logout' value='Logout'>
    </li>
  </ul>
</form>
{% endblock %}
```

This works similarly to the `login.html` file we created in the previous tutorial, with `base.html` acting as the page skeleton and our unique content being entered between the `{% block content %}` and `{% endblock %}` lines. We also ensure that our form uses the `POST` method so we can differentiate between a user visiting the page and clicking on of the three form buttons. To determine which button a user has clicked in a given `POST` request, we'll be use the button's HTML `name` attribute (`submit`, `graph` or `logout`) in Flask.

Notice the Jinja snippet `{{ user['username'] }}`. This will display data sent from our Flask back-end code in the page – in this case, the user's name.

The `<input type="date" id="date" name="date" value="">` line creates an interactive calendar so users can click on dates rather than typing them out.

## Adding Sleep Data Submission and Logout

In the `app.py` file, add the following line below the `app = Flask(__name__)`  line.

```python
app.config['plotting'] = False
```

Here we add a new entry in our Flask application's configuration settings. This will come in handy soon – we'll use this line to tell whether or not a user has clicked the "View Graph" button in our `main.html` file. If a user has clicked the button, we'll set this line to "True" and a graph with the user's sleep data will display. 

Now, we can implement the functionality for our "Submit", "View Graph" and "Logout" buttons. Add the following code below the `main()` function.

```python
@app.route('/main', methods = ['POST'])
def submit_sleep():
    if request.form.get('submit'): # if submitting new sleep data
        time_entered = float(request.form.get('time'))
        date_entered = request.form.get('date')
        message = add_sleep(time_entered,date_entered,db.users.find_one({'username':current_user.get_id()}))
        if message:
          return message

    if request.form.get('logout'):
        logout_user()
        app.config['plotting'] = False
        return 'You logged out!'

    elif request.form.get('graph'):
        app.config['plotting'] = True

    return redirect(url_for('main'))
```

This function operates similarly as our `login_or_register` function:

If a user clicks "Submit", the data they entered will be stored in their MongoDB entry via the `add_sleep` function (that we'll create next). This function will return a string with an error message if it encounters an error, and `None` if it succeeds.

If a user clicks "Logout", the user will be logged out. 

If a user clicks "View Graph", the `app.config['plotting']` entry is set to `True`. Later, we'll expand `main.html` to display the graph.

Let's wrap up our button functionality by creating the `add_sleep` function that is called when a user clicks "Submit". Add the following code **above** the `submit_logout_plot` function to create the `add_sleep` function:

```python 
def add_sleep(time, date, user):
    if not re.match("[0-9]{4}-[0-1][0-9]-[0-3][0-9]", date):
        return "Invalid date supplied."

    if time < 0.0 or time > 24.0:
        return "Sleep time must be between 0 and 24 hours."

    if 'date' in user:
        user['date'].append(date)
        user['time'].append(time)
    else: # adding sleep data for the first time
        user['date'] = [date]
        user['time'] = [time]

    # Update MongoDB Atlas
    db.users.update_one({ 'username': user['username'] },
        { '$set': { 'date': user['date'],
          'time': user['time'] }})
```

Our `add_sleep` function takes three variables:

* `time`: The number of hours slept that the user entered.
* `date`: The calendar date the user selected.
* `user`: The user's MongoDB entry.

First, we validate the user's input to ensure that a correctly formatted date has been provided and that the time given is not a negative number or larger than 24. If either value does not pass validation, we return a relevant error message from the function without writing to the database. Otherwise we continue.

If a user has never entered any sleep data, we add a new `'date'` and `'time'` entry to the user's MongoDB entry with the date and time entered. Otherwise, we add the data they entered to their existing sleep data. Finally, we update the user's MongoDB entry with `db.users.update_one`. 

We've implemented functionality for all three buttons. Now we need to modify the `main()` function to pass the current user's data to `main.html`. This will allow us to display their username on the page, and to graph their sleep data. Find the `main()` function and modify it like so:

```python
@app.route('/main')
def main():
    if current_user.get_id() is None:
        return redirect(url_for('login')) # redirect to login page if not logged in

    user_data = db.users.find_one({ 'username': current_user.get_id() })
    return render_template("main.html", user=user_data, plot=app.config['plotting'])
```

First, we leverage Flask-Login's [anonymous users](https://flask-login.readthedocs.io/en/latest/#anonymous-users) functionality to check if the current user is not logged in, and if so, we redirect them to the login page. If the current user is logged it, we retrieve their MongoDB entry and assign it to `user_data`. Then we pass this variable to the `render_template` function as `user`. This is how the line below will access and display the user's name.

```html
<h2>Hey, {{ user['username'] }}! Let's track your sleep.</h2>
```

As `user_data` contains the entire MongoDB user entry, our template will be able to access the current user's sleep data from the `user` variable as well.

We've also passed the template the value of `app.config['plotting']` in `plot`. This is how our application will know whether or not to display a graph on the `/main` page. 

All that's left now is to add the sleep data graph in our `main.html` file. After that, we can deploy our application to Code Capsules.

## Adding the Plotly Graph

As mentioned at the beginning of the article, we'll add the ability to graph sleep data with the help of [Plotly](https://plotly.com/). Plotly provides an external JavaScript library that we can use to create interactive graphs for the web. 

In the `main.html` file, find the `</form>` line. Right below this line, add the following: 

```html
{% if plot %}
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<div id="graph">
    <script>
      var x = {{user['date'] | safe }};
      var y = {{user['time'] | safe }};

    var trace1 = {
    x: x,
    y: y,
    type: 'bar'
  };

  var data = [trace1];

  var layout = {

    title: {
      text:'My Sleep',
      font: {
        family: 'Courier New, monospace',
        size: 24
      },
      xref: 'paper',
      x: 0.05,
    },
    xaxis: {
      title: {
        text: 'Date',
        font: {
          family: 'Courier New, monospace',
          size: 18,
          color: '#7f7f7f'
        }
      },
    },
    yaxis: {
      title: {
        text: 'Time Slept (hrs)',
        font: {
          family: 'Courier New, monospace',
          size: 18,
          color: '#7f7f7f'
        }
      }
    }
  };
  Plotly.newPlot('graph', data, layout);
    </script>
</div>
{% endif %}
```

Let's break this down, starting with the line `{% if plot %}`. This line references the `plot` variable we created in our `app.py` file in the `main` function. If someone has clicked "View Graph", we set `plot`  to true. If `plot` is true, the HTML between `{% if plot %}` and `{% endif %}` will be included in the page served to the user, otherwise it will be left out.

The line `<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>` imports the Plotly graphing library. This is similar to an `import` statement in Python.

Under this line, we see a lot of code enclosed in `<script>...</script>` tags. This is all JavaScript code. In this code, we create two variables, `x` and `y`, which contain arrays of the dates and number of hours slept that a user has logged.

Note that we have set these variables as `safe`, which means that Jinja will not attempt to escape or encode any of the characters within them when it renders the HTML. This is dangerous to do with user input, which is why we validated both the date and time values in our Python code.  If we had not validated them, a malicious user might be able to supply JavaScript code in the date or time input fields and alter the behaviour of this page.

The code inside `var trace1 = {...}` tells Plotly which data to use for the x and y axes, and the type of graph we'll make – a bar graph.

All the code in `var layout = {...}` effects things like x and y axis labelling, font type, and size of font. Customise this yourself!

Finally, The line `Plotly.newPlot('graph', data, layout)` creates the actual graph and displays it to a user. 

Try running the application by opening a terminal in the `sleep-tracker` directory, activating the virtual environment and entering `flask run`. You should be able to register a new user account, log in, enter sleep data, and view your graph. 

## Preparing for Deployment

With our sleep tracker functionally complete, we need to make one last modification to our `app.py` file and add some files in the `sleep-tracker` directory before we can push our code to GitHub and deploy it on Code Capsules. 

### Creating environment variables

Before we push our code to GitHub, we need to remove our `app.config['SECRET_KEY']` and the MongoDB user credentials. If we were to push our code now, anyone could use our secret key to forge user sessions on our application or our MongoDB credentials to alter our database. Luckily, there is an easy fix. 

First, save your secret key and MongoDB credentials somewhere safe, outside of this project's directory so that you don't lose them. Then, at the top of `app.py`, add the line

```python
import os
```

Then, replace the line:

```python
app.config['SECRET_KEY'] = 'your-secret-key-here' 
```

with this:

```python
app.config['SECRET_KEY'] = os.getenv('SECRET_KEY') 
```

And replace this line:

```python
client = pymongo.MongoClient('mongodb+srv://YOURUSERNAME:<password>@cluster0.e2fw3.mongodb.net/<dbname>?retryWrites=true&w=majorhostity')
```

with this:

```python
client = pymongo.MongoClient('MONGO_CONNECTION_STRING')
```

`os.getenv('SECRET_KEY')` and `os.getenv('MONGO_CONNECTION_STRING')` will look for [environment variables](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa) with the names "SECRET_KEY" and "MONGO_CONNECTION_STRING". When we host the sleep tracker application on Code Capsules, we'll set these environment variables to the values we removed from the code. 

### Creating a Procfile and requirements.txt

Code Capsules requires a couple of files to deploy our application: `Procfile` and `requirements.txt`. The first one tells Code Capsules how to run our application, and the second one tells it which libraries it needs to install.

To create the `Procfile`:

1. Create a file named `Procfile` in your project directory (do **not** add a file extension).
2. Open the `Procfile`, enter `web: gunicorn app:app`, and save the file. This tells Code Capsules to use the Gunicorn WSGI server to run `app.py`.

In the same terminal, activate the virtual environment and enter `pip3 freeze > requirements.txt` to create `requirements.txt` and populate it with all the libraries we've used to create this application.

Now we can push our code to GitHub. Create a GitHub repository and send every file and directory except for virtual env's `env` directory to GitHub.

## Deploying the Sleep Tracker to Code Capsules

With all of the files on  GitHub, we can deploy the sleep tracer to Code Capsules:

1. Log in to Code Capsules, and create a Team and Space as necessary.
2. Link Code Capsules to the GitHub repository created [previously](#creating-the-procfile-and-adding-requirements
).
3. Enter your Code Capsules Space.
4. Create a new Capsule, selecting the "Backend" capsule type.
5. Select the GitHub repository containing the sleep tracker – leave "Repo subpath" empty and click "Next"
6. Leave the "Run Command" blank and click "Create Capsule"

Now we just need to set those environment variables we mentioned [previously](#creating-environment-variables).

### Creating environment variables in Code Capsules

Let's create set the environment variables so our sleep tracker will work properly: 

1. Navigate to your Capsule.
2. Click the "Config" tab.
3. Add two environment variables, one named "SECRET_KEY" and another "MONGO_CONNECTION_STRING". Enter the secret key and connection string values you saved earlier.

When done, **make sure** to click "Update". 

Now the sleep tracker is ready to try out! The application is complete.

## What Next?

There are many ways to expand or improve this application. Some ideas include:

* Improve the application's styling – it's fairly simple right now. If you want to learn more about CSS styling, this tutorial written by Mozilla is a [great place to start](https://developer.mozilla.org/en-US/docs/Web/CSS).
* Add a way for users to keep track of other data (calories, daily notes, exercise).
* Displaying better looking error messages, preferably somewhere in the current page.

