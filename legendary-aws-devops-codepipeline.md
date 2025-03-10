<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# CI/CD with CodePipeline

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## CI/CD with CodePipeline

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codepipeline_da9d9cae)

---

## Introducing Today's Project!

### What is AWS CodePipeline?

AWS CodePipeline is a CI/CD service that automates integrating, testing, building, and deploying code changes. 
It ensures continuous development of software code, code testing, detection/troubleshooting of bugs and continuous deployment of error-free code, new software features and bug fixes automatically to production, very quickly - reducing the risk of human errors, ensuring faster and more reliable software delivery.

### How I used CodePipeline in this project

I used AWS CodePipeline to automate the deployment of my web app. It monitored my GitHub repository for changes, built the app with CodeBuild, and deployed it with CodeDeploy, to my EC2 webserver instance, ensuring continuous integration and delivery with minimal manual/human intervention.

### One thing I didn't expect in this project was...

One thing I didnʼt expect in this project was how easily CodePipeline detects changes in the repository and automatically triggers a deployment, making the
process seamless without needing manual updates.

Another key highlight for me was the rollback feature - The ability to go to different versions of the deployment making it easy to compartmentalize issues, fix bugs and maintain the code.

### This project took me...

This project took me approximately 2 days to complete, including setting up the pipeline, updating the web app, troubleshooting issues, and testing the rollback functionality.

I also used VScode and GitHub thoughout this project series instead of the deprecated Cloud9 IDE and AWS codecommit, therefore I had to work around establishing connections to GitHub instead of AWS codecommit. 

Also spent time working on my documentation too, to elaborate my work clearly to my understanding and the understanding of readers.

---

## CI/CD Pipeline

A CI/CD pipeline is an automated process for integrating, testing, building, and deploying code changes. 

It ensures continuous development of software code, code testing, detection/troubleshooting of bugs and continuous deployment of error-free code, new software features and bug fixes automatically to production, very quickly - reducing the risk of human errors, ensuring faster and more reliable software delivery.

### My CI/CD pipeline has three stages

The source stage is where Codepipeline will find the source code for my application...which is my Github repository.

The build stage is where my app's source code is compiled and transformed into artifacts (i.e. the WAR file for our web app)! I need to tell CodePipeline that I've set up my pipeline's build stage in CodeBuild.

The deploy stage is where the built and tested code is deployed to its target environments, like production or staging servers. This stage makes the application or changes accessible to users. I need to tell CodePipeline that we've set up the deploy stage in CodeDeploy.

---

## CI/CD Pipeline

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codepipeline_da9d9cae)

---

## Releasing a Change

My CI/CD pipeline gets triggered by clicking "Create Pipeline"...and also triggered automatically whenever I push new changes to my repository i.e. when I make a commit in my local developer environment (VSCode) that pushes changes up to my Github repository! This automation means the latest version of my code is tested, built, and deployed without manual intervention.

I tested my pipeline's trigger by making two updates to my web app’s source code. These changes were:
1. Updated the contents of the  index.jsp with a new HTML file's contents.
2. Uploaded a folder of image assets for my webapp into the webapp folder in my VSCode IDE.

These changes initiated the CI/CD process and verified the automatic pipeline execution.

Once my pipeline executed successfully, I checked my webserver IP address to see the updated version of my web app and confirm whether the latest changes were deployed correctly or not.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codepipeline_8ea16579)

---

## Trigger A Rollback

A rollback in a pipeline means going back to an earlier, working version of your application - a super useful tool if the latest changes caused problems!

I initiated a rollback on the Deploy stage.

I checked the source stage, and learnt that the source stage was unaffected by the rollback...because we rolled back only the Deploy stage, meaning my webapp will look like how it did before I updated index.jsp. But, the Source Stage still reflects our latest changes to index.jsp...still untouched....the Build stage also remains unchanged.

Rolling back only the Deploy stage is useful when the latest deployment has errors, but the source code and build processes are correct and we don't want to undo some valid code and build changes.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codepipeline_a554216f)

---

## Reverting a Rollback

Once the rollback was complete, my web app showed the previous state (static version)...reflecting the previous verison of the source code.

To update my Deploy stage back to the latest version of my source code, I clicked on "release change" on my code pipeline interface...which updates all three stages of the CI/CD Pipeline to their latest versions...using the latest source code.

my web app showed the latest updates,
restoring it to the most recent update I made.


![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codepipeline_e2bd6672)

---

---
