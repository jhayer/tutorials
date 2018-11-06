# Git in RStudio

## Version control in Rstudio with Git and GitHub

*This tutorial uses some parts of [Happy Git and GitHub for the useR](http://happygitwithr.com) by Jennifer Bryan.*

## Step 1: Create a GitHub account

Before you register, I would like to give you some tips.

For most of the settings, joining organizations, etc. you will be able to deal with it in the future, so you do not need to think about all this now. **Except your username. You might want to give that some thought.**

Some advices about the username:

- Incorporate your actual name! People like to know who they’re dealing with. Also makes your username easier for people to guess or remember.

- Pick a username you will be comfortable revealing to your future boss.

- Shorter is better than longer.

- Make it timeless. Don’t highlight your current university, employer, or place of residence.

- Reuse your username from other contexts, e.g., Twitter or Slack. But, of course, someone with no GitHub activity will probably be squatting on that.


That being said, you can now register an account with GitHub. It’s free!

[https://github.com](https://github.com)

**Note**: *you do not need to use you SLU email address to register, you might want to use a personal one that you will keep using even if you move to another workplace.*

## Step 2: Install Git on your computer

### For Windows users
Install Git for Windows, also known as “Git Bash”, to get Git in addition to some other useful tools, such as the Bash shell. Yes, all those names are totally confusing, but you might encounter them elsewhere and I want you to be well-informed.

We like this because Git for Windows leaves the Git executable in a conventional location, which will help you and other programs, e.g. RStudio, find it and use it. This also supports a transition to more expert use, because the “Git Bash” shell will be useful as you venture outside of R/RStudio.

**Note**: Select “Use Git from the Windows Command Prompt” during installation. Otherwise, we believe it’s OK to accept the defaults.
**Note**: RStudio for Windows prefers for Git to be installed below C:/Program Files, for example the Git executable on my Windows system is found at C:/Program Files/Git/bin/git.exe. Unless you have specific reasons to otherwise, follow this convention.

### For MacOS and Linux users

#### Check if git is already installed

Maybe `git` is already installed?
Go to the shell terminal and Enter which git to request the path to your Git executable:
```
which git
## /usr/bin/git
```
and `git --version` to see its version:
```
git --version
## git version 2.15.1
```
If you are successful, that’s great! You have Git already. No need to install! Move on to step 3.

If, instead, you see something more like `git: command not found`, keep reading.

Mac OS users might get an immediate offer to install command line developer tools. Yes, you should accept! Click “Install” and read more below.


#### Install Git on MacOS

Please follow [this link](http://happygitwithr.com/install-git.html#mac-os), read paragraph 7.3 and choose one of the 3 first options.
Once git is installed, you can move to Step 3.


#### Install Git on Linux

Install Git via your distro’s package manager.
Ubuntu or Debian Linux:
```
sudo apt-get install git
```
Fedora or RedHat Linux:
```
sudo yum install git
```
A comprehensive list for various Linux and Unix package managers: [https://git-scm.com/download/linux](https://git-scm.com/download/linux)

## Step 3: git configuration

In the shell (terminal or Git for Windows terminal), type:
```
git config --global user.name 'Firstname Lastname'
git config --global user.email 'my_email@slu.se'
```
substituting your name and **the email associated with your GitHub account.**

These 2 commands return nothing. You can check that Git understood what you typed by looking at the output of `git config --global --list`.


**Note**: *What user name should you give to Git? This does not have to be your GitHub username, although it can be. Another good option is your actual first name and last name. Your commits will be labelled with this name, so this should be informative to potential collaborators.*

*What email should you give to Git? This must be the email associated with your GitHub account.*




## Step 4: make a new repository on GitHub

Go to [https://github.com](https://github.com) and make sure you are logged in.

Click green “New repository” button. Or, if you are on your own profile page, click on “Repositories”, then click the green “New” button.

Repository name: rstudio_git_test (or whatever you wish, we will delete this)
Public
YES Initialize this repository with a README

Click big green button “Create repository.”

Copy the HTTPS clone URL to your clipboard via the green “Clone or Download” button. Or copy the SSH URL if you chose to set up SSH keys.

## Step 5: Clone the new GitHub repository to your computer via RStudio

In RStudio, start a new Project:

*File > New Project > Version Control > Git*. In the “repository URL” paste the URL of your new GitHub repository. It will be something like this `https://github.com/jhayer/rstudio_git_test.git`

>Take charge of – or at least notice! – the local directory for the Project (under "Create project as subdirectory of" when you clone the Git repository). A common rookie mistake is to have no idea where you are saving files or what your working directory is. Pay attention. Be intentional. Personally, I would do this in `~/Documents/courses/reproducible_research/day4/`


I suggest you check “Open in new session”, as that’s what you’ll usually do in real life.
Click “Create Project”.
This should download the README.md file that we created on GitHub in the previous step. Look in RStudio’s file browser pane for the README.md file.

**Note**: *If you do you NOT see an option to get the Project from Version Control? Have a look [here](http://happygitwithr.com/rstudio-see-git.html).*

## Step 6: We are ready to start creating scripts and track them with version control!

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

## Step 7: Practice!

You will now add some code to your script, and then commit the changes and push to the remote repository
