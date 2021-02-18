
# Getting started on Code Capsules with React
Deploy a React application and learn how to host backend code on Code Capsules. 

## Set up

Here we'll look at a step-by-step process for deploying a React application on Code Capsules. To do so, you'll need a [GitHub](www.github.com) account. Code Capsules uses GitHub repositories to deploy code - you can link any of your GitHub repositories to Code Capsules, and Code Capsules will deploy it.

We'll use a pre-made React application provided by Code Capsules – find the repository here: https://github.com/codecapsules-io/demo-react. To be able to deploy this repository to Code Capsules, make sure to fork the React application. You can do so by visiting the repository and clicking the "Fork" button at the top right of the repository. 

After forking the application, we can deploy the application. Make any edits you'd like to the repository and continue.

## Creating an Account with Code Capsules

First, let's create an account with [Code Capsules](https://codecapsules.io). Make sure to confirm your Code Capsules account by checking your email. After account creation, Code Capsules will redirect you here:

![login_screen](images/login.png)

This is your default Code Capsules [Team](https://codecapsules.io/docs/faq/what-is-a-team/) (note the "Team Personal" at the top left). The centre box labelled "Personal" is the default personal [Space](https://codecapsules.io/docs/faq/what-is-a-space/). 

Code Capsules provides every new user with a default "Personal" Team and Space. The default personal Team allows users to host static frontend websites for free.

Since we're deploying backend code, we'll need to add payment information. Add payment information by navigating to "Team Settings" at the top of the screen, then add a payment method under "Payment Methods".

After adding payment information, we just need to give Code Capsules access to the React application we forked.

## Linking to GitHub

So we can host our React application on Code Capsules, we need to link our forked GitHub repository to our Code Capsules account.

We can link the React application by clicking the profile image at the top right of the screen, and finding the GitHub button under "GitHub Details"

![git-button](images/git-button.png)

Click the GitHub button. To give Code Capsules access to the React application:

1. Click your GitHub username.
2. Select "Only Select Repositories".
3. Choose the GitHub repository we forked.
4. Press "Install & Authorize".

![install&authorize](images/authorize_cc.png)

After authorizing, Code Capsules will be able to deploy the React application. All that's left to deploy the application is to create a [Capsule](https://codecapsules.io/docs/faq/what-is-a-capsule). This Capsules will act as a storage space for the React application.

## Create the Capsule

Return to the personal Team. Next, click on the personal Space at the centre of the screen.

![space_image](images/space.png)

To deploy the React application, click the button "Create a New Capsule for Your Space". Next:

1. Choose a "Backend Capsule".
2. Select the "Sandbox" product.
3. Choose the GitHub repository we forked.
4. Press next.
5. Leave the "Run Command" blank and create the Capsule.

After creating the Capsule, the Capsule will build the React application. You can view the build logs by clicking the "Logs" tab in the Capsule. Once built, navigate to the "Overview" tab. Code Capsules provides a default URL for viewing applications - find this under "domains". Click the URL to view the application.

If you'd like to deploy another React application in the future, take a look at the [React repository](https://github.com/codecapsules-io/demo-react). Code Capsules was able to deploy the application by reading the `package.json` file. You can find the script that Code Capsules used to run the application on line 16 of the `package.json` file.