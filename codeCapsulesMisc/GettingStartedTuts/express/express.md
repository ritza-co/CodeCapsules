
# Getting Started on Code Capsules with Express.js
Deploy a Express JS application and learn how to host backend code on Code Capsules. 

## Set up
Code Capsules connects to GitHub repositories to deploy code online, so you'll need a [GitHub](www.github.com) account to follow this tutorial. 

We'll learn how to deploy backend code with Code Capsules by following this process, using a sample Express.js application provided by Code Capsules. Find the GitHub repository containing the sample Express.js application here: https://github.com/codecapsules-io/demo-express-js

To use this repository, fork the application by navigating to the repository and clicking "Fork" at the top-right of the screen. Once you've forked the application, we are ready to deploy it – no edits necessary. 

## Prepare a Team

To deploy the Express.js web application, we need to create an account with Code Capsules.

To create an account:

1. Register an account at https://codecapsules.io/.
    ![account-creation](images/create-account.png)
2. Confirm your account by viewing the email sent to you.

After creating a new account, you'll see a prompt to create a [Team](Link-to-team-explanation-here). Create a Team, and name it anything you'd like.

![team-creation](images/team-creation.png)

With a new Team created, we can create a [Space](https://codecapsules.io/docs/faq/what-is-a-space/)

## Creating a Space and Linking GitHub

Click "Create Space" again. The name here is irrelevant.

![space_img](images/create-space.png)

Next, we need to link our GitHub repository to our Code Capsules account.

Click the profile image on the top right of the page, and find the "GitHub" button.

![git-button](images/git-button.png)

Authorize Code Capsules to connect to the Express.js application by:

1. Clicking your GitHub username.
2. Selecting "Only Select Repositories".
3. Choosing the GitHub repository we forked.
4. Pressing "Install & Authorize".

![install&authorize](images/authorize_cc.png)

After clicking "Install & Authorize", Code Capsules links to the forked GitHub repository containing the Express.js application – this means we can now create a [Capsule](https://codecapsules.io/docs/faq/what-is-a-capsule/) that'll host the code found in our repository.

## Create the Capsule

Select the Space created previously and add your payment information to create a new Capsule, Afterward, click the "Create a New Capsule for Your Space" button.

1. Choose a "Backend Capsule".
2. Select the "Sandbox" product.
3. Choose the GitHub repository we forked.
4. Press "Next".
5. Leave the "Run Command" blank and create the Capsule.

You can view the build logs under the "Logs" tab in your Capsule. For a closer look at a Capsules's properties, refer to this [explanation on Capsules](https://codecapsules.io/docs/faq/what-is-a-capsule/). When the Capsule is built, navigate to the "Overview" tab and click on the URL provided to view the application. 

This application also exposes a `GET` endpoint at `/api/data`. If [curl](https://curl.se/download.html) is installed on your computer (MacOSX and Linux distributions generally have curl installed), we can request the data exposed at that endpoint using the URL that Code Capsules provided. Try this in your terminal:

`curl https://<Your-CodeCapsules-URL/api/data`

The response should look like: 

{`"data":[{"id":1,"title":"Some data"},{"id":2,"title":"Some other data"}]}`

If you'd like to deploy another Express.js application in the future, take a close look at the `package.json` file in the [repository we forked](https://github.com/codecapsules-io/demo-express-js). On line six you can see the "start" script that Code Capsules used to build and deploy the Express.js application. 
