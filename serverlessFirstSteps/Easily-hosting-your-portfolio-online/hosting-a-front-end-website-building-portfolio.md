# Hosting a front-end website: building and hosting your professional portfolio

> It's not really a web *application* - just a front end. I think our audience isn't going to have much experience messing around with servers so this might not be a selling point. Rather focus on a) They can build it locally and then make it public with one command; They can come back in five years time and update it with a single command.

Deploying a web application to a production environment is complex. Traditionally you set up a server and micromanage the tasks associated with hosting one: choosing the servers' operating system, editing its configurations, and monitoring the server to ensure it is running.

> 'no longer necessary' - this is also a bit salesy and PaaS has been around for a while so we don't want to push CodeCapsules as groundbreaking. 'lesson' - try to keep these vaguer as this will be used in various formats: articles, book chapters, blog posts, etc. Use the 'author + reader are a team' voice more, so "Here, we'll build out our portfolio and host it on..." or similar.

This time-consuming process is no longer necessary. In this lesson, you'll learn how to create a portfolio and host it on [CodeCapsules](https://codecapsules.io/), a _Platform as a Service_, which:

> again - not a web application in this context. The main points that will appeal to audience are a) Integrates with GitHub so you can deploy with a `git push` and b) manages uptime etc for you - you don't need to babysit your server.

- Manages servers hosting your web-applications
- Integrates with GitHub to deploy your web-applications

The first half of this guide covers choosing and personalizing a portfolio template. After, we will push it to a GitHub repository and see how CodeCapsules interfaces with GitHub to make your portfolio public.

## Requirements & Prerequisite knowledge

Hosting your portfolio on CodeCapsules requires no previous knowledge about servers. To personalize and deploy your portfolio to CodeCapsules, you will need:

- A text editor, such as [Sublime Text](https://www.sublimetext.com/), or [VSCode](https://code.visualstudio.com/). 
- A [GitHub](https://www.github.com) account and [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed. 

> I think we should assume that they have some coding knowledge already - we're not teaching HTML etc and they will need to know that to find this at all useful.

It will also be helpful (though not necessary) to know some HTML, so you can easily edit a portfolio template we will use.

## Creating a portfolio

[HTML5 UP](https/www.html5up.net) provides HTML5 site templates for free. We will use [Massively](https://html5up.net/massively) template. The Massively template is elegant and is easy to edit.

Download the Massively template and extract it to a directory on your computer. **Within that directory, create _another_ directory, and transfer the template files into it**. This step is necessary for hosting a web-page on CodeCapsules.

The names of both directories are irrelevant. The file structure should look something like this:

```
MyPortfolio  
    PortfolioFiles 
        + assets
        + images
        + generic.html
        + elements.html
        + index.html
```

In our example, we'll only edit the `index.html` file. The `index.html` file contains all of the HTML code for the main page of our template. 

### Personalizing the template

As an example, let's create a portfolio for Abraham Lincoln - the 16th president of the U.S.A. Looking at the template, you may have noticed some things we don't need for a portfolio. Things Abraham Lincoln (and probably you) won't need include:

- Social Media Accounts
- Extra tabs
- Date Entries 
- Irrelevant text
- Pagination
- Email contact form
- Your address

Let's start with the title and subheading of the portfolio. To start editing, open the `index.html` file with your text editor. 

1. At the top of the file, change the text located within the `<title>` tags to whatever you like, e.g. "Abraham Lincoln". This is what will appear in search engines and in browser tabs.

2. Customize the words wrapped in the `<h1>` tags.

3. Below the previously edited `<h1>` tags, edit the text within the `<p>` tags.  

4. Scroll down until you see text that looks like: `<!-- Header -->`, delete all text from `<header id="header">` down to and including the `</header>` tag.

Save the file and open it in your web browser. Your portfolio should now look something like this: 

![image2](images/Image2.png)

Next, we need to delete the extra tabs, social media links, and dates above each portfolio piece. 

### Removing the tabs, dates, and links

To delete the unnecessary tabs, find these lines:

```html
<li class="active"><a href="index.html">This is Massively</a></li>
<li><a href="generic.html">Generic Page</a></li>
<li><a href="elements.html">Elements Reference</a></li>
```

Personalize the "This is Massively" text, and delete the two lines below containing the "Generic Page" and "Elements Reference" text.

The code for social media accounts is located at the top and bottom of the index.html file. Starting at the top, find and delete:

```html
<ul class="icons">
    <li><a href="#" class="icon brands fa-twitter"><span class="label">Twitter</span></a></li>
    <li><a href="#" class="icon brands fa-facebook-f"><span class="label">Facebook</span></a></li>
    <li><a href="#" class="icon brands fa-instagram"><span class="label">Instagram</span></a></li>
    <li><a href="#" class="icon brands fa-github"><span class="label">GitHub</span></a></li>
</ul>
```

At the bottom, find and delete this code-block:

```html
<section>
  <h3>Social</h3>
    <ul class="icons alt">
        <li><a href="#" class="icon brands alt fa-twitter"><span class="label">Twitter</span></a></li>
        <li><a href="#" class="icon brands alt fa-facebook-f"><span class="label">Facebook</span></a></li>
        <li><a href="#" class="icon brands alt fa-instagram"><span class="label">Instagram</span></a></li>
        <li><a href="#" class="icon brands alt fa-github"><span class="label">GitHub</span></a></li>
    </ul>
</section>
```

Finally, find and delete all lines that begin with `<span class="date"...>`. This will delete all the date entries in your portfolio.

### Personalizing text and removing other extranious components

To complete the portfolio we'll

- Personalize default text
- Change the "Full Story" button's text
- Replace images with personal images
- Remove the contact form
- Update or remove the contact information
- Delete the "next" button

> "text with 'Full Story' to edit" - this feels like it has something missing? I'm not sure what it means.
1. Find and change all text with "Full Story" to edit the buttons of the same name. 
 
2. Remove the email form feature by deleting the following code:

```html
<section>
    <form method="post" action="#">
        <div class="fields">
            <div class="field">
                <label for="name">Name</label>
                <input type="text" name="name" id="name" />
            </div>
            <div class="field">
                <label for="email">Email</label>
                <input type="text" name="email" id="email" />
            </div>
            <div class="field">
                <label for="message">Message</label>
                <textarea name="message" id="message" rows="3"></textarea>
            </div>
        </div>
        <ul class="actions">
            <li><input type="submit" value="Send Message" /></li>
        </ul>
    </form>
 </section>
```

3. Remove pagination by deleting the following code:

```html
<footer>
    <div class="pagination">
        <!--<a href="#" class="previous">Prev</a>-->
        <a href="#" class="page active">1</a>
        <a href="#" class="page">2</a>
        <a href="#" class="page">3</a>
        <span class="extra">&hellip;</span>
        <a href="#" class="page">8</a>
        <a href="#" class="page">9</a>
        <a href="#" class="page">10</a>
        <a href="#" class="next">Next</a>
    </div>
</footer>
```

> Rather don't give people options as it adds friction (and they are likely going to be not following your instructions exactly anyway as they customize it for their own needs). I would just show how to remove some sections - it's a portfolio so they will want at least basic contact info.

For contact information, we have two options:

- To delete all contact information, navigate to the bottom of the file and delete the `<section class="split contact">` tag and all lines below it, up to the `</footer>` tag. 

- To delete specific sections (an example section is shown below), delete the `<section>` tag, the information you want to delete, and the corresponding `</section>` tag.

```html
<section class="alt">
    <h3>Address</h3>
    <p>1234 Somewhere Road #87257<br />
    Nashville, TN 00000-0000</p>
</section>
```

### Custom images and button linking

The portfolio is almost complete - you just need to customize the images in your portfolio and link the buttons to your work. First, let's customize the images: 

1. Find the images directory in the directory where the `index.html` file is located.
2. Place your image in the images directory - remember the images' name and file extension.
3. Find lines with `class="image main"` or `class="image fit"` - to the right, you will see `src="images/pichere.jpg"` - change the name of the image with your image name and file extension.

Link buttons to your work by locating `<article>` tags. Below each `<article>` tag, you will see a line that contains `class="button"`. On that line, find `href="#"` and replace the "#" with your link. 

Ensure you provide a full link, including `https://`. An example section for Abraham Lincoln looks like this:

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

Once we're happy with the portfolio, we need to push it to GitHub so that it can be deployed to CodeCapsules from there.

## Uploading to GitHub

> Most style guides recommend not having empty space between two headings as it looks a bit weird, so add a short sentence here to introduce the subsections.

### Creating the remote repository

> 'interface with' is a bit hard to understand for me. I think 'connect' is simpler - and use active voice to be more specific "We'll connect our CC account with our GH account so that CC can automatically detect and deploy changes"

CodeCapsules will interface with your GitHub repository and use it to create a web-application. Further, every time you update your repository, CodeCapsules will automatically detect the changes made and update your CodeCapsules hosted web-application.

If you already know how to push code from a local repository to a remote repository on GitHub, push the sub-directory containing the portfolio to GitHub and [skip](#deploying-to-codecapsules) to the next section. 

Otherwise, follow these steps:

> It's conventional to use the same name on GitHub as the outer directory name. I think it can be a bit clearer how the local directory structure you set up earlier fits in with GitHub - is the out folder part of the repo? Then use the same name for the Repo.

- Go to www.github.com and log in.
- Find the "Create new repository" button and click it.
- Name your repository anything. We named ours "AbesPortfolio".
- Copy the URL given to you under "Quick setup".

![image10](images/image10.png)
_The link to your repository is located under Quick setup_

### Sending files to the GitHub repository 

The final step before deploying your portfolio to CodeCapsules is to send the files located in your portfolio folder to GitHub. Open your terminal and navigate to the top-level directory you created. This directory should contain the sub-directory that has your template files. Enter each command in order:

```
git init
git add .
git commit -m "First commit!"
git branch -M main
git remote add origin https://github.com/yourusername/yourrepositoryname.git
git push -u origin main
```

> Make it clearer that they copied the whole URL so they only need to replace one thing - not copy the whole URL into each of the placeholders. 

Replace "yourusername" with your GitHub username, and "yourrepositoryname.git" with the URL copied previously. 

Now, you can see your code in your GitHub repository. Your repository should look similar to this, where all of your portfolio code is contained in a sub-directory (mine is portFolder):

![image11](images/image11.png)

We are ready to host our portfolio on CodeCapsules.

## Deploying to CodeCapsules 

To deploy your portfolio to CodeCapsules, navigate to https://codecapsules.io/, and create an account. After creating your account, log in. 

> Instead of 'when you do x, you will see y', rather "do x, and do y" - e.g. "log in and follow the prompt to create a team"

When you log in, you will be prompted to create a team. CodeCapsules provide teams for collaborative development - you can invite other people to view and edit your web-applications by adding them to your team. 

> Even if you are working alone, you'll still need a "team" (which will contain just yourself).

Regardless if you are working with other people, you must create a team. Click the "Create A New Team" button.

![image12](images/image12.png)

> After you create a team, skip the prompt to add ... 
> (Also note that they are still considering making this compulsory - so might have to edit this in future)

After creating a team, you are prompted to enter your team's billing information and invite Team Members. For now, we can skip this.

### Linking the repository

> For CodeCapsules to access our portolio in GitHub, we'll need to give it some permissions ...

For CodeCapsules to interface with your portfolio hosted on GitHub, you must give CodeCapsules access to your repository containing your portfolio. Click "Get Started By Installing The GitHub App"

![image13](images/image13.png)



You are redirected to GitHub to install CodeCapsules.

1. Click your user name.
2. Press "Only select repositories".
3. From the drop-down menu, choose the name of the repository containing your portfolio.
4. Press "Install & Authorize"

> maybe add an image for the step above

With CodeCaspsules connected to your portfolio, you can now deploy it. 

### Creating a Capsule and viewing the portfolio

The final step to deploy your portfolio is to press "Create A New Capsule For Your Space". Within a space, you can have many capsules - these are your web-applications (in our case, a portfolio), that the world can view. Let's create a capsule, and get our portfolio online. 

You will be prompted to choose a capsule type. Choose a "Frontend" capsule and:

1. Select your product type.
2. Click the repository that contains your portfolio.
3. Press "Next".
4. Leave the build command blank and enter the name of the sub-directory containing your portfolio files in the "Static Content Folder Path" entry box.

  ![image16](images/image16.png)

5. Press "Create Capsule". 

Your Capsule is now building. This process will make your portfolio visible online. After it has deployed, click the "Overview" tab, then press the link under "Domains" to view your portfolio! 

![image15](images/image15.png)

## Conclusion

> Add a screenshot of the portfolio and a link either here or at the start.

> No need to sell CodeCapsules here - rather quickly summarize the takeaways (learnt how to set up code capsules with GitHub, create a basic portfolio, and deploy it) and talk about where next (limitations are we don't have a backend which makes things like the contact form we removed more difficult to implement. In future, we'll look at backend capsules which will be able to "do" more things instead of just "present" them.)

By using CodeCapsules to host your web-applications (or portfolio), the hardest part is _creating_ your web-application. CodeCapsules follows a simple process, guiding you from start to finish without having to mess around with any servers. 

In little time, you can have a portfolio ready for the entire world to see - and with CodeCapsules, the moment you update you update your portfolio on GitHub, the changes will be visible to all. 
