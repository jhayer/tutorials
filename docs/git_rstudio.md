# Git in RStudio

## Version control in Rstudio with Git and GitHub

This tutorial uses some parts of [Happy Git and GitHub for the useR](http://happygitwithr.com) by Jennifer Bryan

## Step 1: Install Git on your computer

### For Windows users
Install Git for Windows, also known as “Git Bash”, to get Git in addition to some other useful tools, such as the Bash shell. Yes, all those names are totally confusing, but you might encounter them elsewhere and I want you to be well-informed.

We like this because Git for Windows leaves the Git executable in a conventional location, which will help you and other programs, e.g. RStudio, find it and use it. This also supports a transition to more expert use, because the “Git Bash” shell will be useful as you venture outside of R/RStudio.

**Note**: Select “Use Git from the Windows Command Prompt” during installation. Otherwise, we believe it’s OK to accept the defaults.
Note that RStudio for Windows prefers for Git to be installed below C:/Program Files, for example the Git executable on my Windows system is found at C:/Program Files/Git/bin/git.exe. Unless you have specific reasons to otherwise, follow this convention.

### For MacOS users

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

*File > New Project > Version Control > Git*. In the “repository URL” paste the URL of your new GitHub repository. It will be something like this *https://github.com/jhayer/rstudio_git_test.git*

If you do you NOT see an option to get the Project from Version Control? Have a look [here](http://happygitwithr.com/rstudio-see-git.html).

Take charge of – or at least notice! – the local directory for the Project. A common rookie mistake is to have no idea where you are saving files or what your working directory is. Pay attention. Be intentional. Personally, I would do this in `~/Documents/courses/reproducible_research/day4/`


I suggest you check “Open in new session”, as that’s what you’ll usually do in real life.
Click “Create Project”.
This should download the README.md file that we created on GitHub in the previous step. Look in RStudio’s file browser pane for the README.md file.

## Step 5: We are ready to start creating scripts and version control them!

In the beginning we will prepare our working environment, and then we will create a Rscript for plotting data that we will modify step by step and use version control follow this modifications.

### Preparing our working directory:

Let's get organised! Within your project folder, you will create 3 other folders (you can do it directly from RStudio): *data, results, scripts*

Download the data file `surveys_complete.csv` from here:
[https://osf.io/9zkwm/download]().
Then place it in the `data/` subfolder of your R project

### Creating the script

*From here, the tutorial is based on a [R tutorial for data visualisation](https://datacarpentry.org/R-ecology-lesson/04-visualization-ggplot2.html) from Data Carpentry.*

You will now create a new Rscript `plot_surveys.R` in the `scripts/` subfolder.
Then add the first commands to the script:

```r
# Install the tidyverse if not already installed
if (!requireNamespace("tidyverse"))
  install.packages("tidyverse")
# loading library
library("tidyverse")
# loading datafile
surveys_complete <- read_csv("data/surveys_complete.csv")

# Assign plot to a variable
surveys_plot <- ggplot(data = surveys_complete,
                       mapping = aes(x = weight, y = hindfoot_length))
```

Test the script. Does it seem to work?

### Add files and commit

We will now use Git to add our new files to the staging area and commit!
You can find the Git tab in the up right panel of RStudio.


When you click on commit, you get a new window that will allow you to select the files and folders that you want to add to the staging area (you might want to commit everything except the folders `results/` and `data/`)

You are now ready for your initial commit!! Do not forget to write a commit message.

### Push to the remote repository
Now you can push to GitHub, and then check online!

## Step 6: Practice!

You will now add some code to your script, and then commit the changes and push to the remote repository
