<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Dependencies and CodeArtifact

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-code-artifact)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Store Dependencies with CodeArtifact

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-code-artifact_1d79e699)

---

## Introducing today's project!

### What is AWS CodeArtifact?

AWS CodeArtifact acts a private storage locker for you to keep a backup copy of all your web app's dependencies. Even if a dependency goes down or removed or unavailable in the public repository that it comes from, you can still access your backup from CodeArtifact and continue building your app without any hiccups.

You can imagine how CodeArtifact adds a layer of reliability and security, protecting your development process from external disruptions

### How I used CodeArtifact in this project

I created a local repo that will be used to store the packages for the NextWork web app. And connected my local repo to the Mavin central store which is further linked to the bigger Marvin central repo.

### One thing I didn't expect in this project was...

Key highlight for me was connecting my local storage in CodeArtifact to my web app project via VSCode.

Also implementing IAM policies to grant secure access to the CodeArtifact repository, ensuring that only authorized services can interact with your stored dependencies.


### This project took me...

2 hours

---

### My project has three artifact repositories

The local repository is the personal toolbox where you keep all the specific tools and materials for your current project.


The upstream repository is (i.e. maven-central-store): Is the upstream secondary storage level whereby if the needed packages are not in your local repository toolbox, Maven then checks this upstream repository.

The public repository is the biggest and most reliable library for Java packages (Maven Central Repository) helping us get and store new tools when we need them for your toolbox.

It’s the primary source that supplies packages to other repositories, like maven-central-store.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-code-artifact_ef93d32c)

---

## Connecting my project with CodeArtifact

I connected my web app project (via VSCode IDE) to CodeArtifact so that my development environment can fetch and store dependencies from CodeArtifact.

### I created a new file, settings.xml, in my web app

settings.xml is a file that contains instructions that tells Maven where to find the dependencies and how to connect to the right repositories (e.g. the ones in CodeArtifact) to get access.


The snippets of code in the settings.xml file is:

The servers section is where your store your access details to the repositories you're connecting with your web app project. In this example, you've added your authentication token (which is kind of like an access pass) to access the local nextwork-packages repository you've set up in CodeArtifact.

The profiles section is where you write a rulebook on when Maven should use which repository. We only have one package repository in this project, so our profiles section is more straightforward than other projects that might be pulling from multiple repositories! Our profiles section is telling Maven to go to the nextwork-packages repository to find the tools / packages needed to build your Java web app.

The mirrors section sets up backup locations that Maven can check if it can’t find what it needs in the first local repository it goes to. You might notice that the backup location we've set is... nextwork-packages again. This mea

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-code-artifact_c17eace8)

---

## Testing the connection

### To test the connection between Cloud9 and CodeArtifact, I compiled my web app

Compiling means translating your project’s code into a language that computers can understand and run. When you compile your project, you're making sure everything is correctly set up and ready to turn into a working app.

### Success!

After compiling, I checked nextwork-packages repository.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-code-artifact_1d79e699)

---

## Create IAM policies

### The importance of IAM policies

I also created an IAM policy because other DevOps services like AWS CodeBuild, AWS CodePipeline, and other development environments (VSCode) might need access to the dependency packages stored in CodeArtifact - but they don't have access by default. 

### I defined my IAM policy using JSON

This policy will allow other DevOps services can use the packages we've just installed into our CodeArtifact repository.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-code-artifact_9b2ded4f)

---

---
