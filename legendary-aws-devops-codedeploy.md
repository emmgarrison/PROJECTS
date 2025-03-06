<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy an App with CodeDeploy

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codedeploy)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Deploy an App with CodeDeploy

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_69ede4b0)

---

## Introducing today's project!

### What is AWS CodeDeploy?

AWS CodeDeploy is used to host and deploy the packed WAR file to a server / EC2 instances, (i.e. servers that deliver code to users) giving you full control over your server environment.

### How I'm using AWS CodeDeploy in this project

I used CodeDeploy application as a folder that holds all the settings it needs for a deployment. Think of this as setting up a template for deploying a web app, so you won't need to configure all the settings from scratch every time! This means using CodeDeploy applications helps to streamline the deployment process and ensures consistency across deployments

### One thing I didn't expect...

Key highlight for me was to create deployment scripts. These scripts automate the setup, startup, and shutdown of your web app on EC2 instances, ensuring a consistent and error-free deployment environment.

### This project took me...

2 hours

---

## Set up an EC2 instance

I set up an EC2 instance and VPC because This new instance is created specifically for running our application in a live, production environment i.e. what users will see. 

💡 Why do we need a VPC for a web app? We didn't need this to host a static website.
Web apps need a VPC because they have more complex needs like connecting with multiple resources that communicate with each other e.g. databases and EC2 instances, keeping everything secure, and controlling network traffic.

We manage production and development environments separately because by having a separate EC2 instance for deploying our application, we avoid the risk of testing any code changes/new features in a live production environment that users end up seeing too.

To set up my EC2 instance and VPC, I used CloudFormation.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_26e7b830)

---

## Bash scripts

Scripts are code snippets that help you automate tasks run on your computer's terminal. Imagine you have a list of commands to run, like installing software or starting a program. Instead of doing each step by hand every time in your VSCode terminal, you write them down in a script. Then, when you run the script, it does all those steps for you in order. This makes repetitive tasks easier and faster.

### I used three scripts for my project's deployment

The first script I created was install_dependencies.sh;

#!/bin/bash
sudo yum install tomcat -y
sudo yum -y install httpd
sudo cat << EOF > /etc/httpd/conf.d/tomcat_manager.conf
<VirtualHost *:80>
  ServerAdmin root@localhost
  ServerName app.nextwork.com
  DefaultType text/html
  ProxyRequests off
  ProxyPreserveHost On
  ProxyPass / http://localhost:8080/nextwork-web-project/
  ProxyPassReverse / http://localhost:8080/nextwork-web-project/
</VirtualHost>
EOF


The second script I created was start_server.sh;

#!/bin/bash
sudo systemctl start tomcat.service
sudo systemctl enable tomcat.service
sudo systemctl start httpd.service
sudo systemctl enable httpd.service


The third script I created was stop_server.sh;

#!/bin/bash
isExistApp="$(pgrep httpd)"
if [[ -n $isExistApp ]]; then
sudo systemctl stop httpd.service
fi
isExistApp="$(pgrep tomcat)"
if [[ -n $isExistApp ]]; then
sudo systemctl stop tomcat.service
fi


---

## Bash scripts

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_69ede4b0)

---

## CodeDeploy’s IAM Role

I created an IAM service role for CodeDeploy so that CodeDeploy can deploy to an EC2 instance! Right now, CodeDeploy does not have access to EC2, so we're going to grant access using the AWS Managed AWSCodeDeployRole policy.

To set up CodeDeploy’s IAM role, I created a role in the AWS IAM console.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_59de20cd)

---

## CodeDeploy application

A CodeDeploy application works like a folder that holds all the settings it needs for a deployment. Think of this as setting up a template for deploying a web app, so you won't need to configure all the settings from scratch every time! 

This means using CodeDeploy applications helps to streamline the deployment process and ensures consistency across deployments - very intertwined with what DevOps is all about!

To create a CodeDeploy application, I had to select a compute platform, which means it can deploy applications to different types of environments. Besides EC2/On-premises environments, it can also deploy to AWS Lambda (serverless applications) and Amazon ECS (containerized applications).

The compute platform I chose was EC2 because it provides us with virtual servers in the cloud that we can fully control.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_75e763ef)

---

## Deployment group

A deployment group means specific instructions for one particular deployment scenario. It defines which servers to use, how to deploy, and what settings to apply for that specific deployment.

### Two key configurations for a deployment group

Environment means it tells CodeDeploy what kind of servers you want to use for your application. In this project, it's Amazon EC2 instances!

A CodeDeploy Agent is is like a helper installed on your servers (EC2 instances) that communicates with CodeDeploy to get the instructions for deploying your application. It ensures that the servers know what to do when a new version of your application needs to be deployed.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_054373a8)

---

## CodeDeploy application

To create my deployment, I had to set up a revision location, which means a the place where CodeDeploy looks to find your application's build artifacts, like a zip file or WAR file, stored in an S3 bucket. 

We're using the S3 bucket that's storing our WAR bucket so CodeDeploy knows where to find the latest version of our web app it's deploying to the target instances!

My revision location was my S3 bucket.

To visit my web app, I had to visit the public address link, of my EC2 instance using http:// at the start of your link. 

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-codedeploy_a333c89c)

---

---
