<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Connect a GitHub Repo with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-github)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Connect a GitHub Repo with AWS

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_dd9d254e)

---

## Introducing Today's Project!

### What is GitHub?

GitHub is a place for engineers to store and share their code and projects online. It's called GitHub because it uses Git to manage your projects' version history.

In today's project, i used Github to create a repository on the cloud for my webapps created in my remote virtual server.

### One thing I didn't expect...

Key takeaways for me was that I learned about establishing a connection from AWS EC2 instance to my Github repository and pushing code to it. Also learned about using tokens to access my Github repo instead of a password.

### This project took me...

1.30 hours

---

## Git and GitHub

Git is a version control, like a time machine and filing system for your code. It tracks every change you make, which lets you go back to an earlier version of your work if something breaks.

You can also see who made specific changes and when they were made, which makes teamwork/collaboration a lot easier.... I installed Git using the commands:
sudo dnf update -y
sudo dnf install git -y


GitHub is a place for engineers to store and share their code and projects online. It's called GitHub because it uses Git to manage your projects' version history. 

I'm using GitHub in this project to help me use Git and see my file changes in a more user-friendly way. It's just like how using an IDE (VSCode) makes editing code easier than using the terminal.

GitHub is also especially useful in situation where you're working in teams and need to share your updates and reviews to a shared code base.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_efaadbf7)

---

## My local repository

A Git repository is a folder that contains all your project files and their entire version history. Hosting a repo in the cloud, like on GitHub, means you can also collaborate with other engineers and access your work from anywhere.

git init is a command that creates a local repository on your computer.

I run git init inside a directory e.g. nextwork-web-project, it sets up the directory as a local Git repository which means changes are now tracked for version control.

After running git init, the response from the terminal was yellow text warning about naming my main branch master and suggesting that I can choose a different name like 'main' or 'development' if I want. 

A branch in Git is s parallel version or 'alternate universes' of the same project. For example, if you wanted to test a change to your code, you can set up a new branch that lets you diverge from the original/main version of your code (called master) so you can experiment with new features or test bug fixes safely.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_7bf21bae)

---

## To push local changes to GitHub, I ran three commands

### git add

The first command I ran was:
git remote add origin https://github.com/emmgarrison/nextwork-devops-webapp.git

A staging area is where GIt puts together or Adds all your modified files for a final review before you commit them.

### git commit

The second command I ran was...git add . which stages all (marked by the '.') files in nextwork-web-project to be saved in the next version of your project.

git commit -m "Updated index.jsp with new content" 
saves the staged changes as a snapshot in my project’s history. 
  Using '-m' flag means it lets you leave a message describing what the commit is about, making it easier to review what changed in this version.

### git push

The third command I ran was:

git push -u origin master 
-- It uploads i.e. 'pushes' my committed changes to origin, which I've bookmarked as my GitHub repo. 'master' tells Git that these updates should be pushed to the master branch of my GitHub repo. 

Using -u means I'm also setting an 'upstream' for my local branch, which means I'm telling Git to remember to push to master by default. Next time, I can simply run git push without needing to define origin and master.

---

## Authentication

When I commit changes to GitHub, Git asks for my credentials because Git needs to double check that I have the right to push any changes to the remote origin my local Git repo is connected with.

### Local Git identity

Git needs my name and email because Git needs author information for commits to track who made what change. If you don't set it manually, Git uses the system's default username, which might not accurately represent my identity in my project's version history.

Running git log showed me the author of the commit and the date of the commit.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_9a27ee3b)

---

## GitHub tokens

GitHub authentication failed when I entered my password because GitHub phased out password authentication to connect with repositories over HTTPS - there are too many security risks and passwords can get intercepted over the internet.

Hence I need to use a personal access token instead, which is a more secure method for logging in and interacting with my repos.

A GitHub token is a unique string of characters that looks like a random password. For example, a GitHub token might look like ghp_xHJNmL16GHSZSV88hjP5bQ24PRTg2s3Xk9ll. As you can imagine, tokens are great for security because they're unique to every Github access.

I'm using one in this project because access to Github from Git requires it.

I could set up a GitHub token by navigating to the my profile settings > developer settings > personal access tokens page of my github profile.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_fa11169d)

---

## Making changes again

I wanted to see Git working in action, so I made changes to my web app on my EC2 instance.

I couldn't see the changes in my GitHub repo initially because saving changes in your VSCode environment only updates your local repository. Note: the local repository in VSCode is separate from your GitHub repository in the cloud.

I finally saw the changes in my GitHub repo after, I had to write commands that commit and pushed those changes from your local repository into your origin.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_6becb2bc)

---

## Setting up a READMe file

As a finishing touch to my GitHub repository, I added a README file, which is a document that introduces and explains your project, like what the project does, how to set it up, and how to use it. 

I added a README file by running:
touch README.md
which creates a blank file

My README is written in Markdown because it's a text language that lets you format text that you'll display on a webpage. With Markdown, you can make words bold, create headers, add links, and use bullet points—all with simple symbols added to your text. 

It’s useful for creating documents like README files that need to look clean and easy to read without complex software, making it a favorite for writing on platforms like GitHub.

Special characters can help you format text in Markdown, such as **text** to make text bold

My README file has 6 sections that outline...
Table of Contents
Introduction
Technologies
Setup
Contact
Conclusion

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-github_c94976902)

---

---
