
# How to Deploy a React Application on Code Capsules

If you've built a web app with React, you probably want to deploy it to production to share it with the rest of the world. In this guide, we'll show you how to do that, step by step.

You'll need 

* Your own [GitHub](https://github.com) account
* An account on [Code Capsules](https://codecapsules.io)

## Getting some example code

We'll use [our example React application](https://github.com/codecapsules-io/demo-react) for the demonstration, but you can use your own React application instead, or work from ours as a starting point.

Fork the project to your own account by clicking on the above link and pressing "fork" in the top right.

![CleanShot 2021-03-22 at 16 32 29@2x](https://user-images.githubusercontent.com/2641205/112015606-42c59900-8b2c-11eb-8ab4-bbabd0de6550.png)

Log into your Code Capsules account and press the "New Space" button.

<img width="1083" alt="CleanShot 2021-03-22 at 16 36 02@2x" src="https://user-images.githubusercontent.com/2641205/112016130-b8316980-8b2c-11eb-8141-629f1658ddca.png">

Now choose a region and name for your space and press "Create Space".

![CleanShot 2021-03-22 at 16 34 40@2x](https://user-images.githubusercontent.com/2641205/112016054-a51e9980-8b2c-11eb-9d55-a13102c084f5.png)

Once the space has created, click on it and create a new capsule inside that space.

<img width="999" alt="CleanShot 2021-03-22 at 16 37 23@2x" src="https://user-images.githubusercontent.com/2641205/112016415-f9c21480-8b2c-11eb-9cc2-16872907a990.png">

Choose to create a front-end capsule.

![CleanShot 2021-03-22 at 16 38 33@2x](https://user-images.githubusercontent.com/2641205/112016526-15c5b600-8b2d-11eb-8ca1-8c616293ffa6.png)






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
