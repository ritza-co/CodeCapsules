# Getting started on Code Capsules with a static HTML site
Deploy a static HTML site and learn how to host frontend code on Code Capsules.

## Set up

This tutorial will focus on deploying frontend code to Code Capsules - we'll deploy a simple static HTML website and see how this process works. The HTML code has already been provided by Code Capsules. Because the Code Capsules uses GitHub to deploy code online, and because the HTML code we're going to use today is stored in a GitHub repository, make sure to register an account with [GitHub]([www.GitHub.io). 

When ready, you can find the HTML code at this GitHub repository: https://github.com/codecapsules-io/demo-html. 

So we can deploy this repository to Code Capsules, we'll need to fork the repository. Fork the repository by clicking the Fork button at the top right of the repository. Feel free to make any edits to the HTML code, otherwise, we can continue and get to deploying the static website.

## Creating an Account with Code Capsules

Before continuing, we'll need to create an account with [Code Capsules](https://codecapsules.io/). Once you've created an account, make sure to confirm your account by checking your email for a message from Code Capsules. After creating the account you'll be redirected to a page that looks like this:

![login_screen](images/login.png)

NAt the top left, note the "Team Personal". For every new user, Code Capsules provides a personal [Team](https://codecapsules.io/docs/faq/what-is-a-team/) and a personal [Space](https://codecapsules.io/docs/faq/what-is-a-space/) (located in the middle of the screen). This default "personal" Team allows users to host static frontend websites for free. For this tutorial, we'll use the provided Team and Space.

## Linking to GitHub

Code Capsules deploys code online by "linking" to your GitHub repositories. For Code Capsules to gain access to the forked repository containing the HTML code, we need to permit it to do so. 

We can permit Code Capsules to use the forked repository by clicking the profile image at the top right of the page. Here we'll see a GitHub button located under "details".

![git-button](images/git-button.png)

Click on the GitHub button. You can authorize Code Capsules to connect to the forked repository by:

1. Clicking your GitHub username.
2. Selecting "Only Select Repositories".
3. Choosing the GitHub repository we forked.
4. Pressing "Install & Authorize".

![install&authorize](images/authorize_cc.png)

Once authorized on your GitHub account, Code Capsules will be able to access the repository and deploy the code. Now we just need to tell Code Capsules to deploy the HTML code, by creating a [Capsule](https://codecapsules.io/docs/faq/what-is-a-capsule).

## Create the Capsule

Navigate back to your Code Capsules account. Find your personal team, then click on the personal Space in the centre of the screen. 

![space_image](images/space.png)

To deploy the HTML website, click the "Create a New Capsule for Your Space" button. Then:

1. Choose a "Frontend" Capsule.
2. Select the "Trial - Static Site Hosting" product.
3. Choose the GitHub repository we forked.
4. Press next.
5. Leave the "Build command" and "Static content folder path" blank and create the Capsule.

While the Capsule is building, you can view its logs under the "Logs" tab in the Capsule. Once built, you can navigate to the "Overview" tab and click on the provided URL to view the application. For a closer look at capsules, take a look at this explanation of [Capsules](https://codecapsules.io/docs/faq/what-is-a-capsule).