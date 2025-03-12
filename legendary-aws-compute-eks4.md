<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy Backend with Kubernetes

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks4)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Deploy Backend with Kubernetes

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks4_6cfb382f2)

---

## Introducing Today's Project!

In this project, I will:
-Set up the backend of an app for deployment.
-Install kubectl.
-Deploy the backend on a Kubernetes cluster.
-(Secret Mission) Track my Kubernetes deployment using EKS.

### Tools and concepts

I used Kubernetes, ECR, kubectl, EKS, eksctl, nano, Github, EC2, IAM, Dockerfile, Linux, Bash
etc... to create Kubernetes clusters, nodes, node groups and pods ... Key concepts include using manifests -Deployment manifest that tells Kubernetes how to deploy my backend...and Service manifest that tells Kubernetes how to expose my backend to my users.

### Project reflection

This project took me approximately 5 hours. 

The most challenging part was installing the Kubectl (the CLI for Kubernetes) and applying the manifests from the EC2 instance connect terminal....I ran into an ERROR: error validating "flask-deployment.yaml": error validating data: failed to download openapi: Get "http://localhost:8080/openapi/v2?timeout=32s": dial tcp 127.0.0.1:8080: connect: connection refused. I troubleshooted, researched and resolved the error successfully.

My favourite part was navigating back in the EKS console and seeing the cluster with the nodes, node group and the pods visibly created as well as tracking the events for each pod. This validated that my backend is up and working.

---

## Project Set Up

### Kubernetes cluster

To set up today's project, I launched a Kubernetes cluster. The cluster's role in this deployment is to deploy and manage a containerized application (specifically, its backend) using Kubernetes.

When you're ready to deploy your application, the EKS cluster is where Kubernetes takes your app, runs them on groups of containers called nodes, and makes sure everything stays up and running.

### Backend code

Next, I retrieved the backend that I plan to deploy. An app's backend means the "brain" of the app. It's where my app processes user requests and stores and retrieves data. 

I retrieved backend code by cloning the app's backend repository from GitHub. This repository contains all the backend code needed for your application, including files like app.py, Dockerfile, and requirements.txt.

By cloning this repository, I'm copying all the code and resources onto my EC2 instance so I can build, run, and deploy the backend part of my project.

Once I've cloned it, the project will look like a new folder in my EC2 instance with the entire project's files inside.

### Container image

Once I cloned the backend code, I built a container image because Kubernetes requires it - a container image is like a blueprint/template that contains all the instructions, code, libraries, and dependencies needed by Kubernetes as the basis for deploying clusters in order to run the application.

Kubernetes also uses the container image to set up multiple, identical containers so my application runs consistently across different environments. Whether I'm deploying in development, testing, or production, my app behaves the same way because Kubernetes refers to my image each time a new instance/node needs to be created.

I used the command docker build -t nextwork-flask-backend .
Without an image, it would be difficult for Kubernetes to create a cluster without knowing the code, libraries, and dependencies, network, and all the basic infrastructure needed for the deployment of the code.

I also pushed the container image to a container registry, which is the Elastic Container Registry ECR because ECR facilitates scaling for my deployment because. If I didn't use a container registry, I’d need to preload every node in your Kubernetes cluster with the image. I'd also need to update each node manually with every change to my container image.

---

## Manifest files

Kubernetes manifests are a set of instructions that tells Kubernetes how to run your app. Manifests are helpful because Kubernetes uses this information to know what your app needs, like which containers to run, how many copies to create, and how much memory to allocate.

Without manifests, Kubernetes wouldn’t know how to set up and manage your app automatically. This means you'd have to manually configure each container every time you deploy, which would be confusing, error-prone, and hard to repeat. Manifests make deployment simple, clear, and consistent.

A Kubernetes deployment manages Kubernetes on exactly how to manage these tasks. It includes details like the how many copies of my app should run across my cluster, and which settings to apply (e.g. CPU limits, container image, or network settings).

I'm only deploying the backend of our app, but a Deployment manifest can manage multiple components, like frontend servers, databases, and other services; each with its own specific configuration.

The container image URL in my Deployment manifest tells Kubernetes where to pull the container image from when it deploys my application.

A Kubernetes Service exposes my application i.e. making it accessible to network traffic (from within the cluster or from external sources). 

My Service manifest tells Kubernetes to create/update a Service using details like which pods to route traffic to, the type of traffic it should handle, and which ports should be used.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks4_b01876554)

---

## Backend Deployment!

To deploy my backend application, I install a key tool using in this command, sudo curl -o /usr/local/bin/kubectl \
https://s3.us-west-2.amazonaws.com/amazon-eks/1.31.0/2024-09-12/bin/linux/amd64/kubectl

Once kubectl is successfully installe, I used it to apply my manifests and deploy our application.

### kubectl

kubectl is the command-line tool for interacting with Kubernetes resources (e.g. , Deployment or Service resources) once your cluster is up and running. I need this tool to apply our manifests and deploy our application. I can't use eksctl for the job because is great for setting up and deleting your EKS cluster and configuring its settings.

But, when it comes to deploying applications and managing resources within the cluster, kubectl is the tool to use.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks4_6cfb382f2)

---

## Verifying Deployment

My extension for this project is to use the EKS console to verify my backend's deployment manifest instructions was followed in the deployment of the nodes in EKS Kubernetes cluster.

I had to set up IAM access policies because IAM User has AdministratorAccess permissions alone don’t automatically carry over to Kubernetes — Kubernetes has its own way of managing access within a cluster.
Even if you have AdministratorAccess in AWS, which grants full access to AWS resources, Kubernetes will only let you into different parts of the cluster if you have permissions under its own system too.

I set up access by running this command in my ec2 instance connect terminal...eksctl create iamidentitymapping --cluster nextwork-eks-cluster --arn arn:aws:iam::225989350324:user/Emmanuel-IAM-Admin --group system:masters --username admin --region us-east-1

Once I gained access into my cluster's nodes, I discovered pods running inside each node. Pods are bundle containers together so they can work as a unit. Pods are the smallest things you can deploy - you can't deploy containers on their own; they must be inside a Pod. In more technical terms, we'd call pods the smallest deployable units in Kubernetes.

Containers in a pod share the same network space and storage so they can communicate and share data more efficiently.

The EKS console shows you the events for each pod, where I could see:
-Successfully assigned default/demo-flask-backend...: Your pod has been given an internal IP address in the cluster.
-Pulling image...: The pod is getting your container image from Amazon ECR.
-Successfully pulled image...: The image was successfully downloaded to the cluster.
-Created container demo-flask-backend: The container was built from your image and prepared for launch.
-Started container demo-flask-backend: The container has been successfully started and is now running in your pod.

This validated that my backend is up and working and can be accessed within the cluster's network.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks4_3b391f873)

---

---
