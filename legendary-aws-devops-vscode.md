<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up a Web App in the Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-vscode)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Set Up a Web App Using AWS and VSCode

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_7a1de541)

---

## Introducing Today's Project!

### What is VSCode and why is it useful?

Today I used VSCode to connect with the EC2 instances, so I can create and develop my web app's code.

### How I'm using VSCode in this project

Today I used VSCode to connect with the EC2 instances, so I can create and develop my web app's code.

### One thing I didn't expect...

Key takeaways for me was that I learned that VSCode has multi-purpose capabilities such as the provision on Remote SSH connection to securely connect to a remote EC2 server instance.

I also learned about creating IAM user logins from a root user to protect confidentiality of root user access.

### This project took me...

3 hours

---

## Launching an EC2 instance

I started this project by launching an EC2 instance because EC2 is the service that creates virtual computers/servers, an instance is one of those computers/servers hence that will be the home for all my web app's files.

### I also enabled SSH

SSH = Secure Shell is a protocol used to make sure only authorized users can access a remote server. When I connect to the EC2 instance, SSH verifies I have the correct private key that matches the public key on the server.

I enabled SSH so that I can securely connect remotely to the EC2 instance.

### Key pairs

A key pair is like the keys to your virtual computer. Just like you need a key to unlock and start your car, a key pair lets you securely access your EC2 instance.

It's made of two halves: a public key that AWS keeps, and a private key that you download.

Once I set up my key pair, AWS automatically downloaded my private key in a .pem format..stands for privacy enhanced mail

---

## Set up VSCode

VSCode is an IDE (Integrated Development Environment), which means software that help you write and edit code. 

VSCode comes with extra tools that can be used to connect to virtual servers like EC2 instances.

I installed VSCode to connect with the instance, so I can create and develop my web app's code.


![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_53d05e68)

---

## My first terminal commands

A terminal is where you send instructions to your computer using text instead of clicks. It's like sending text messages to your computer's operating system to tell it what to do.

Every computer has a terminal. On Windows, it's often called Command Prompt or PowerShell, while macOS and Linux systems use Terminal.... 

The first command I ran for this project is cd ~/Desktop/DevOps



I also updated my private key's permissions by executing this code:
icacls "nextwork-keypair.pem" /reset
icacls "nextwork-keypair.pem" /grant:r "KT:R"
icacls "nextwork-keypair.pem" /inheritance:r

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_9328ada1)

---

## SSH connection to EC2 instance

To connect to my EC2 instance, I ran the command ssh -i ~/Desktop/DevOps/nextwork-keypair.pem ec2-user@ec2-54-224-6-15.compute-1.amazonaws.com


### This command required an IPv4 address

A server's IPV4 DNS is the public address of the server that the internet uses to connects to it.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_e3069dca)

---

## Maven & Java

Apache Maven is a tool that helps developers build and organize Java software projects. It's also a package manager, which means it automatically download any external pieces of code your project depends on to work.

We're also using Maven today because it's really useful for kick-starting web projects! It uses something called archetypes, which are like templates, to lay out the foundations for different types of projects e.g. web apps.


Maven is required in this project because it help us set up all the necessary web files to create a web app structure, so we can jump straight into the fun part of developing the web app sooner.

Java is a popular programming language used to build different types of applications, from mobile apps to large enterprise systems.

Amazon Corretto 8 is a version of Java that we're using for this project. It's free, reliable and provided by Amazon.

Java is required in this project because Maven, which we just downloaded, is a tool that NEEDS Java to operate. So if we don't install Java, we won't be able to use Maven to generate/build our web app today.



---

## Create the Application

I generated a Java web app using the command:

mvn archetype:generate \
   -DgroupId=com.nextwork.app \
   -DartifactId=nextwork-web-project \
   -DarchetypeArtifactId=maven-archetype-webapp \
   -DinteractiveMode=false


I installed Remote - SSH, which is an extension in VScode. I installed it so that I can connect directly via SSH to my EC2 instance securely over the internet. This enables me to use VSCode to work on files or run programs on that server as if I were doing it on my own computer...better than doing it through the terminal.



Configuration details required to set up a remote connection include:
-Host name of the virtual EC2 instance server, 
-Directory path of the private key file and the 
-Username of the EC2 instance.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_2939cf01)

---

## Create the Application

Using VSCode's file explorer, I could see src, webapp, resources, pom.xml files

Two of the project folders created by Maven are src and webapp, which are:

-The src (source) folder holds all the source code files that define how your web app looks and works.

-src is further divided into webapp, which are the web app's files e.g. HTML, CSS, JavaScript, and JSP files, and resources, which are the configuration files a web app might need e.g. connection settings to a database.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_45f91fd7)

---

## Using Remote - SSH

index.jsp is a file used in Java web apps. It's similar to an HTML file because it contains markup to display web pages.

However, index.jsp can also include Java code, which lets it generate dynamic content.

This means content can change depending on things like user input or data from a database. Social media apps are great examples of web apps because the content you see is always changing, updating and personalised to you. HTML files are static and can’t include Java code. That's why it's so important to install Java in your EC2 instance - so you can run the Java code in your web app!

I edited index.jsp by changing the placeholder code

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-vscode_7a1de541)

---

## Using nano

---

---
