<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Create Kubernetes Manifests

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks3)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Create Kubernetes Manifests

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks3_b01876555)

---

## Introducing Today's Project!

In this project, I will;
-Set up a Deployment manifest that tells Kubernetes how to deploy my backend.
-Set up a Service manifest that tells Kubernetes how to expose your backend to my users.
-(Secret Mission) Dive into the details of my manifest files and discover new learnings!

### Tools and concepts

I used Amazon EKS, eksctl, Kubernetes, ECR, nano, Github, EC2, IAM, Dockerfile, etc... to create Kubernetes clusters ... Key concepts include using manifests - Deployment manifest that tells Kubernetes how to deploy my backend...and Service manifest that tells Kubernetes how to expose my backend to my users.


### Project reflection

I did this project to be equipped with hands-on practical and most in-demand skills in the market. Also to understand the concepts and build complex solutions/applications that integrate more tools such as Kubernetes, Terraform, EKS, ECR, Docker, Github and multi-cloud architectures to solve real world problems that companies face.

This project took me approximately 5 hours. The most challenging part was how to provide write permissions for ec2-user to create directories and files. My favourite parts was creating the Deployment and Service Manifests...and annotating and decomposing the different layers that make up the Deployment manifest. I learned about nano, a built-in text editor within the terminal!

---

## Project Set Up

### Kubernetes cluster

To set up today's project, I launched a Kubernetes cluster. Steps I took to do this included:
-Launched and connected to an EC2 instance.
-Set up my EC2 instance's IAM role.
-Create an EKS cluster using eksctl.

### Backend code

Next, I retrieved the backend that I plan to deploy. An app's backend means the "brain" of the app. It's where my app processes user requests and stores and retrieves data. 

I retrieved backend code by cloning the app's backend repository from GitHub. This repository contains all the backend code needed for your application, including files like app.py, Dockerfile, and requirements.txt.

By cloning this repository, I'm copying all the code and resources onto my EC2 instance so I can build, run, and deploy the backend part of my project.

Once I've cloned it, the project will look like a new folder in my EC2 instance with the entire project's files inside.

### Container image

Once I cloned the backend code, I built a container image because Kubernetes requires it - a container image is like a blueprint/template that contains all the instructions, code, libraries, and dependencies needed by Kubernetes as the basis for deploying clusters in order to run the application.

Kubernetes also uses the container image to set up multiple, identical containers so my application runs consistently across different environments. Whether I'm deploying in development, testing, or production, my app behaves the same way because Kubernetes refers to my image each time a new instance/node needs to be created.

I used the command docker build -t nextwork-flask-backend .

I also pushed the container image to a container registry, because if I didn't use a container registry, I’d need to preload every node in your Kubernetes cluster with the image. I'd also need to update each node manually with every change to my container image.

---

## Manifest files

Kubernetes manifests are a set of instructions that tells Kubernetes how to run your app. Manifests are helpful because Kubernetes uses this information to know what your app needs, like which containers to run, how many copies to create, and how much memory to allocate.

Without manifests, Kubernetes wouldn’t know how to set up and manage your app automatically. This means you'd have to manually configure each container every time you deploy, which would be confusing, error-prone, and hard to repeat. Manifests make deployment simple, clear, and consistent.

A Kubernetes deployment manages Kubernetes on exactly how to manage these tasks. It includes details like the how many copies of my app should run across my cluster, and which settings to apply (e.g. CPU limits, container image, or network settings).

I'm only deploying the backend of our app, but a Deployment manifest can manage multiple components, like frontend servers, databases, and other services; each with its own specific configuration.

The container image URL in my Deployment manifest tells Kubernetes where to pull the container image from when it deploys my application.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks3_b01876554)

---

## Service Manifest

A Kubernetes Service exposes my application i.e. making it accessible to network traffic (from within the cluster or from external sources). 

I need a Service manifest to tell Kubernetes to create/update a Service using details like which pods to route traffic to, the type of traffic it should handle, and which ports should be used.

My Service manifest sets up my app to the outside world or other parts of your system. 

In this case, the Service manifest I created sets up a Service that routes external traffic to port 8080 on pods labeled app: nextwork-flask-backend. This allows traffic from outside the cluster (e.g., my users) to reach my app!

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks3_b01876555)

---

## Deployment Manifest

Annotating the Deployment manifest helped me understand what each layer does because as I move down the Deployment manifest file, I'm essentially peeling back layers of Kubernetes’ architecture, going from the API level all the way down to the container level.

A notable line in the Deployment manifest is the number of replicas, which means identical copies (or instances) of the app Kubernetes should run. Pods are relevant to this because Pods are groups of containers that are bundled together so they can work as a unit.

Pods are the smallest things you can deploy - you can't deploy containers on their own; they must be inside a Pod. In more technical terms, this means Pods are the smallest deployable units in Kubernetes.

Containers in a Pod share the same network space and storage so they can communicate and share data more efficiently.

One part of the Deployment manifest I still want to know more about is the specs for the Pods and Container layers.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks3_6aae73e71)

---

---
