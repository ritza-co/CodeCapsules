# How to Deploy a Flask Server on Code Capsules

If you've built a server with Flask, you probably want to deploy it to production to use it as an interface receiving and posting data. In this guide, we'll show you how to do that, step by step.

You'll need 

* Your own [GitHub](https://github.com) account
* An account on [Code Capsules](https://codecapsules.io)

## Getting some example code

We'll use [a demo Flask application](https://github.com/AnesuMurakata/demo-flask) for the demonstration, but you can use your own Flask application instead, or work from the one mentioned beforehand as a starting point.

Fork the project to your own account by clicking on the above link and pressing "fork" in the top right.

## Code Capsules sign up

Navigate to [Code Capsules](https://codecapsules.io/) and click on the "Sign Up" button in the top right corner of the webpage. Alternatively, you can click on the "Log In" text next to the Sign Up button if you already have a registered Code Capsules account. Enter your details to sign up for an account or to log into an existing one. 

If you've just signed up for an account you will be directed to the Welcome page on your first login. Click on the "Go To Personal Team" button to advance to the next step.

![](images/welcome-screen.JPG)

After clicking on the "Go To Personal Team" button you will be redirected to the Spaces tab for your Personal Team. Code Capsules gives every account a Personal Team as a default. 

## Create a Space for your apps

![](images/spaces.JPG)

Now click on the "Create A New Space For Your Apps" button. Alternatively, if you had logged into an existing Code Capsules account you could click on the "New Space" button to create a new space for this tutorial or select any of your existing ones. After actioning either of these steps a UI similar to the one shown below should slide in from the right of your screen.  

<img width="1083" alt="CleanShot 2021-03-22 at 16 36 02@2x" src="https://user-images.githubusercontent.com/2641205/112016130-b8316980-8b2c-11eb-8141-629f1658ddca.png">

Select an appropriate region and enter a name for your space and press "Create Space".

## Linking to GitHub

To host our Flask application on Code Capsules, we need to link our forked GitHub repository to our Code Capsules account.
 
We can link the Flask application by clicking the profile image at the top right of the screen, and finding the GitHub button under "GitHub Details"

![git-button](images/git-button.png)

Click the GitHub button. To give Code Capsules access to the Flask application:

1. Click your GitHub username.
2. Select "Only Select Repositories".
3. Choose the GitHub repository we forked.
4. Press "Install & Authorize".

![Install & authorize github](images/github-integration.gif)

After authorizing, Code Capsules will be able to read the contents of the selected repositories. All that's left to deploy the application is to add the repo to your team and create a [Capsule](https://codecapsules.io/docs/faq/what-is-a-capsule). This Capsule will act as a storage space for the Flask application.

## Add Repo to Team

Click on "Team Settings" on the top navigation bar to switch to the Team Settings tab. Once there, click on the Modify button under the "Team Repos" section to add the repo to your Personal Team. When the "Edit Team Repos" screen slides in select "Add" next to the repo you want to add to your Personal Team and then confirm. After this is done, all Spaces in your team will have access to this repo. 

![Edit Team Repos](images/team-repos.JPG)

## Create the Capsule

Return to the "Spaces" tab. Next, click on the Space you just created or are using and create a new capsule in that space. To do this, click the "New Capsule" or "Create A New Capsule For Your Space" button when inside the space.

As we want to host a flask application choose to create a backend capsule as shown below.

![Create Backend Capsule](images/creating-backend-capsule.gif)

Select the repository with the project you want to host on Code Capsules and enter the build command that will be used by Code Capsules when building your project. 

After creating the Capsule, the Capsule will build the Flask application. You can view the build logs by clicking the "Logs" tab in the Capsule. 

![Build logs](images/backend-capsule-build-logs.gif)

Once built, navigate to the "Overview" tab. Code Capsules provides a default URL for viewing applications - find this under "domains". Click the URL to view the application.

