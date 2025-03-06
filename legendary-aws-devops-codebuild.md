<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Package an App with CodeBuild

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codebuild)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Package an App with CodeBuild

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codebuild_52365514)

---

## Introducing today's project!

### What is AWS CodeBuild?

AWS CodeBuild helps automate steps like compiling the code, running tests, and packaging the application...
(so you won't need to run manual commands in your Cloud9 terminal to compile your web app anymore) AND it creates the WAR file for your server to run.

### How I used CodeBuild in this project

I used AWS CodeBuild to configure a new build project.

### One thing I didn't expect in this project was...

Key takeaways for me was:

Create a CodeBuild build project: You configured a new build project in AWS CodeBuild. By selecting AWS CodeCommit as the source and configuring the environment with specific settings like the Amazon Linux image and a new service role, you prepared your build system to handle the project's requirements efficiently.

📂 Create your web app's buildspec.yml file: In the Cloud9 IDE, you created a buildspec.yml file in your project repository. This file outlines the commands and phases that CodeBuild will execute during the build, such as installing dependencies, compiling the code, and packaging the output into a WAR file.

💂‍♀️ Modify the IAM role: You enhanced the auto-generated IAM role by attaching the codeartifact-nextwork-consumer-policy. This modification grants CodeBuild the necessary permissions to interact with AWS CodeArtifact and access the packages stored inside.

### This project took me...

1:30 hour

---

## Set up an S3 bucket

I started my project by creating an S3 bucket because CodeBuild will later use it to store a file that gets created in the build process.

The key artifact that this S3 bucket will capture is called the Build artifact.

This artifact is important because it packages up everything a server could need to host our web app in one neat bundle. This bundle is called a WAR file (which stands for Web Application Archive, or Web Application Resource)

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codebuild_df9d97e6)

---

## Set up a CodeBuild project

### Source

My CodeBuild project's Source configuration means a version control repository, such as Github, AWS CodeCommit, where your project's code is maintained and managed and I selected Github.

### Environment

My CodeBuild project's Environment configuration means an environment that includes all the software, tools, settings, and configurations that your application needs to function properly. 

The environment in my VSCode console is designed for writing and editing code, but it's not exactly fit for compiling/packaging your web app. That's why we're setting up a build environment dedicated to compiling, testing and then packaging your code!

... and I selected:
Provisioning model: On-demand
Environment image: Managed image
Compute: EC2
Operating System: Amazon Linux
Runtime: Standard
Image: aws/codebuild/amazonlinux2-x86\_64-standard:coretto8
Image version: Always use the latest image for this runtime version
Service role: New service role

### Artifacts

My CodeBuild project's Artifacts are the files that CodeBuild will produce while building your project. So in this panel, we're telling CodeBuild to package all the artifacts it creates into a WAR file, .... and I selected or named that WAR file nextwork-web-build.zip and then store it in the S3 bucket we created.

### Logs

My CodeBuild project's Logs configuration means collect and store log files from AWS services in your account. These log files are like diaries where everything that happens when the service is running gets recorded. 

By enabling CloudWatch logs for CodeBuild, we can keep an eye on CodeBuild's logs and get a record of exactly what happens during the build process. This helps us understand and fix any issues that come up, making it easier to see what worked and what didn't.. .. and I selected CloudWatch logs.

---

## Create a buildspec.yml file

I created a buildspec.yml file in my project because makes sure that every build follows the same steps and produces consistent results.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codebuild_35588a47)

---

## Create a CodeBuild build project

### My buildspec.yml file has four stages

The first two phases in my buildspec.yml file are:

1️⃣ install tells CodeBuild the Java runtime that should be installed when building this project. 

2️⃣ pre_build contains the commands to run before building your web app. 
• echo Initializing environment tells the terminal to print out "Initializing environment" as a message in the terminal, which helps you know that the environment setup process is 🔥starting🔥
• export CODEARTIFACT_AUTH_TOKEN... gets an authorization token (which is like an access pass) from AWS CodeArtifact. Maven will later use this token to securely access your CodeArtifact repository during the build process.

The third phase in my buildspec.yml file:

3️⃣ build kicks off your build process with just two commands!
• echo Build started on date tells the terminal to print out "Build started on" followed by the current date and time, indicating the start of the build process.
• mvn -s settings.xml compile tells Maven to compile the project using the settings in the settings.xml file. You might recall entering this command manually in Step #1 of this project! Now it's automated

The fourth phase in my buildspec.yml file:

4️⃣ The post-build phase runs another two commands in your terminal:
• echo Build completed on date tells the terminal to print out "Build completed on" followed by the current date and time, telling you that the build process is now ✨complete✨
• mvn -s settings.xml package tells Maven to package the compiled code into a WAR file using the settings in the settings.xml file.

📜 The artifacts section include commands to manage the the WAR file that gets created during the build process: 
• The files section describes the files that we want to save as a build artifact once the build is done. 
• target/nextwork-web-project.war tells the terminal to select the WAR file in the target folder as a build artifact. 
• discard-paths: no makes sure your WAR file is saved in the target folder. If discard-paths was set to yes, the WAR file would be saved directly in the root directory which isn't best practise for organising your web app's files. he root 

---

## Modify CodeBuild’s IAM role

### Before building my CodeBuild project, I modified its service role first. 

My CodeBuild project's service role was first created when we selected new service role when we were setting up our build environment with CodeBuild.

I attached a new policy called codeartifact-nextwork-consumer-policy.

Attaching this policy means, it can modify the IAM role's permissions so it can fetch the necessary packages it needs to complete your web app's build process.

Because the new service role we selected initially gave CodeBuild the minimum permissions it needs to build your web app, which by default does not include access to your CodeArtifact repository. 

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codebuild_84fcff2a)

---

## My first project build 💪

To build my project, all I had to do was select the nextwork-web-build project and select Start build.

he build process in CodeBuild took about 3 minutes.

Once the build is complete, I opened my S3 bucket to verify I have a packaged WAR file inside a zip named nextwork-web-project.zip.

I saw nextwork-web-build.zip, which verified that the build was completed successfully.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codebuild_d9cc6191)

---

---
