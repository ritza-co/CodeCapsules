# Hosting a frontend: building and deploying your portfolio to Code Capsules

In the past, deploying a website to a server was a time-consuming and error-prone process. It required a large amount of knowledge about setting up and maintaining operating systems, as well as webservers, file transfer programs, and much else. And all this came after you'd already learnt how to put together your website with HTML and CSS!

[Code Capsules](https://codecapsules.io/) allows developers to cut through the admin of website deployment and server maintenance and focus on what they're really interested in: developing great websites and applications. With Code Capsules, deploying a frontend website is as simple as uploading code to GitHub. And if that still sounds intimidating, don't worry, we'll show you how to do both in this tutorial, all in the process of creating a professional portfolio website.

First, we'll take a look at choosing a portfolio template and personalising it. Then we'll push the portfolio to a GitHub repository and see how to connect to Code Capsules and make it public. [Here](https://abesportfoliowzfg.codecapsules.space/#) is a sneak preview of what it will look like.

## Requirements & prerequisite knowledge

Hosting a portfolio on Code Capsules requires no previous knowledge about web development or how to operating a server. To personalise a portfolio template and deploy it to Code Capsules, we'll need:

- A text editor, such as [Sublime Text](https://www.sublimetext.com/), or [VSCode](https://code.visualstudio.com/). 
- The version control software [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). 
- A free user account on [GitHub](https://www.github.com).

You can install the above software and complete this tutorial on Windows, Mac, or Linux. 

## Creating a portfolio

[HTML5 UP](https://www.html5up.net) has a large array of beautiful HTML5 site templates that you can use free of charge, [as long as you visibly credit them for the design](https://html5up.net/license). Most of their templates already a visible design credit, so we'll be fine as long as we don't remove it. In this tutorial, we'll work with the [Massively](https://html5up.net/massively) template. Once you've got the hang of editing HTML through working with this template, feel free to [pick any other template](https://www.html5up.net) that catches your eye.

Download the Massively template and extract it to a folder on your computer. Then create a second folder inside that folder and move the template files into it (you can leave the LICENSE and README files behind). This step is necessary for hosting a web-page on Code Capsules. 

You can name the two folders whatever you want. The file structure should look something like this:

```
html5up-massively
    myPortfolio
        + assets
            + css
            + js
            + sass
            + webfonts
        + images
        + generic.html
        + elements.html
        + index.html
```

Open the `index.html` file in your browser now and scroll through it to get an idea of what it looks like. In this tutorial, we're only going to be editing this file, but feel free to look through the rest and see what happens to the site when you change them.

### Personalising the template

We've got a nice-looking website without any content. Now it's time to start putting our personal stamp on it. To illustrate the process, we'll work through creating a portfolio for Abraham Lincoln, the 16th president of the USA. You're free to follow along with the example, or use your own name and details, or even make a portfolio for a totally different president.

Let's start with the title and subheading of the portfolio. Open the `index.html` file in your text editor. You should see the following block of HTML near the top of the file. 

```html
<head>
    <title>Massively by HTML5 UP</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
    <link rel="stylesheet" href="assets/css/main.css" />
    <noscript><link rel="stylesheet" href="assets/css/noscript.css" /></noscript>
</head>
```

The text inside the `<title>` tags is what shows up in the page's browser tab and search engine results, so let's change that first.

```html
<head>
    <title>Abraham Lincoln</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
    <link rel="stylesheet" href="assets/css/main.css" />
    <noscript><link rel="stylesheet" href="assets/css/noscript.css" /></noscript>
</head>
```

If you kept the `index.html` file in your browser, you can refresh the page now, and you'll see the new tab title.

Now we're going to change the text at the top of the page itself. Scroll down a bit in your text editor, until you see the below text.

```html
<!-- Intro -->
    <div id="intro">
        <h1>This is<br />
        Massively</h1>
        <p>A free, fully responsive HTML5 + CSS3 site template designed by <a href="https://twitter.com/ajlkn">@ajlkn</a> for <a href="https://html5up.net">HTML5 UP</a><br />
        and released for free under the <a href="https://html5up.net/license">Creative Commons license</a>.</p>
        <ul class="actions">
            <li><a href="#header" class="button icon solid solo fa-arrow-down scrolly">Continue</a></li>
        </ul>
    </div>

<!-- Header -->
    <header id="header">
        <a href="index.html" class="logo">Massively</a>
    </header>
```

Change the text inside the `<h1>` and `<p>` tags to something else (don't worry about the `<br />` tag, it just splits the text across lines, and you can safely delete it). Then delete the `<header>` block. Once you've done that, your portfolio should look something like this: 

![image2](images/Image2.png)

### Removing unnecessary content

The default layout for Massively is designed for a blog or news website containing articles. To make it look more like a portfolio, we're going to delete the dates above each article entry. To do this, we will find and delete all lines that begin with `<span class="date"...>`.

Next, because this is going to be a single-page portfolio, we can also delete the theme's extra tabs. Find and delete these lines:

```html
<li class="active"><a href="index.html">This is Massively</a></li>
<li><a href="generic.html">Generic Page</a></li>
<li><a href="elements.html">Elements Reference</a></li>
```

Depending on who you're making a portfolio for, you may want to have a different set of social media links, or possibly none at all. In this template, social media links are shown at the top and bottom of the page, so we'll need to change them in two places.

First, find this block of HTML near the top of the page:

```html
<ul class="icons">
    <li><a href="#" class="icon brands fa-twitter"><span class="label">Twitter</span></a></li>
    <li><a href="#" class="icon brands fa-facebook-f"><span class="label">Facebook</span></a></li>
    <li><a href="#" class="icon brands fa-instagram"><span class="label">Instagram</span></a></li>
    <li><a href="#" class="icon brands fa-github"><span class="label">GitHub</span></a></li>
</ul>
```

For each social media profile you want to link to, enter its link in place of the `#` in `href="#"`. For example, to link Abraham Lincoln's Twitter account, you can do this:

```html
<li><a href="https://twitter.com/Abe_Lincoln" class="icon brands fa-twitter"><span class="label">Twitter</span></a></li>
```

Delete any lines you don't want to link to. If you don't like social media, you can delete all of them. Then find the second set of social media links at the bottom of the page and repeat the process. They look similar to the ones at the top of the page, so you should be able to locate them quite easily.

### Custom images and button linking

Now we're going to replace the placeholder images with our own pictures. If you're making a portfolio for yourself, this is where you might want to include pictures of different projects you've completed. If you want to use images from the web, make sure you understand their licences and provide credit where necessary. 

Once you've chosen your pictures, navigate to your project's `images` folder and place your image there. Remember each image's name and file extension.

In HTML, images are displayed using the `<img>` tag. Find those tags in `index.html`. To start you off, the big image at the top of the page is specified by this line:

```html
<img src="images/pic01.jpg" alt="">
```
The image file to display is controlled by the `src` attribute. Change the one above from `images/pic01.jpg` to `images/YOUR-IMAGE.FILE-EXTENSION`. Do this for all the images on the page. If you don't have enough images to replace everything, you can delete the image code or even the whole content block.

Now that the images are in, you can change the article headings (in `<h2>` tags) text (in `<p>` tags) and links (in `<a>` tags). For example, we can change this:

```html
<article>
    <header>
        <span class="date">April 24, 2017</span>
        <h2><a href="#">Sed magna<br />
        ipsum faucibus</a></h2>
    </header>
    <a href="#" class="image fit"><img src="images/pic02.jpg" alt="" /></a>
    <p>Donec eget ex magna. Interdum et malesuada fames ac ante ipsum primis in faucibus. Pellentesque venenatis dolor imperdiet dolor mattis sagittis magna etiam.</p>
    <ul class="actions special">
        <li><a href="#" class="button">Full Story</a></li>
    </ul>
</article>
```

To this:

```html
<article>
    <header>
        <h2><a href="#">The Gettysburg<br />
        address</a></h2>
    </header>
    <a href="#" class="image fit"><img src="images/gettysburg.jpg" alt="" /></a>
    <p>One of my greatest qualities is my ability to deliver compelling speeches.</p>
    <ul class="actions special">
    <li><a href="https://en.wikipedia.org/wiki/Gettysburg_Address" class="button">Read More</a></li>
    /ul>
</article>
```

Change as many articles as you have content for and delete the rest.

### Cleaning up

Now that most of our content is in place, to complete the portfolio we'll need to do the following:

- Change the **Full Story** button's text.
- Remove the contact form.
- Update or remove the contact information. You probably don't want to put your address or phone number online.
- Delete the pagination buttons at the bottom of the page.

    ![](images/pagination.png)

Now that you've looked around the template and edited quite a lot of it, you should be able to find and change all of the above on your own. Once you're happy, the next step is to push it to GitHub. After that, the portfolio can be deployed on Code Capsules.

## Uploading to GitHub

First, we'll use Git to create a local repository in our code folder. Then we'll push that code to a remote repository on GitHub. Finally, we will connect the GitHub repository our Code Capsules account so that Code Capsules can automatically detect and deploy changes.

### Creating the remote repository

First, log in into [Github.com](https://github.com) and click on **Create new repository**. Name your repository anything you want (we've called it "myPortfolio" in the image below).

![image10](images/image10.png)

Then open a terminal in your portfolio's top-level folder and create a repository by running the command below.

```
git init
```

Add all the files in your folder to the repository with this command:

```
git add .
```

Create a [commit](https://www.git-tower.com/learn/git/commands/git-commit/) with the command below.

```
git commit -m "First commit!"
```

Making a commit tells repository to save the files you've added as they currently are. If we make a mistake in our code later on, Git will allow us to come back to the state of this or any other commit. This ability to create incremental backups is one of the reasons developers love version control.

Branches are a related Git feature, which provide a way to maintaining different versions of the same codebase simultaneously, which is useful when more than one person has to be able to work on a codebase at the same time. As this is a solo project, we don't need to worry about them, except to make one change. By default, Git calls its main [branch](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) `master`, but the community is making an effort to move towards the more neutral name `main`. We can rename the `master` branch to `main` with the following command: 

```
git branch -M main
```

Now we need to tell our local repository about our remote repository on GitHub, so we can upload our files there. Run this command, substituting in your GitHub username and the name of your repository:

```
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY-NAME.git  
```

Finally, we're going to push our code up to GitHub. For this first push, we need to tell our local repository that its `main` branch has an [upstream counterpart](https://www.git-tower.com/learn/git/faq/set-upstream/) on our repository, like this:

```
git push --set-upstream origin main
```

Now you should see the portfolio code in your GitHub repository. It should look similar to this, with all of your portfolio files in a sub-directory.

![image11](images/image11.png)

If you want to make further changes to your portfolio before you publish it on Code Capsules, you can add, commit and upload with the following commands (note that we don't need to set the upstream branch a second time):

```
git add index.html
git commit -m "Update portfolio"
git push
```

Once you're happy with your portfolio, proceed to the next section to learn how to publish it on Code Capsules.

## Deploying to Code Capsules 

First, go to https://codecapsules.io/, create an account and log in. 

Follow the prompts to create a team. Code Capsules provide teams for collaborative development -- you can invite other people to view and edit your web applications by adding them to your team. If you're working alone, you'll have a team of one.

![image12](images/image12.png)

After creating a team, you will be prompted for billing information. Code Capsules requires billing information to create Capsules, but will **not** charge you for hosting the portfolio. You will also be prompted to add team members, but you can skip that for now.

### Linking the repository

Code Capsules requires permission to connect to our portfolio on GitHub. Click **Get Started By Installing The GitHub App** and Code Capsules will redirect you to GitHub.

![image13](images/image13.png)

On GitHub: 

1. Click on your username.
2. Choose **Only select repositories**.
3. Find your portfolio repository in the drop-down and select it.
4. Click **Install & Authorize**.
    
    ![permissions_git](images/permissions_git.png)

Now your Code Capsules account is connected to your GitHub account, and you can deploy your portfolio. 

### Creating a Space, Capsule, and viewing the portfolio

Next, we need to create a *space*. A space contains *capsules* -- more on those soon. Click **Create a New Space For Your Apps** and follow the prompts. After creating a Space, click on it.

Now we can create a capsule. Capsules contain code -- this can be frontend or backend data, or a database. Click **Create A New Capsule For Your Space**.

You'll be prompted to choose a capsule type. Our portfolio only contains frontend code, so choose the frontend type.

1. Select the "Trial" product type.
2. Choose the repository that contains the portfolio and click **Next**.
3. Leave the build command blank. Enter the name of the subfolder containing your portfolio files in the **Static Content Folder Path** entry box. This is why we needed a subfolder!

  ![image16](images/image16.png)

4. Click **Create Capsule**. 

Your Capsule is now building. Once this process is finished, your portfolio will be visible online. 

![image15](images/image15.png)

## Conclusion

To see your portfolio, switch to the **Overview** tab and click the link under **Domains**.

![finished_port](images/finished_port.png)

We've created a portfolio, pushed it to GitHub, and deployed it on Code Capsules. It's now public, and you can share it with your friends. 

In the future, we'll take a look at **backend** capsules. These capsules will enable us to host backend code that will enable visitors to interact with our websites. For example, we might want to implement a contact form or a guestbook. 