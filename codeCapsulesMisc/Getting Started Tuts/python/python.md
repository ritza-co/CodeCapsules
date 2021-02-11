
# Getting started on Code Capsules with Python's Flask
Deploy a Flask application and learn how to host backend code on Code Capsules. 

## Set up

Here we'll learn how to deploy backend code with Code Capsules and Flask. Because Code Capsules connects with GitHub to deploy code, you'll need a [GitHub](www.github.com) account to follow this tutorial. We'll use a sample Flask application provided by Code Capsules to demonstrate deployment – you can find the GitHub repository for the application here: https://github.com/codecapsules-io/demo-python

To deploy this repository, we'll need to fork the application by clicking "Fork" at the top right of the GitHub repository. Once forked, we're ready to deploy the application – feel free to make any edits to it, otherwise continue. 

## Prepare a Team

First we'll need to create an account with [Code Capsules](https://codecapsules.io/). Do so, and make sure to confirm your account with by checking your email. 

After creating a new account, you'll see a screen that looks like this:

![personal_team](images/personal_team.png)

Code Capsules provides a "Personal" [Team](https://codecapsules.io/docs/faq/what-is-a-team/) for you upon account creation. 

With a new team created, we can create a [Space](https://codecapsules.io/docs/faq/what-is-a-space/)

## Creating a Space and linking GitHub

Click "Create Space" again. The name here is irrelevant.

![space_img](images/create-space.png)

Next, we need to link our GitHub repository to our Code Capsules account.

Click the profile image on the top right of the page, and find the "GitHub" button.

![git-button](images/git-button.png)

Authorize Code Capsules to connect to the Node.js application by:

1. Clicking your GitHub username.
2. Selecting "Only Select Repositories".
3. Choosing the GitHub repository we forked.
4. Pressing "Install & Authorize".

![install&authorize](images/authorize_cc.png)

After clicking "Install & Authorize", Code Capsules links to the forked GitHub repository containing the Node.js application - this means we can now create a [Capsule](https://codecapsules.io/docs/faq/what-is-a-capsule/) that'll host the code found in our repository.

## Create the Capsule

Select the Space created previously and add your payment information to create a new Capsule, Afterward, click the "Create a New Capsule for Your Space" button.

1. Choose a "Backend Capsule".
2. Select the "Sandbox" product.
3. Choose the GitHub repository we forked.
4. Press next.
5. Leave the "Run Command" blank and create the Capsule.


You can view the build logs under the "Logs" tab in your Capsule. When the Capsule builds, navigate to the "Overview" tab and click on the provided URL to view the application. For a closer look at a Capsules's properties, refer to this [explanation on Capsules](https://codecapsules.io/docs/faq/what-is-a-capsule/).