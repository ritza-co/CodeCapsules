# Developing a Persistent Sleep Tracker Part 2: Tracking and Graphing Sleep Data

## Recap

We're creating a web application that allows users to log their sleep data. This application has two parts:

- A login-management system. 

- A way to log sleep data and display it via a graph.

We've already created the login-management system in the previous tutorial. If you haven't already completed the first tutorial, [check it our here](link-to-previous-tutorial.io).

To recap, last time we:

- Learned about Flask's [template inheritance](https://flask.palletsprojects.com/en/1.1.x/patterns/templateinheritance/).
- Implemented our `index.html` file.
- Connected a [MongDB](https://www.mongodb.com/cloud/atlas) to our web-application.
- Created a login management system with the help of [Flask-Login](https://flask-login.readthedocs.io/en/latest/).

In this part of the guide, we'll implement the actual sleep tracker logic - we'll develop our `main.html` file and learn how to dynamically display interactive [Plotly](https://plotly.com/) graphs in HTML. The general flow of the application work like: When logged in, users will be able to track how many hours they slept on a given date. Then, users will be able to display a graph with all of the sleep data they've logged. 

Let's implement the ability to track and graph sleep data!

## Requirements

Because this is the final part in a two-part series on how to create a sleep tracker, make sure you've followed the [previous tutorial](link-to-previous-tutorial-here). It'll also help (but is not necessary) to have some Javascript experience when we implement the graph that displays sleep data. If you've already finished the previous tutorial, continue. 

## Expanding the Main.html file

Now that users can register an account with our sleep application and log in, we need to implement the `main.html` file. Right now, our `main.html` file is empty - we need to implement the following: 

1. A text box where users can input the number of hours they've slept.

2. A date field, where users can choose the date that they're tracking sleep data for.

3. A "Submit" button, that sends the date and hours slept to the user's MongoDB entry.

4. A "View Graph" button, that displays the information they've logged.

5. A "Logout" button, that logs the user out. 


To create a graph based on user-logged sleep data, we'll see how to send user data from Flask to the `main.html` file. Doing this, we'll also be able to display a personalized message to the user: "Hey, (username here)! Let's track your sleep.".  

Let's get to it.

### Editing the main.html file

Open the `main.html` file in the `templates` directory. Add the following:

```html
{% extends "base.html" %}

{% block content %}
<h2>Hey, {{user['username']}}! Let's track your sleep.</h2>
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

This works similarly to the `login.html` file we created in the previous tutorial. We reuse the `base.html` markup with the `{% extends "base.html" %}` line and our unique content is entered between the `{% block content %}...{% endblock %}`lines. We also create another `POST` method to know when a user clicks our ""Submit", "Logout", or "View Graph" buttons. 

To know which button a user clicks, we'll be able to use the HTML `name` variable set for our buttons (`'submit', 'graph', 'logout'`) in Flask.

Notice the line `{{user['username']}}` wrapped in the `<h2>...</h2>` blocks. This Jinja code will display the username of the user logged in to our sleep tracker. As mentioned before, we'll send our user's data from Flask (by accessing our MongoDB) to our `main.html` file so that the username will display.

The `<input type="date" id="date" name="date" value="">` creates an interactive calendar so users can click on the date that they want to log their sleep for. Now let's implement the code to make these buttons work.

### Main.html button functionality

In the `app.py` file, add the following line below the `app = Flask(__name__)`  line.

```python
app.config['plotting'] = False
```

Here we add a new entry in our Flask application's configuration settings. This will come in handy soon - we'll use this line to tell whether or not a user has clicked the "View Graph" button in our `main.html` file. If a user has clicked the button, we'll set this line to "True" and a graph with the user's sleep data will display. 

Now, we can implement the functionality for our "Submit", "View Graph" and "Logout" buttons. Add the following code below the `main` function.

```python
@app.route('/main', methods=['POST','GET'])
def submit_sleep():
    if request.method == 'POST':

        if request.form.get('submit'): # if submitting new sleep data
            time_entered = float(request.form.get('time'))
            date_entered = request.form.get('date')
            add_sleep(time_entered,date_entered,db.users.find_one({'username':current_user.get_id()}))

        if request.form.get('logout'):
            logout_user()
            app.config['plotting'] = False
            return 'You logged out!'

        elif request.form.get('graph'): 
            app.config['plotting'] = True

    return redirect(url_for('main'))
```

This function operates similarly as our `login_or_register` function:

- If a user clicks "Submit", the data they entered will send to their MongoDB entry via the `add_sleep` function (that we'll create next). 

- If a user clicks "Logout", the user logs out. 

- If a user clicks "View Graph" the `app.config['plotting']` entry is set to `True`. We'll have to add some more code in our `main.html` file to display the graph.

Let's wrap up our button functionality by creating the `add_sleep` function that is called when a user clicks "Submit"

### Creating the "add_sleep" function

Add the following code **above** the `submit_logout_plot` function to create the `add_sleep` function:

```python 
def add_sleep(time,date,user):
    if 'date' in user:
        user['date'].append(date)
        user['time'].append(time)
    else: # If first time adding sleep data
        user['date'] = [date]
        user['time'] = [time]

    # Update MongoDB Atlas
    db.users.update_one({'username':user['username']},
        {'$set':{'date':user['date'],
        'time':user['time']}})
```

Our `add_sleep` function takes three variables:

-`time`: The number of hours slept that the user entered.

-`date`: The calendar date the user selected.

-`user`: The user's MongoDB entry.

If a user has never entered any sleep data, we add a new `'date'` and `'time'` entry to the user's MongoDB entry with the date and time entered. Otherwise, we add the data they entered to their existing sleep data. Finally, we update the user's MongoDB entry with `db.users.update_one`. 

We've implemented the button's functionality. Now we need to modify the `main()` function, and add the graph display in our `main.html` file.

###  Modifying the main function

Modify the `main` function like so:

```python
@app.route('/main')
def main():
    user_data = db.users.find_one({'username':current_user.get_id()})
    return render_template("main.html",user=user_data,plot=app.config['plotting'])
```

Here we retrieve the MongoDB entry of the user that is currently logged in and assign it to `user_data`. Then we make some modifications to the `render_template` function.

Remember: in our `main.html` file, we wanted to display a message welcoming the user. In the `main.html` file, we retrieve their name with the line `{{user['username']}}`. Take a look at the `render_template` function in the above code.

In the `render_template` function we:

- Create the variable `user`. This is where we send the user's data to the `main.html` file. With the user's data, we can display their username (with the line `{{user['username]}}`). 
  - This `user` variable will also contain any sleep data they've entered.

- Add a `plot` variable and set it to `app.config['plotting']`.
  - This `plot` variable will let our `main.html` file know whether or not to create a graph that displays the sleep data the user has entered. 

Almost done! All that's left is to add a Plotly graph in our `main.html` file that will display the user's data. After, we can deploy our application to Code Capsules.

## Adding the Plotly Graph

As mentioned at the beginning of the article, we'll add the ability to graph sleep data with the help of [Plotly](https://plotly.com/). Plotly provides an external Javascript "script" that we can use to create interactive graphs in HTML files. Since we send all of the user's data to our `main.html` file via Flask, we have access to all of the dates and times they've logged. 

Let's create the Plotly graph - open the `main.html` file and continue.

### Final touches to the Main.html file

In the `main.html` file, find the `</form>` line. Right below the `</form>` line, add the following: 

```html
{% if plot %}
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<div id="graph">
    <script>
      var x = {{user['date'] | safe}};
      var y = {{user['time'] | safe}};

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

Let's break this down, starting with the line `{% if plot %}`. This line references the `plot` variable we created in our `app.py` file in the `main` function. If someone has clicked "View Graph", we set `plot`  to true. If `plot` is true, the rest of the code under `{% if plot %}` will execute.

The line `<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>` imports the external Plotly script for Javascript. This way, we can create a Plotly graph in HTML.

Under this line, we see a lot of code enclosed in `<script>...</script>` tags. The `<script>` HTML tag allows developers to embed Javascript code. Looking at the code in the `<script>...</script>` tags:

- We create two variables, `var x` and `var y` which contain the dates and amount of hours slept that a user has logged.
  - We set the variables as `safe` - this means our variables contain no special characters that the HTML could misinterpret. 

- The code inside `var trace1 = {...}` tells Plotly which data to use for the x and y axes, and the type of graph we'll make (a bar graph).

- All the code in `var layout = {...}` effects things like x and y axis labelling, font type, and size of font. Customise this yourself!

- The line `Plotly.newPlot('graph', data, layout)` creates the actual graph and displays it to a user. 

All done. Try running the application by opening a terminal in the `sleep-tracker` directory and entering `flask run`. You should be able to register a new user account, log in, track your sleep data, and view your graph. 

With our sleep tracker implemented, we need to make one last modification to our `app.py` file and add some files in the `sleep-tracker` directory before we can push our code to GitHub and deploy it on Code Capsules. 

### Creating environment variables

Before we push our code to GitHub, we need to "hide" our `app.config['SECRET_KEY']` and the MongoDB user credentials. If we were to push our code now, anyone could use our secret key or our MongoDB credentials and alter our database. Luckily, there is an easy fix. 

In the `app.py` file, add the line

```python
import os
```

to the top of the file. Then, replace the line

```python
app.config['SECRET_KEY'] = 'your-secret-key-here' 
```

with

```python
app.config['SECRET_KEY'] = os.getenv('SECRET_KEY') 
```

Finally, do the same for the MongoDB user credentials. Replace the line

```python
client = pymongo.MongoClient('mongodb+srv://YOURUSERNAME:<password>@cluster0.e2fw3.mongodb.net/<dbname>?retryWrites=true&w=majorhostity')
```

with

```python
client = pymongo.MongoClient('MONGO_USER')
```


`os.getenv('SECRET_KEY')` and `os.getenv('MONGO_USER')` will look for [environment variables](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa) with the names "SECRET_KEY" and "MONGO_USER". When we host the sleep tracker application on Code Capsules, we'll set these environment variables. 

The "SECRET_KEY" variable will store the secret key we set in the previous tutorial, and the "MONGO_USER" variable will store `mongodb+srv://YOURUSERNAME:<password>@cluster0.e2fw3.mongodb.net/<dbname>?retryWrites=true&w=majorhostit` with "YOURUSERNAME" and the "<password>" corresponding to your MongoDB Atlas user account information. 

With that done, we have some files we need to create - then we can push our code to GitHub and deploy it on Code Capsule.

## Creating a Procfile and Adding Requirements

Before we can push our code to GitHub so Code Capsules can deploy it, we need to create some files so Code Capsules can run our application. First, we'll need to create  `Procfile`. This file will tell Code Capsules how to run our `app.py`  file.

Then we'll create a file `requirements.txt`. This file will tell Code Capsules which tell Code Capsules which libraries to install to be able to run our `app.py` file.

To create the `Procfile`:

1. Navigate to the `sleep-tracker` directory and enter the virtual environment.

2. Create a file named `Procfile` (do **not** add a file extension).

3. Open the `Procfile`, enter `web: gunicorn app:app`, and save the file. This tells Code Capsules to use the Gunicorn WSGI server to run our `app.py` file

In the same terminal,  enter `pip3 freeze > requirements.txt` to generate a list of requirements for our Code Capsules server.

Now we can push our code to GitHub. Create a GitHub repository and send every file and directory except for virtual env's `env` directory to GitHub.

## Deploying the Sleep Tracer to Code Capsules

With all of the files on  GitHub, we can deploy the sleep tracer to Code Capsules:

1. Log in to Code Capsules, and create a Team and Space as necessary.

2. Link Code Capsules to the GitHub repository created [previously](#creating-the-procfile-and-adding-requirements
).
3. Enter your Code Capsules Space.
4. Create a new Capsule, selecting the "Backend" capsule type.
5. Select the GitHub repository containing the sleep tracker – leave "Repo subpath" empty and press "Next"
6. Leave the "Run Command" blank and press "Create Capsule"

Now we just need to set those environment variables we created [previously](#creating-environment-variables).

### Creating environment variables in Code Capsules

Let's create set the environment variables so our sleep tracker will work properly: 

1. Navigate to your Capsule.

2. Click the "Config" tab.
3. Add two environment variables, one named "SECRET_KEY" and another "MONGO_USER" 
  - Make the value for "SECRET KEY" anything you'd like.
  - For "MONGO_USER", make the value `mongodb+srv://YOURUSERNAME:<password>@cluster0.e2fw3.mongodb.net/<dbname>?retryWrites=true&w=majorhostit` and replace "YOURUSERNAME" and "<password>" with your MongoDB Atlas user credentials.
  
When done, **make sure** to click "Update". 

Now the sleep tracker is ready to try out! The application is complete.

## What next?

There are many ways to expand or improve this application. Some ideas include:

- Improve the application's styling - it's fairly simple right now. If you want to learn more about CSS styling, this tutorial written by Mozilla is a [great place to start](https://developer.mozilla.org/en-US/docs/Web/CSS).

- Add a way for users to keep track of other data (calories, daily notes, et cetera).

- Display a warning that the username or password supplied was incorrect.

