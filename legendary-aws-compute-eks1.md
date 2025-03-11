<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launch a Kubernetes Cluster

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks1)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Launch a Kubernetes Cluster

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks1_e5f6g7h8)

---

## Introducing Today's Project!

In this project, I will deploy my first Kubernetes cluster using Amazon EKS (Elastic Kubernetes Service)! This is because EKS is AWS service for deploying Kubernetes clusters in the cloud.

### What is Amazon EKS?

Amazon EKS is AWS cloud Kubernetes service that is pre-configured with all the needed resources/infrastructure - such as networking, hardware, scaling, and security settings...required to manage and deploy Kubernetes clusters and integrate Kubernetes with other AWS services on the cloud or on-premise.

In this project, I used EKS to create my first Kubernetes cluster and tested its resilience/recoverability when nodes in the cluster fail.


### One thing I didn't expect

One thing I didn't expect in this project was that, I initially ran into an error while using eksctl (the CLI tool for AWS EKS) to create the cluster...(eksctl not found) was because I haven't installed eksctl yet. So I had to run a command to download and install the latest eksctl from GitHub, on my EC2 instance.

Another highlight for me was the failover capability of the Kube cluster, I didn't expect that when I deleted all 3 nodes Kubernetes automatically detected the change and redeployed new nodes (EC2 instances) very quickly.

### This project took me...

This project took me about 7 hours. The longest part was creating the cluster, troubleshooting and understanding the root cause of errors and the documentation.

---

## What is Kubernetes?

Kubernetes is a container orchestration platform, which means it's a tool for automatically coordinating or managing containers or containerized application...so they're running smoothly across all your servers. It makes sure all your containers are running where they should, scales containers automatically to meet demand levels, and even restarts containers if something crashes.

Companies and developers use Kubernetes as the standard tool to deploy and manage large scale, container-based applications steady and easy to scale with traffic.

I used eksctl (the CLI tool for AWS EKS clusters) to create a Kubernetes cluster. The eksctl create cluster command I ran defined:
 -the name of the cluster
-the name of node group
-the number of nodes / node size
-the node type
-the region of the cluster
-the version of Kubernetes to use

I initially ran into 3 errors while using eksctl. 

The first one (eksctl not found) was because I haven't installed eksctl yet. So I had to run a command to download and install the latest eksctl from GitHub, on my EC2 instance.

The second one was because the specific AWS region of the cluster was not set in the command. So I had to set the aws region to the same region as my EC2 instance.

The third one was because my EC2 lacked permissions to even access EKS services let alone create clusters, so I had to create an IAM role and grant Admin permissions for it.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks1_ff9bfc221)

---

## eksctl and CloudFormation

CloudFormation helped create my EKS cluster because CloudFormation is AWS’s service for setting up infrastructure as code. You write a template describing the resources you need (like an instruction manual), and CloudFormation automatically creates and configures the necessary resources for the EKS cluster.

CF created VPC resources because creating the EKS cluster in my default VPC would cause manually editing settings, else there would be compatibility and permission issues.

There was also a second CloudFormation stack for node group...which is a group of EC2 instances that will run your containers. The difference between a cluster and node group is that...

A cluster is the entire environment that Kubernetes manages for your containerized app. This cluster is made up of nodes (the servers that actually run your containers) and a control plane (the brain that decides things like when to create or shut down containers).

Within your cluster, the nodes are organized into node groups (a grp of EC2s). Node groups let you manage multiple nodes more easily by grouping them together, so you can control settings like instance type and resource limits for the whole group instead of adjusting each node individually. This makes it much simpler to scale and configure nodes for specific tasks.

CloudFormation separates the core EKS cluster stack from the node group stack to make it easier to manage and troubleshoot each part independently, especially if one half fails.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks1_w3e4r5t6)

---

## The EKS console

I had to create an IAM access entry in order to gain access to Kubernetes objects. Even though I have AdministratorAccess in AWS, which grants full access to AWS resources, BUT that alone doesn’t automatically carry over to Kubernetes — Kubernetes has its own way of managing access within a cluster.
Kubernetes will only let you into different parts of the cluster if you have permissions under its own system too. Therefore I had to create an ACCESS ENTRY and add the AmazonEKSClusterAdminPolicy which gives me full administrative rights over my EKS clusters...so that my IAM user will be able to see and control all parts of my EKS cluster - inclusding all the nodes inside!

Access entries are permissions that connect IAM users with Kubernetes Role-Based Access Control (RBAC) which is Kubernetes system to manage access inside a cluster.

In short, my IAM Admin user (the one I'm using to open this Management Console) needs a specific access entry to get access to the nodes in my EKS cluster.

It took 30 minutes to create my cluster. Since I'll create this cluster again in the next project of this series, maybe this process could be sped up if I can save the Cloudformation template in a YAML file or use Terraform.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks1_e5f6g7h8)

---

## EXTRA: Deleting nodes

Did I know I can find your EKS cluster's nodes in Amazon EC2? This is because when I created nodes in a Kubernetes cluster on AWS, each node is actually an EC2 instance!

Kubernetes uses a generic term (node) because different cloud platforms use different cloud resources to be the node; in AWS, we use EC2 instances as nodes.

Desired size is the number of nodes you want running in your node group =3 in this project. Mininum and maximum sizes are helpful for Kubernetes, being a container orchestration platform, to automatically adjust the number of nodes based on the demands of your application.

When I deleted my EC2 instances Kubernetes automatically detects the change and launches new nodes to keep the cluster running at its desired state.

This is because -one of the key benefits of using Kubernetes – it makes sure your application is always running, even if some nodes fail.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eks1_q7r8s9t0)

---

---
