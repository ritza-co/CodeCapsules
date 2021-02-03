

# Adding Functionality to Your Web Application: Setting up Stripe Checkout and Email Subscription with Flask and Code Capsules

## What We'll Cover

Constructing a frontend for your web application is the first step towards providing an interactive user experience. The next step is building a working backend, or, making your web application functional. Buttons are nice to have, but it's more interesting to have those buttons do something. That's what we'll focus on today. 

Through a step-by-step process, we'll develop this functionality. We'll use [Flask](https://flask.palletsprojects.com/en/1.1.x/) and a frontend template to create a web application that allows users to buy products through [Stripe Checkout](https://stripe.com/payments/checkout) (a tool for creating a "checkout" process for products) and subscribe to an email list with help from the [Mailgun](https://www.mailgun.com/ ) email API service. 

Then, we'll host the web application on Code Capsules so people around the world can buy your product and subscribe to your email list.


## Requirements

To successfully complete this project, we'll need:

- A text editor (like [Sublime](www.sublimetext.com) or [VSCode](https://code.visualstudio.com/)) installed.
- [Python 3.XX+](https://www.python.org/downloads/) installed.
- [Virtualenv](https://pypi.org/project/virtualenv/) installed.
- [Git](https://git-scm.com/) installed and a [GitHub](https://github.com/) account.
- A [Code Capsules](www.codecapsules.io) account. 

## Setting Up the Frontend

We'll use the [laurel](https://cruip.com/laurel/) frontend template from https://cruip.com to add our functionality to. This template is perfect for our project – there is already an email subscription box that just needs to be implemented, and, with a few modifications, we'll implement a "Buy Now" button. 

After downloading the laurel template:

1. Create a directory named `project`.

2. Within the `project` directory, create a sub-directory named `templates`. 

3. Open the downloaded template and extract the files within the `laurel` directory into the `templates` subdirectory. 

You can view the template by opening the `index.html` file in the `templates` subdirectory. We'll change the first "Early access" button to "Buy Now" and implement Stripe Checkout functionality for it. 

Then we'll change the second "Early access" button at the bottom of the template to "Subscribe", and implement the email subscription functionality for it. 

First, let's change the "Early access" button texts. 

### Modifying the "Early Access" Text

We'll start with changing the first "Early access" button to "Buy Now". Open the `index.html` file in a text editor.

Find the line

```html
<div class="hero-cta"><a class="button button-shadow" href="#">Learn more</a><a class="button button-primary button-shadow" href="#">Early access</a></div>
```

and replace it with:

```html
<div class="hero-cta"><a class="button button-shadow" href="#">Learn more</a><a id='checkout-button' class="button button-primary button-shadow" href="#">Buy Now</a></div>
```

As well as changing "Early access" to "Buy Now", we've also added `id=checkout-button`. The latter will be useful when implementing Stripe Checkout. 

Next, find the line 

```html
<a class="button button-primary button-block button-shadow" href="#">Early access</a>
```

and replace "Early access" with "Subscribe". 

View the changes by saving the `index.html` file and re-opening it in a web browser. We have one more task before building our Flask backend.

### Project Directory Restructuring 

Flask uses the [Jinja](https://jinja.palletsprojects.com/en/2.11.x/templates/) templating library to render frontend data. Using Flask, we can [render](https://flask.palletsprojects.com/en/1.1.x/tutorial/templates/) our `index.html` file, and implement our functionality. 

To render our template, Flask requires a specific directory structure. We need to make a couple of modifications to our `project` directory, so Flask can render our frontend.

First: 

1. Create a new directory named `static` in the `project` directory.

2. Navigate to the `templates` directory and then the `dist` directory. 

3. Copy all of the directories located in `dist` into the `static` directory that we created in step one.

Your `project` file structure should look like this:

```
project
    static
      css
        + style.css
      images
        + iphone-mockup.png
      js
        + main.min.js
    templates
```

This was necessary because Flask **strictly** renders CSS, Javascript, and images in folders named `static`, and renders `.html` files located within `templates`. Because we've shifted a few files locations, we need to edit our `index.html` file.

Open the `index.html` file in the `templates` folder and replace the line

```html
<link rel="stylesheet" href="dist/css/style.css">
```

with:

```html
<link rel="stylesheet" href="{{url_for('static',filename='css/style.css')}}">
``` 

`url_for()` helps Flask find the location of our `style.css` file when rendering the `index.html` file. This isn't as confusing as it looks – when we call `url_for('static',filename='css/style.css')`, we are saying:

_In the root directory, find a folder named static. Then find a file named "style.css" within the "css" folder"_.

Speaking of Flask – we're almost ready to implement our functionality. 

## Setting Up the Virtual Environment

We'll create a [Virtual environment](https://docs.python.org/3/library/venv.html) for our project. The virtual environment will be useful later on when we host our web application on Code Capsules, and it will ensure we are using the same Python libraries. 

To create a Virtual environment, navigate to the `project` directory in a terminal. Then, enter `virtualenv env`.

Activate the virtual environment with:

**Linux/MacOSX** `source env/bin/activate`

**Windows** `\env\Scripts\activate.bat`

If the virtual environment activated correctly, you'll notice `(env)` to the left of your name in the terminal. Keep this terminal open – we'll install the project requirements next. 

### Installing the Requirements 

For our project, we'll use the following libraries:

- [Flask](https://flask.palletsprojects.com/en/1.1.x/) is a lightweight Python web development framework. 

- [Gunicorn](https://gunicorn.org/) is the [WSGI server](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface) we'll use to host our application on Code Capsules. 

- [Requests](https://pypi.org/project/requests/) is a Python library that allows us to send [HTTP/1.1 requests](https://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html).  

- [Stripe](https://pypi.org/project/stripe/) is another Python library that will help us interact with the [Stripe API](https://stripe.com/docs/api).

Install these by entering  `pip3 install flask gunicorn requests stripe` within the virtual environment. 

Now we can build the backend for our web application. 

## Creating the Flask Application

In the `projects` folder, create a new file named `app.py`. This file will contain all of our Flask code. 

Open the `app.py` file in a text editor and enter the following: 

```python
from flask import Flask, render_template, request
import requests, stripe


app = Flask(__name__)

@app.route("/",methods=["GET","POST"]) 
def index():
  return render_template("index.html")

if __name__== '__main__':
  app.run(debug=True)   
```

We import multiple functions from `flask`. 
  - As the name implies, `render_template()` will render our `index.html` file.
  - `Flask` provides the Flask application object.
  - Not to be mistaken with `requests`, the `request` object contains any information sent to our web application – later this will be used to retrieve the email address entered in our subscription box.


Note `render_template("index.html")`. As mentioned above, this is where Flask reads the `index.html` file. It knows the location of the `index.html` file because Flask knows to look at the `templates` directory for `.html` files. 

Run `app.py` with `flask run` in your terminal. Open the provided IP address in your browser – the web application should look like this:

![images](images/website_init.png)

Let's make this web application useful and implement the first bit of functionality – the email list feature.

### Signing Up for Mailgun 

We'll use [Mailgun](https://www.mailgun.com/) to handle our email subscriber list. Mailgun is free, provided one doesn't send more than 5,000 emails per month. [Register with Mailgun](https://signup.mailgun.com/new/signup) and continue.

With an account registered, create a mailing list by:

1. Logging in to Mailgun.

2. Clicking "Sending" then "Mailing lists" on the dashboard.

    ![image2](images/mailing_lists.png) 

3. At the top right, press "Create mailing list"

4. Enter in whatever you'd like for the address prefix, name, and description – leave everything else default.

5. Click "Add mailing list"


Navigate to the mailing list we just created. You'll see something called an _Alias address_ – Mailgun provides every new mailing list with an alias address. When you send an email to your alias address, Mailgun sends a copy of the email to everyone who is subscribed to your mailing list. Jot down your alias address, we'll use it soon.

Next, you'll need to retrieve the [API key](https://cloud.google.com/endpoints/docs/openapi/when-why-api-key) for your Mailgun account. We'll use this API key in our web application to validate your Mailgun account when people subscribe to your mailing list.

Find the API key by clicking on your account at the top right of the screen. Click "API keys", and note your **private** API key.

## Implementing the Subscribe Button

With the mailing list created, we can create implement the subscribe button. 

Re-open the `index.html` file. At the bottom of the file, find the line:

```html
 <section class="newsletter section">
``` 

From the above line down to its corresponding `</section>` tag, replace all of the code with the following:

```html
<section class="newsletter section">
  <div class="container-sm">
    <div class="newsletter-inner section-inner">
      <div class="newsletter-header text-center">
        <h2 class="section-title mt-0">Stay in the know</h2>
        <p class="section-paragraph">Lorem ipsum is common placeholder text used to demonstrate the graphic elements of a document or visual presentation.</p>
      </div>
      <form method="POST">
        <div class="footer-form newsletter-form field field-grouped">
          <div class="control control-expanded">
            <input class="input" type="email" name="email" placeholder="Your best email&hellip;">
          </div>
          <div class="control">
            <button class="button button-primary button-block button-shadow" type="submit">Subscribe</a>
            </div>
          </div>
        </form>
      </div>
    </div>
</section>
```

Here we've added a `POST` method to the subscribe button – we'll see how we'll use this `POST` method to identify when a user has entered their email next. 

### Subscribe Functionality in Flask

Return to the `app.py`  file. Above the

```python
@app.route("/",methods["GET","POST"]) 
```
line, enter:

```python
def subscribe(user_email, email_list, api_key):
  return requests.post(
        "https://api.mailgun.net/v3/lists/"+email_list+"/members",
        auth=('api', api_key),
        data={'subscribed': True,
              'address': user_email,})
```

This function calls when a user hits the "Subscribe" button. The `subscribe()` function takes three variables:

- `user_email`: The email the user has entered.

- `email_list`: Your Mailgun alias address.

- `api_key`: Your Mailgun secret API key. 

The real logic is contained in the `return` line. Here, we use `requests.post()` to add the `user_email` to our `email_list`, by sending (or "posting") all of the values in `data` Mailguns [email list API](https://documentation.mailgun.com/en/latest/api_reference.html).

Next, modify the current `index()` function like below, replacing `MAILGUN_ALIAS` and `YOUR-MAILGUN-PRIVATE-KEY'` with the email alias and private API key we previously retrieved:

```python
@app.route("/",methods["GET","POST"]) 
def index():
  if request.method == "POST":
    user_email = request.form.get('email') 
    response = subscribe(user_email,
      'MAILGUN_ALIAS',
      'YOUR-MAILGUN-PRIVATE-KEY') 

  return render_template("index.html")
```

In the [previous section](#implementing-the-subscribe-button), we added the `POST` method to the subscribe button. We know when someone has clicked the subscribe button with the line: 
```python
if request.method == "POST":
```
If they've clicked the subscribe button, we obtain the email they entered with `user_email = request.form.get('email')`, then we add the email to the mailing list.

Try it out  enter an email address and hit "Subscribe". Navigate back to the mailing lists tab on Mailgun and click on the list we created. You can find the email address entered under "Recipients".

All that's left is to add functionality to our "Buy Now" button. 

## Implementing "Buy Now" with Stripe Checkout

Stripe Checkout allows business owners to accept payments on their web applications. Let's create an account with [Stripe Checkout](https://dashboard.stripe.com/register). After creating an account, log in and find your API keys by clicking "Developers" then "API keys" on the dashboard. 

![stripe-dashboard](images/stripe_dashboard.png)

Here we'll see two API keys – a "Publishable" API key, and a "Secret" API key. Stripe uses the publishable API key to identify your account. We'll use the secret API key to send requests to the Stripe API. 


Open the `app.py` file. Above the subscribe function, add the following lines, replacing `YOUR PUBLISHABLE KEY HERE` and `YOUR SECRET KEY HERE` appropriately:

```python
app.config['STRIPE_PUBLISH_KEY'] = 'YOUR PUBLISHABLE KEY HERE'
app.config['STRIPE_SECRET_KEY'] = 'YOUR SECRET KEY HERE'
stripe.api_key = app.config['STRIPE_SECRET_KEY']
```

We place the two Stripe keys in our Flasks configuration settings for ease of access, then set our Stripe secret API key. 

These are test API keys that we'll use to check out our product – with these keys, no charge will incur for making a payment. However, before we can make a payment, we need a product – let's create one. Return to Stripe, login, and navigate to the "Products" tab on the dashboard. 

### Creating a Product

Create a product by performing the following:

1. Click "Add product" on the top right.
2. Name the product.
3. Add a description and price.
4. Choose "One time" payment.
  ![example_product](images/example_product.png)

5. Click "Save product"

After the last step, save the API key found in the "Pricing" section. We'll use this API key to tell Stripe which product we want our customers to pay for. 

### Adding Functionality in Flask

Time to create the "Buy Now" button logic. Open `app.py` again and modify the index function accordingly. Replace "YOUR-PRICE-API-KEY" with the API key for your product, that we saved in the previous section. 

```python
@app.route("/",methods=["GET","POST"]) 
def index():
  session = stripe.checkout.Session.create(
    payment_method_types=['card'],
    mode = 'payment',
    success_url = 'https://example.com/success',
    cancel_url = 'https://example.com/cancel',
    line_items=[{'price':'YOUR-PRICE-API-KEY',
          'quantity':1,
    }]
    )

  if request.method == "POST":
    user_email = request.form.get('email') 

    response = subscribe(user_email,
      'my_list@sandboxc67e2c0ba1e64bdbb055e64d41135bb4.mailgun.org',
      'YOUR-MAILGUN-PRIVATE-KEY') 

  return render_template("index.html", 
    checkout_id=session['id'],
    checkout_pk=app.config['STRIPE_PK'],
    )
```

We use the `stripe` library to create a new "Session" object. This object contains multiple variables affecting how our customers interact with the "Buy Now" button. For more information on these variables, read Stripes [documentation](https://stripe.com/docs/api/checkout/sessions/object)

We also return two new variables – `checkout_id` and `checkout_pk`. `checkout_id` stores information about the potential purchase (price, payment type, etc). The `checkout_pk` variable stores our private API key – when a customer buys our product, the money sends to the account associated with this private API key.

Let's see how our HTML code will utilize these new variables and redirect us to the Stripe Checkout page next. 

### "Buy Now" Button Functionality in the HTML File

With our Flask logic finished, we can implement the "Buy Now" button functionality in our `index.html` file. Open the `index.html` and and find the section:

```html
<div class="hero-copy">
  <h1 class="hero-title mt-0">Landing template for startups</h1>
  <p class="hero-paragraph">Our landing page template works on all devices, so you only have to set it up once, and get beautiful results forever.</p>
  <div class="hero-cta"><a class="button button-shadow" href="#">Learn more</a><a id='checkout-button' class="button button-primary button-shadow" href="#">Buy Now</a></div>
</div>

```

Directly below the `</div>` line, add:

```html

<script src="https://js.stripe.com/v3/"></script>
<script>
  const checkout_pk= '{{checkout_pk}}';
  const checkout_id = '{{checkout_id}}';
  var stripe = Stripe(checkout_pk)
  const button = document.querySelector('#checkout-button')

  button.addEventListener('click', event =>{
    stripe.redirectToCheckout({
      sessionId: checkout_id
      }).then(function(result){

        });
        })
</script>
```

This code adds a Javascript event – when a customer clicks the "Buy Now" button, the customer redirects to the Stripe Checkout page. The Stripe Checkout page changes according to the information stored in `checkout_id`

Also, take a look at the lines: 

```javascript
const checkout_pk= '{{checkout_pk}}';
const checkout_id = '{{checkout_id}}'; 
```

These variables evaluate to the variables returned when calling the `render_template()` function in `app.py`. This is another piece of the [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) templating language syntax that Flask uses when calling `render_template()`.

The "Buy Now" button is good to go – run `app.py` again. Try making a payment –  use the credit card number `4242 4242 4242 4242` (This is a [test credit-card number](https://stripe.com/docs/testing), no charges will incur with its use) and enter any name, expiration date, and security code.


## Hosting the Application on Code Capsules

Now that we've added all the functionality, we need to create some files that Code Capsules will use when hosting our application. We'll also take a look at a security problem we have in our current application, and how to fix it.

### Creating the "requirements.txt" File and Procfile

To host this application on Code Capsules, we need to create a requirements.txt file and a Procfile. 

1. In the `project` directory, ensure you the virtual environment is activated and enter `pip3 freeze > requirements.txt`. 

2. Create another file named `Procfile` and add the line `web: gunicorn app:app` – save the `Procfile`.

With the `requirements.txt` file, Code Capsules will now know what libraries to install to run the application. The `Procfile` tells Code Capsules to use the `gunicorn` WSGI server.

Now we can fix that security problem mentioned before, and host the application.

### Removing Secret Keys

We need to send our application to GitHub so Code Capsules can host it. Remember – this project contains all of our API keys. It is considered **very** poor practice to send personal API keys to GitHub – anyone could use them and incur charges on your debit card. Luckily, there is a workaround. 

By working with [Environment variables](https://opensource.com/article/19/8/what-are-environment-variables), we can use our API keys without exposing them on GitHub. We'll soon add these environment variables containing our API keys on Code Capsules.

If you haven't worked with Environment variables before, their use will become clear when we get to hosting the application on Code Capsules.

To access the environment variables we'll set on Code Capsules, use this final iteration of our Flask application:

```python
from flask import Flask, render_template, request
import requests, stripe, os


app = Flask(__name__)
app.config['STRIPE_PK'] = os.getenv("STRIPE_PK")
app.config['STRIPE_SK'] = os.getenv("STRIPE_SK")
stripe.api_key = app.config['STRIPE_SK']


def subscribe(user_email, email_list, api_key):
  return requests.post(
        "https://api.mailgun.net/v3/lists/"+email_list+"/members",
        auth=('api', api_key),
        data={'subscribed': True,
              'address': user_email,})

@app.route("/",methods=["GET","POST"]) 
def index():

  session = stripe.checkout.Session.create(
    payment_method_types=['card'],
    mode = 'payment',
    success_url = 'https://example.com/success',
    cancel_url = 'https://example.com/cancel',
    line_items=[{'price':'price_1I9xueJaVOFHXZDzV46F934h',
          'quantity':1,
    }]
    )

  if request.method == "POST":
    user_email = request.form.get('email') 

    response = subscribe(user_email,
      'my_list@sandboxc67e2c0ba1e64bdbb055e64d41135bb4.mailgun.org',
      os.getenv("MAILGUN_SK")) 

  return render_template("index.html", 
    checkout_id=session['id'],
    checkout_pk=app.config['STRIPE_PK'],
    )

if __name__== '__main__':
  app.run(debug=True)   

```

The changes:

- We've import a new Python module, [os](https://docs.python.org/3/library/os.html).

- We've replaced `app.config['STRIPE_PK']`, `app.config['STRIPE_SK']` and the Mailgun private key with:
  - `app.config['STRIPE_PK'] = os.getenv("STRIPE_PK")`

  - `app.config['STRIPE_SK'] = os.getenv("STRIPE_SK")` 

  - and `os.getenv("MAILGUN_SK")` respectively.

`os.getenv("ENV-VAR-NAME-HERE")` retrieves any environment variable named within the quotation marks. On Code Capsules, we'll set environment variables named `'STRIPE_PK'`, `'STRIPE_SK'`, and `'MAILGUN_SK'`. This way, our API keys will remain secret. 

We can now safely host our application.

### Pushing the Application to GitHub and Hosting the Application on Code Capsules

Take the following steps before we create a Capsule that will host our code:

1. Create a GitHub repository for the application.
2. Send all code within the `project` directory to the repository on GitHub.
3. Log in to [Code Capsules](www.codecapsules.io).
4. Grant Code Capsules access to the repository.
5. Create a Team and Space as necessary. 

Let's create the Capsule: 

1. Press "Create A New Capsule".
2. Select your repository.
3. Choose the Backend Capsule type.
4. Create the Capsule.

All that's left is to set the environment variables. Navigate to the Capsule and click on the "Config" tab. Use the image below as a guide to properly add your environment variables. Replace each value with the appropriate API key. 

![environment-variables](images/enviro_vars.png)

After entering the API keys, __make sure to click update__. 

All done - now anyone can view the web application and interact with the "Buy Now" and "Subscribe" buttons.

## Further Reading 

We covered a lot in this tutorial - how to use Flask to implement functionality for frontend code, how to set up an email subscriber list, and how to work with Stripe. 

Earlier I mentioned more information on the `url_for()` function. Check out [Flasks documentation](https://flask.palletsprojects.com/en/1.1.x/api/#flask.url_for) for more information. 

For further information on how `render_template()` works, or if you wanted to know what that `const checkout_pk= '{{checkout_pk}}';` line was doing, check out this link to learn more about [Flask templating and Jinja2](https://realpython.com/primer-on-jinja-templating/). 

Now that you have a functional email-subscriber list, you may be interested in [sending emails to your list](https://documentation.mailgun.com/en/latest/user_manual.html?highlight=template%20variables#sending-messages).