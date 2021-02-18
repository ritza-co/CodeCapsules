
# Getting started on Code Capsules with Python's Flask
Deploy a Flask application and learn how to host backend code on Code Capsules. 

## Set up

Here we'll learn how to deploy backend code with Code Capsules and Flask. Because Code Capsules connects with GitHub to deploy code, you'll need a [GitHub](www.github.com) account to follow this tutorial. We'll use a sample Flask application provided by Code Capsules to demonstrate deployment – you can find the GitHub repository for the application here: https://github.com/codecapsules-io/demo-python

To deploy this repository, we'll need to fork the application by clicking "Fork" at the top right of the GitHub repository. Once forked, we're ready to deploy the application – feel free to make any edits to it, otherwise continue. 

## Creating an Account with Code Capsules

First, we'll need to create an account with [Code Capsules](https://codecapsules.io/). Do so, and make sure to confirm your account by checking your email. 

After creating a new account, you'll be greeted with a screen that looks like this: 

![login_screen](images/login.png)

Note the "Team Personal". Code Capsules provides a personal [Team](https://codecapsules.io/docs/faq/what-is-a-team/) and a personal [Space](https://codecapsules.io/docs/faq/what-is-a-space/) (located in the middle of the screen) to every new user. This default Team allows users to host static frontend websites for free. For this tutorial, we'll use the provided Team and Space.

For this tutorial, we'll need to add payment information to create a backend Capsule. Do so by navigating to "Team Settings" at the top of the screen, then add a payment method under "Payment Methods". 

Once you've added payment information, we need to permit Code Capsules to access our GitHub repositories. 

## Linking to GitHub

So we can host our Flask application on Code Capsules, we need to link our forked GitHub repository to our Code Capsules account.

Find the profile image at the top right of the page and click on it. You'll see a GitHub button located under "GitHub" details. 

![git-button](images/git-button.png)

Click on the GitHub button. You can authorize Code Capsules to connect to the Flask application by:

1. Clicking your GitHub username.
2. Selecting "Only Select Repositories".
3. Choosing the GitHub repository we forked.
4. Pressing "Install & Authorize".

![install&authorize](images/authorize_cc.png)

Once you've clicked the "Install & Authorize" button, Code Capsules will link to the GitHub repository containing the Flask application. Now, all we have left is to create a [Capsule](https://codecapsules.io/docs/faq/what-is-a-capsule) that will host our Flask application.

## Create the Capsule

Return to the personal Team, then click on the personal Space in the centre of the screen. 

![space_image](images/space.png)

We can deploy our Flask application to Code Capsules by clicking the "Create a New Capsule for Your Space" button. Do so, then:

1. Choose a "Backend Capsule".
2. Select the "Sandbox" product.
3. Choose the GitHub repository we forked.
4. Press next.
5. Leave the "Run Command" blank and create the Capsule.

While the Capsule is building, you can view its logs under the "Logs" tab in the Capsule. Once built, you can navigate to the "Overview" tab and click on the provided URL to view the application. For a closer look at capsules, take a look at this explanation of [Capsules](https://codecapsules.io/docs/faq/what-is-a-capsule).

If you'd like to deploy your own Flask application, take a close look at the [Flask repository](https://github.com/codecapsules-io/demo-python) we forked. There you'll find a file named `Procfile`. Code Capsules reads Procfiles' to build and deploy Flask applications. Click [here to read more about Procfiles](https://pythonhosted.org/deis/using_deis/process-types/). 