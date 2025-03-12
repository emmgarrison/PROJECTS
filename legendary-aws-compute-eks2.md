<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up Kubernetes Deployment

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks2)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Set Up Kubernetes Deployment

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks2_45e6c3de5)

---

## Introducing Today's Project!

By the end of this project, you will...
📥 Clone a backend application from GitHub.
💿 Build a Docker image of the backend.
🗃️ Push your image to an Amazon ECR repository.
🤯 Troubleshoot installation and configuration errors - as a PRO student, get ready to face errors head-on and push your skills to the next level.
💎 (Secret Mission) Dive into the backend code on GitHub.

### Tools and concepts

I used Amazon EKS, Git, Github, Docker, ECR...Key steps include:
-Cloning the backend application from GitHub.
-Building a Docker image of the backend.
-Pushing my image to an Amazon ECR repository.
-Troubleshooting installation and configuration errors
-Diving deeper into the backend code on GitHub to 
breakdown the 3 main files in my backend app's Github repo:

1. requirements.txt == that lists the dependencies my app needs.
2. The Dockerfile == that provides a set of instructions for building a Docker image for my app.
3. app.py == is the core of my backend. It contains the actual code that determines what my app does and the kind of responses it sends back to the client. 

### Project reflection

This project took me approximately 5 hours. The most challenging part was reviewing and understanding what's happening in the Docker and app.py files. My favourite part was troubleshooting the installation and configuration errors.

Something new that I learnt from this experience was the Amazon Elastic Container Registry ECR -to securely store, share, and deploy container images. ECR is a good choice for the job  because it's an AWS service, which lets Elastic Kubernetes Service (EKS) access and deploy my container image with minimal authentication setup.

---

## What I'm deploying

To set up today's project, I launched a Kubernetes cluster. Steps I took to do this included:
-Launched and connected to an EC2 instance.
-Set up my EC2 instance's IAM role.
-Create an EKS cluster using eksctl.

### I'm deploying an app's backend

Next, I retrieved the backend that I plan to deploy. An app's backend means the "brain" of the app. It's where my app processes user requests and stores and retrieves data. 

I retrieved backend code by cloning the app's backend repository from GitHub. This repository contains all the backend code needed for your application, including files like app.py, Dockerfile, and requirements.txt.

By cloning this repository, I'm copying all the code and resources onto my EC2 instance so I can build, run, and deploy the backend part of my project.

Once I've cloned it, the project will look like a new folder in my EC2 instance with the entire project's files inside.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks2_1ebb86c71)

---

## Building a container image

Once I cloned the backend code, my next step is to build a container image of the backend. This is because Kubernetes requires it - a container image is like a blueprint/template that contains all the instructions, code, libraries, and dependencies needed by Kubernetes as the basis for deploying clusters in order to run the application.

Kubernetes also uses the container image to set up multiple, identical containers so my application runs consistently across different environments. Whether I'm deploying in development, testing, or production, my app behaves the same way because Kubernetes refers to my image each time a new instance/node needs to be created.

When I tried to build a Docker image of the backend, I ran into a permissions error because ec2-user, which is the user I've used to access this instance, doesn't have permission to run Docker commands. When Docker was installed, it was set up for the root user (with sudo, which lets a non-root user run commands with root user rights).
But, it's good practice to give your ec2-user the permission instead of using sudo each time.

💡 Why am I logged in as ec2-user instead of the root user?
When you launch an EC2 instance using certain AMIs like Amazon Linux, the default user for SSH access is ec2-user. 

When you access your instance as ec2-user, it’s like logging into an AWS account as an IAM admin user—you’ve got a lot of control but not the absolute top level unless you're using sudo. If you need to run Docker commands, those require root privileges because Docker needs root-level access to create containers or build container images.

To solve the permissions error, I added ec2-user to the Docker group. The Docker group is a group in Linux systems that gives users the permission to run Docker commands. By default, only the root user can run Docker commands. When you add a user (e.g., ec2-user) to the Docker group, it lets that user run Docker commands without typing sudo every time.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks2_45e6c3de5)

---

## Container Registry

I'm using Amazon ECR in this project to securely store, share, and deploy container images. ECR is a good choice for the job  because it's an AWS service, which lets Elastic Kubernetes Service (EKS) deploy my container image with minimal authentication setup.

Container registries like Amazon ECR are great for Kubernetes deployment because if I didn't use a container registry, I’d need to preload every node in my Kubernetes cluster with the image. I'd also need to update each node manually with every change to my container image.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks2_l2m3n4o5)

---

## EXTRA: Backend Explained

After reviewing the app's backend code, I've learnt that:
1. My app's backend fetches data based on a search topic you enter.
2. The backend code uses Flask to connect with an external API (Hacker News Search API) and process the data.
3. The backend then sends the data back as formatted JSON.

### Unpacking three key backend files

The requirements.txt file lists the dependencies your application needs to run properly.

The Dockerfile gives Docker instructions on how a Docker image of the backend should be built. Notice the contents of this file and how it builds your Docker image step-by-step.

Key commands in this Dockerfile include:
FROM python:3.9-alpine
LABEL Author="NextWork"
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
CMD ["python3", "app.py"]

The app.py file contains the main code for your backend. It has three main parts:
-Setting up the app and routing
-Fetching data
-Sending the response

---

---
