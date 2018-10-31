# Version control in Rstudio with Git and GitHub


## Step 1: Install Git on your computer


## Step 2: create a GitHub account


## Step 3: make a new repository on GitHub

Go to [https://github.com]() and make sure you are logged in.

Click green “New repository” button. Or, if you are on your own profile page, click on “Repositories”, then click the green “New” button.

Repository name: rstudio_git_test (or whatever you wish, we will delete this)
Public
YES Initialize this repository with a README

Click big green button “Create repository.”

Copy the HTTPS clone URL to your clipboard via the green “Clone or Download” button. Or copy the SSH URL if you chose to set up SSH keys.

## Step 4: Clone the new GitHub repository to your computer via RStudio

In RStudio, start a new Project:

File > New Project > Version Control > Git. In the “repository URL” paste the URL of your new GitHub repository. It will be something like this https://github.com/jhayer/rstudio_git_test.git

If you do you NOT see an option to get the Project from Version Control? Have a look [here](http://happygitwithr.com/rstudio-see-git.html).

Take charge of – or at least notice! – the local directory for the Project. A common rookie mistake is to have no idea where you are saving files or what your working directory is. Pay attention. Be intentional. Personally, I would do this in ~/Documents/courses/reproducible_research/day4/.

I suggest you check “Open in new session”, as that’s what you’ll usually do in real life.
Click “Create Project”.
This should download the README.md file that we created on GitHub in the previous step. Look in RStudio’s file browser pane for the README.md file.

## Step 5: We are ready to start creating scripts and version control them!
