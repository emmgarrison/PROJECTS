# EKS Project: Launch EKS cluster, Set Up Kubernetes Deployment, Create Kubernetes Manifests and Deploy Backend with Kubernetes

## By Ariana G. Vidal

> *NextWork Student*

## What is Kubernetes?

Kubernetes is an open-source tool that automates the deployment, scaling, and management of containerized applications. Companies and developers use Kubernetes to automate the deployment, scaling, and management of containerized applications across various environments.

## Project 1: Launching EKS Cluster

### What is Amazon EKS?

Amazon EKS (Elastic Kubernetes Service) is a fully managed service provided by Amazon Web Services (AWS) that allows you to run Kubernetes clusters on AWS without the need to install and operate your own Kubernetes control plane.

💡 **Why are we launching an EKS cluster?**

In this project, your goal is to deploy and manage a containerized application (specifically, its backend) using Kubernetes.
Your `EKS cluster` is the **control center** where Kubernetes schedules containers, manages their resources, and handles tasks like scaling, networking, and updates. When you're ready to deploy your application, the EKS cluster is where Kubernetes takes your app, runs them on groups of containers called nodes, and makes sure everything stays up and running. Think of it as Kubernetes' brain for your deployment!

**In this step, you're going to:**

- Launch an EC2 instance.
- Connect to your instance so you can start sending commands.

### 1. Launch and connect to an EC2 instance

- Log into your **AWS Management Console** as your **IAM Admin user**.
- Head to **EC2** in your AWS Management Console.
- Select **Instances** at the left hand navigation bar.
- Check that you're using the **AWS Region** that's closest to you. Select the Region name at the top right corner of your console if you'd like to switch.
- Select **Launch instances** at the top right corner of your console.
  - **Name** your EC2 instance `nextwork-eks-instance`
  - For the **Amazon Machine Image**, select `Amazon Linux 2023 AMI`.
  - For the **Instance type**, select `t2.micro`.
  - For the **Key pair** (login) panel, select `Proceed without a key pair (not recommended)`.
  - We won't change anything in the **Networking** section and keep the `default security group`.
  - Select `Launch instances`.

> Notice that the security group allows SSH traffic from anywhere (0.0.0.0/0) which isn't the best security practice, but we'll leave this to make connecting via SSH easier later on.

💡 **What is AMI? What is Free tier eligible?**

An `AMI (Amazon Machine Image)` is a template or blueprint used to create `EC2 instance`. The image defines your EC2 instance's operating system and with the applications needed to launch the instance.
**Free tier eligible AMIs** are those that qualify for the AWS Free Tier, so you won't get charged for using it. Just remember that not all AMIs are free!

💡 **What is instance type?**

If `AMIs` give you pre-built **software and operating systems**, `instance types` cover the **'hardware' components**. CPU power, memory size, storage space and more!
So, while the AMI decides what operating system your server runs, the instance type determines how fast and powerful it performs.
For example, a larger instance type might have more CPU power for heavy tasks, while a smaller type (for example, t2.micro) might be more affordable for light use.

💡 **What is a key pair?**

A `key pair` is like a **lock and a key** for your EC2 instance. Usually you'd need a key pair to SSH connect to your instance, but we're using a simpler way to do this later in this step (EC2 Instance Connect).
If you're new to key pairs and SSH connections and want to learn more, check out Steps #1 and #2 of Testing VPC Connectivity.

💡 **Connect to the instance using EC2 instance connect**

EC2 Instance Connect is a shortcut way to get direct SSH access to your EC2 instance. Instead of having to manage a key pair manually, Instance Connect connects you to your instance within your AWS Management Console - no download required!

💡 **Extra for PROs: Why are we using EC2 Instance Connect?**

When you connect via SSH to an EC2 instance (which you can learn how to do in Set Up a Web App in the Cloud), you usually need to:

1. Generate a key pair.
2. Associate the public key with your EC2 instance.
3. Securely store your private key on your local machine.
4. Set up an SSH client (a software that can handle the SSH protocol, like Terminal on Max/Linux or VSCode).
5. Provide your private key and run ssh commands to set up an SSH connection to your EC2 instance.

EC2 Instance Connect lets us skip all those steps and connect to your EC2 instances directly using the AWS Management Console. We'd use the usual, manual way when we have some development work to do, but Instance Connect is great if we're just running a few commands in the terminal.

- Select the instance you just launched.
- Select the checkbox next to `nextwork-eks-instance`.
- Select the **Connect** button.

> Welcome to the instance connection page! This is where AWS gives you different options to connect with your EC2 instance.

- Select **Connect** at the bottom of the page.
- This will open a terminal session directly in your browser in a new tab.

💡 **What is the yellow warning banner about Port 22?**

Good spotting! That warning banner is a security reminder from AWS.
Port 22 is the default port for SSH (Secure Shell) connections, which is what we use to connect securely to our EC2 instance.
Back when we set up our EC2 instance, we used the default security group which allows SSH access from any IP address. This opens your instance to the entire internet, which makes it more vulnerable to attacks.

💡 **Extra for PROs: Is using the default security group security best practise?**

We wouldn't do this for production environments or long-term use, but for now the default security group makes it easier to use EC2 Instance Connect.
If we don't use the default security group, we'd have to manually edit our instance's inbound rules to let in Instance Connect's range of IP addresses, which is different depending on your AWS Region.
Feeling up for a challenge? Try editing your security group settings to make it more secure (while letting you use EC2 Instance Connect). If you get stuck, ask the NextWork community!

### 2. Set up eksctl and your instance's AWS credentials

## eksctl

💡 **What is eksctl?**

`eksctl` is an official AWS tool for managing Amazon EKS clusters in your terminal. It's much, much easier to use compared to setting up a Kubernetes cluster using the AWS CLI!
If we were using the AWS CLI, we'd have to create a lot of other components manually before getting to deploy a cluster.

- **Command to install eksctl in my instance**

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv -v /tmp/eksctl /usr/local/bin
```

💡 **What do these commands do?**

The commands here download and install `eksctl` on your EC2 instance.

The first commands downloads the latest `eksctl` release from GitHub, then the second command moves it to a directory that lets you run `eksctl` from anywhere in your terminal.

Check that `eksctl` is installed correctly by running `eksctl version`. You should see the version number printed in the terminal.

```bash
eksctl version
```

### Create an IAM role for your EC2 Instance

- Head to the **IAM console**.
- Select **Roles**.
- Select **Create role**.
- Under **Trusted entity type**, select `AWS service` to tell AWS that we're setting up this role for a AWS service (Amazon EC2).
- Under **Use case**, select `EC2`.
- Select **Next**.
- Under **Permissions policies**, we'll grant our EC2 instance `AdministratorAccess`.
- Make sure the **AdministratorAccess** option is checked, and select **Next**.
- Let's give this role a straightforward name - `nextwork-eks-instance-role` role
- Enter a short description: **Grants an EC2 instance AdministratorAccess to my AWS account. Created during NextWork's Kubernetes project.**
- Select **Create role**.

💡 **Why do we need to set up an IAM role?**

*'Your EC2 instance starts off as a blank slate, so it doesn't actually have the permission to do anything in your AWS account yet! Your instance would fail to use eksctl to create any EKS clusters.
You need to give your instance the permission to work with AWS services, and you can grant this using roles.'*

💡 **Is granting AdministratorAccess best practice?**

*'Granting AdministratorAccess is powerful but not ideal for long-term use. We’re using it now because we don’t know all the services your EC2 instance will access for this project yet.
In a real-world scenario, you’d follow the principle of least privilege—giving user/services/apps just enough permissions for the job to minimize security risks.'*

⭐️ **Attach IAM role to EC2 instance**

- Head back to the Amazon EC2 console.
- Select Instances from the left hand sidebar.
- Select the checkbox next to your nextwork-eks-instance EC2 instance.
- Select the Actions dropdown, and then Security -> Modify IAM role.
- Under IAM role, select your new `nextwork-eks-instance-role role`.
- Select Update IAM role.

### 3. Create EKS Cluster

- Back to EC2 Instance Connect, run the following command to create your EKS cluster.

```bash
eksctl create cluster \
  --name nextwork-eks-cluster \
  --nodegroup-name nextwork-nodegroup \
  --node-type t2.micro \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 3 \
  --version 1.31 \
  --region YOUR-REGION
```

- Make sure to replace `YOUR-REGION` with your AWS region's code e.g. `us-east-1`:
- Creating your EKS cluster can take some time (around 15-20 minutes)

💡 **What does this command do?**

We'll learn all these terms over the rest of this project, but as a sneak peek, this command will:

- Set up an EKS cluster named `nextwork-eks-cluster`
- Launch a node group called `nextwork-nodegroup`.
- Use `t2.micro` EC2 instances as nodes.
- Start your node group with `3 nodes` and automatically scale between 1 (minimum) and 3 nodes (maximum) based on demand.
- Use `Kubernetes version 1.31` for the cluster setup.

💡 **Why do people use Kubernetes?**

Without a tool like Kubernetes, you would create and manage every container manually. You’d have to start each container yourself and keep an eye on them to restart any that crash.

Traffic to your app going up or down would mean turning containers on or off one by one, and you’d also have to make sure each container has access to storage if it needs it. Updating your app would mean carefully swapping out containers without causing downtime.

As you might imagine, managing hundreds or thousands of containers this way would be a huge amount of work and hard to get right all the time. Kubernetes takes care of all these tasks automatically, so you can focus on building your app's features instead.

### CloudFormation

💡 **What is CloudFormation?**

`CloudFormation` is AWS’s service for setting up `infrastructure as code`. You write a template describing the resources you need (like an instruction manual), and CloudFormation handles creating and configuring those resources.

`CloudFormation` helped create my EKS cluster by setting up all the required resources. It created VPC resources because it's easier than changing the default VPC configurations.

💡 **Why are we in CloudFormation?**

`eksctl` actually uses `CloudFormation` under the hood to create your EKS cluster.
When you ran the `eksctl` create cluster command, `eksctl` sets up a `CloudFormation` stack to automate the creation of all the necessary resources for the EKS cluster.

> In the Stacks page, notice that there is a new stack in progress! The stack should be called `eksctl-nextwork-eks-cluster-cluster`.

💡 **Why is there a second stack?**

The second stack is specifically for your `node group`, which is a `group of EC2 instances` that will run your containers.

`CloudFormation` separates the core EKS cluster stack from the node group stack to make it easier to manage and troubleshoot each part independently, especially if one half fails.

There was also a second CloudFormation stack for managing the node group. The difference between a `cluster` and `node group` is that the node group is part of the cluster ecosystem.

💡 **What are these Events about?**

The Events tab gives you a timeline of each action CloudFormation is taking to set up your resources. It’s a live update of what’s happening, which can be helpful for tracking progress or identifying any issues that come up during creation.

You can even select the Refresh button at the top of the panel to watch new updates come through on your stack's resources.

💡 **What are these resources?**

You might notice in your Resources tab all sorts of networking resources - VPC, subnets, route tables, security groups, NAT gateways and internet gateways!

These resources set up a private, secure network for your containers to connect with each other and the internet while keeping your app private. It's not required for this project that you understand all these components, but you can learn more about VPC resources at Launching VPC Resources.

Note that this is one of the big reasons why you'd pick eksctl over the AWS CLI - if we were using AWS CLI, we'd have to create each of these resources manually!

💡 **What's the difference between a cluster and a node group?**

Think of a cluster as the entire environment that Kubernetes manages for your containerized app. This cluster is made up of nodes (the servers that actually run your containers) and a control plane (the brain that decides things like when to create or shut down containers).

Within your cluster, the nodes are organized into node groups. Node groups let you manage multiple nodes more easily by grouping them together, so you can control settings like instance type and resource limits for the whole group instead of adjusting each node individually. This makes it much simpler to scale and configure nodes for specific tasks.

---

### The EKS Console

**Accessing the EKS Console:**

- In a new tab, open up the **EKS console**.
- Select the new `nextwork-eks-cluster` cluster you just created.
- Welcome to your EKS cluster's dedicated page!
- Select the **Compute tab**.
- Scroll down to the **Node groups** panel - aha, it's getting created!

> If it's been some time since you ran your create cluster command, you might even notice that it's already in Created status. Scroll back up to the top of your cluster's page. Notice that there are two banners in blue.

💡 **What are these two banners saying?**

The two blue banners in the EKS console are letting you know that while you have created a node group, you might not have the permission to see the nodes inside.

💡 **But my IAM User has AdministratorAccess - how could it not have permission?**

AWS permissions alone don’t automatically carry over to Kubernetes — Kubernetes has its own way of managing access within a cluster.

Even if you have AdministratorAccess in AWS, which grants full access to AWS resources, Kubernetes will only let you into different parts of the cluster if you have permissions under its own system too.

Let's learn about this system and grant ourselves access now!

### Set up your own access to your cluster

- Select **Create access entry** at the blue banner at the top of the page.

🙋‍♀️ **I don't see that top banner**

No problem! There's a second way to get to the same place:

- Head to the **Access tab**
- Under **IAM access entries**, select `Create access entry`.

💡 **What are IAM access entries?**

**IAM access entries** connect AWS IAM users with Kubernetes’ `Role-Based Access Control (RBAC)`, which is Kubernetes' system to manage access inside a cluster.

In short, your IAM Admin user (the one you're using to open this Management Console) needs a specific access entry to get access to the nodes in your EKS cluster.

Nice work, we can now set up an IAM access entry.

- Under **IAM Principal**, select your **IAM user's ARN**. Double check your IAM Admin's name at the top right corner and make sure it matches your ARN.
- Select **Next**.
- Under **Policy name**, select `AmazonEKSClusterAdminPolicy`.

💡 **What does this policy do?**

The **AmazonEKSClusterAdminPolicy** gives you full administrative rights over your EKS clusters. Your IAM user will be able to see and control all parts of your EKS cluster - including all the nodes inside!

- Select `Add policy`.
- Select **Next**.
- Select **Create**.
- Head back to your cluster's page.
- Select the **refresh button** on the console. You should now see your nodes listed under your node group.

💡 **Why are there three nodes but one node group?**

The `node group` is a group of nodes that share the same configuration, like instance type and scaling settings. In this case, our node group has three nodes, which means there are three EC2 instances that will run containers like a single unit.

---

### EXTRA: Deleting Nodes

Each node refers to an EC2 instance in this context. The term "node" is used as a generic designation, independent of any specific cloud provider.

Desired size is the number of nodes that I want my app to run. Minimum and maximum sizes are helpful for managing the number of nodes based on the demands of the app.

When I deleted my EC2 instances, new ones were automatically created. This happens because K8s detects the failure of the instances and initiates the deployment of replacements to ensure continued functionality.

---

## Project 2: K8s Deployment

### *Pull the Code for your Backend*

💡 **What is 'backend'?**

The `backend` is the **"brain"** of an application. It's how your app processes user requests and stores and retrieves data. In other words, the backend makes sure your app does what it's supposed to do (e.g. load a new page) when a user does things like clicking on buttons.
Unlike frontend code, which is what users see and interact with, backend code works on the server side so it runs in the background.

**In this step, you're going to:**

- Install Git.
- Make a copy of application code from GitHub.

💡 **What does cloning mean?**

When we say **"clone a repository,"** we mean creating a full copy of a repository's code stored remotely (in this case, in GitHub) to your machine (in this case, our EC2 instance).

This way, you get access to all the files and code without having to manually copy and paste.

> Git repository `https://github.com/NatNextWork1/nextwork-flask-backend`

- Select **Code** to reveal the instructions for cloning their repository.
- Copy the **HTTPS URL**.
- Install **Git**

```bash
sudo dnf update
sudo dnf install git -y
```

- Verify you've downloaded Git by checking for its version:

```bash
git --version
```

- Configure Git by running the command below.

```bash
git config --global user.name "userName"
git config --global user.email "myuseremail@mail.com"
```

- Clone the repository

```bash
git clone https://github.com/NatNextWork1/nextwork-flask-backend.git
```

💡 **What am I cloning?**

You are cloning the nextwork-flask-backend repository from GitHub. This repository contains all the backend code needed for your application, including files like app.py, Dockerfile, and requirements.txt.

By cloning this repository, you're copying all the code and resources onto your EC2 instance so you can build, run, and deploy the backend part of your project.

Once you've cloned it, the project will look like a new folder in your EC2 instance with the entire project's files inside.

- Run ls, which is the Linux command to list all files and subdirectories in your current directory.
- Confirm that you have a new folder called nextwork-flask-backend in your EC2 instance now.

💡 **Why does the repository name say 'flask'?**

The repository is named with flask because the backend code is built using Flask, a web framework for Python that developers use to create backend services or APIs quickly. It's called a "framework" because Flask comes with templates for building web applications, making it easier to create features like handling HTTP requests managing databases.

### *Build a Container Image for Your Backend*

💡 **What is a container image?**

A container image is like a blueprint that contains all the instructions, code, libraries, and dependencies needed to run the application. Note that on the other hand, a container is the running instance created from that image, bringing the application to life and running it in an environment.

In this step, you're going to:

- Build a container image of the backend.
- Resolve four installation and configuration errors 🤯

💡 **What does building an image mean?**

When your team member prepared the app's backend, they wrote a file called a Dockerfile and stored it inside the GitHub repository that they shared with you. A Dockerfile contains instructions on how to build a container image that packages up an app (in this case, the backend) and all its dependencies.

Building an image means you're creating a container image using the Dockerfile that your team member prepared. When you ran the docker build command, you were asking Docker to read the instructions in the Dockerfile and build the container image for you accordingly.

The container image lets Kubernetes set up multiple, identical containers so your application runs consistently across different environments. Whether you're deploying in development, testing, or production, your app behaves the same way because Kubernetes refers to your image each time a new instance/node needs to be created.

#### Install and Configure Docker

```bash
sudo yum install -y docker
```

💡 **What is Docker?**

Docker is the tool we're using to build a container image of our backend. In general, you'd use Docker to create containers and container images, while Kubernetes coordinate multiple containers (clusters) running the same/related applications.

**Start Docker:**

```bash
sudo service docker start
```

#### Build the Docker Image

- Run the command to build your container image again:

```bash
sudo docker build -t nextwork-flask-backend .
```

> Remember to use **sudo** for permissions so you won't get access denied --> connect: permission denied

The previous Docker commands you ran worked because they're prefixed with sudo, which lets a non-root user run commands with root user rights. But, it's good practise to give your ec2-user the permission instead of using sudo each time.

💡 **Why am I logged in as ec2-user instead of the root user?**

When you launch an EC2 instance using certain AMIs like Amazon Linux, the default user for SSH access is ec2-user. This user comes pre-set up with the AMI as a normal user that's allowed to have root user privileges if you run a command with sudo.

When you access your instance as ec2-user, it’s like logging into an AWS account as an IAM admin user—you’ve got a lot of control but not the absolute top level unless you're using sudo. If you need to run Docker commands, those require root privileges because Docker needs root-level access to create containers or build container images.

- To confirm which user you're using in the terminal, run the command `whoami`
- You should see ec2-user in the terminal response.

**Add ec2-user to the Docker group:**

```bash
sudo usermod -a -G docker ec2-user
```

💡 **What is the Docker group?**

The Docker group is a group in Linux systems that gives users the permission to run Docker commands. By default, only the root user can run Docker commands. When you add a user (e.g., ec2-user) to the Docker group, it lets that user run Docker commands without typing sudo every time.

💡 **What does this command do?**

The command `sudo usermod -a -G docker ec2-user` adds the `ec2-user` to the docker group.
*usermod* is used to modify a user's account in the system. It allows you to update attributes like their groups, home directory, login name, and more.
The -a flag (append) makes sure your user doesn't get removed from any other groups they might already belong to. Without -a, the user would be removed from all groups not listed in the command.
The -G flag (group) specifies the groups a user should be added to. In this case, it's the docker group.

> Restart your EC2 Instance Connect session by refreshing your current tab

💡 **Why are we refreshing our tab?**

User group changes don’t take effect until you start a new session, so reconnecting to your EC2 instance makes sure your ec2-user picks up the new docker group permissions.

Make sure your ec2-user has been added to the Docker group:

```bash
groups ec2-user
```

💡 **What does this command do?**

This command will list all the groups that ec2-user is a part of.
If you see docker listed, then ec2-user is in the Docker group and can run Docker commands without using sudo.

#### Push Your Container Image to Amazon ECR

Container registries, like Amazon Elastic Container Registry (ECR), are storage spaces for container images. They give container images somewhere to live so they can be accessed by Kubernetes/other services.

💡 **Recap: Why are we using Docker and ECR?**

We’re using Docker to package up the backend and then pushing that container image to Amazon ECR (Elastic Container Registry). When it's time for Kubernetes to deploy your application, Kubernetes can just pull the image directly from ECR.

This setup is the standard way for deploying containerized apps with Kubernetes (instead of deploying code on your local machine) because it helps with consistency and scaling.

**In this step, get ready to:**

- Store the Docker image of your backend in ECR.

- Create a new Amazon ECR repository using this command:

```bash
aws ecr create-repository \
  --repository-name nextwork-flask-backend \
  --image-scanning-configuration scanOnPush=true \
```

This response confirms that your ECR repository has been created - it's ready for you to push Docker images!

💡 Extra for PROs: Here's a breakdown of the terminal response:

**repositoryArn:** The Amazon Resource Name (ARN) i.e. unique ID for your ECR repository.

**repositoryUri:** This is the URL you'll use to push and pull container images. It shows where your images will be stored.

**repositoryName:** The name you've given to your repository—in this case, nextwork-flask-backend.

**imageTagMutability:** Whether image tags are mutable or immutable. "MUTABLE" means you can overwrite which image has a tag e.g. the latest tag can be taken by a newer image at any time.

**imageScanningConfiguration:** Whether images will be scanned for vulnerabilities when pushed.

**encryptionConfiguration:** Your images are encrypted using AES256 for security.

- In a new tab, head to the ECR console.
- Confirm that you can see a new repository called nextwork-flask-backend!
- Push your container image to ECR
- Select your new repository.
- Select View push commands.

💡 **What is a push command?**

A push command in Amazon ECR is used to upload your Docker container images to an ECR repository.

- Copy the first command.

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 752225465361.dkr.ecr.us-east-1.amazonaws.com
```

- Run the command in your EC2 Instance Connect window.

```bash
docker build -t nextwork-flask-backend .
```

- Head back to your ECR console's push commands window.
- We've already built out container image so we can skip the second command.
- Copy the last two push commands.

```bash
docker tag nextwork-flask-backend:latest 752225465361.dkr.ecr.us-east-1.amazonaws.com/nextwork-flask-backend:latest
```

```bash
docker push 752225465361.dkr.ecr.us-east-1.amazonaws.com/nextwork-flask-backend:latest
```

💡 **What do tagging and pushing the image do?**

Since your ECR repository can hold many versions of the same container image, tags help you keep things organized. Tagging your Docker image is like giving it a nickname so you can easily refer to a specific version.

Here, we're tagging our Docker image with latest so Kubernetes knows where it can find the right container image version when it’s time to deploy.

Pushing uploads the tagged image to a remote repository. In our case, we've just uploaded our container image to our ECR repository!

- Head back to the ECR console.
- Close the push commands window.
- Select the refresh button to refresh your console.

💡 **Does Kubernetes only run containerized applications from a repository?**

Nope, Kubernetes doesn’t have to pull container images from a remote repository like ECR or Docker Hub.

But using a container registry is a great way to deploy containerized apps. Your cluster can pull whatever is the latest image in your repository on demand, which makes deployments stay consistent across all nodes automatically.

If you didn't use a container registry, you’d need to preload every node in your Kubernetes cluster with the image. You'd also need to update each node manually with every change to your container image.

💡 What does this Dockerfile do?
The Dockerfile defines how a Docker image of the backend should be built.

Notice the contents of this file and how it builds your Docker image step-by-step.

When we talk about the benefits of using container images and containers—such as consistent, reliable deployment across different environments—these commands in a Dockerfile are exactly what make the magic happen!

It installs the necessary dependencies, copies the application code, and sets up the commands a container needs to run to start your Flask app.

💡 **Can you break down the commands in this Dockerfile?**

```Dockerfile
FROM python:3.9-alpine sets up the environment for your app by using Python 3.9 on Alpine Linux, which is a very lightweight version of Linux. This keeps your app size small, making it faster to build and run.



LABEL Author="NextWork" "adds author metadata to the image."



WORKDIR /app "sets /app as the working directory in the container, so the commands in the rest of this Dockerfile will be run from there."



COPY requirements.txt requirements.txt "copies the requirements.txt file from your local machine (i.e. your EC2 instance) into the container's /app directory."



RUN pip3 install -r requirements.txt "installs the dependencies in requirements.txt."



COPY . . "copies all the files from your current directory i.e. requirements.txt and app.py into the /app directory in the container."



CMD ["python3", "app.py"] "says the container should run the command python3 app.py to start your Flask app."
```

💡 **What is an API?**

An API (Application Programming Interface) is like a bridge that lets different programs talk to each other. It lets one app to ask for data or services from another and get a response.

For example, when our backend app runs a search using the Hacker News Search API, it sends a request, gets data back, and processes it into JSON format to return to the user. This helps different parts of software work together easily.

In our own backend app, we also create our own API using Flask. This API takes user input, connects to the Hacker News API to get the data, processes that data, and then sends it back as JSON.

APIs make it easy for our backend to collect data from Hacker News and then share information back to our users and other services.

> Delete your AWS resources

```bash
eksctl delete cluster --name nextwork-eks-cluster --region us-east-1
```

## Project 3: Create Kubernetes Manifests

With our image safely stored in ECR, we're ready to deploy our application to our EKS cluster. We'll use Kubernetes manifests to tell Kubernetes how we want it to deploy our application.

💡 **What are Kubernetes manifests?**

Just like a Dockerfile tells Docker how to build a container image, Kubernetes manifest files tell Kubernetes how to run your application in a cluster.
They act as blueprints that define the desired state of your app, including which container images to use and how to make your app available to end users or other services.

### Create Kubernetes Manifest Files

- Create a new directory from your instance's root called manifests. Run the following commands:

```bash
cd ..
mkdir manifests
cd manifests
```

💡 **What are Kubernetes manifests?**

Think of a Kubernetes manifest as a set of instructions that tells Kubernetes how to run your app.
Manifests are important for deployment because Kubernetes uses this information to know what your app needs, like which containers to run, how many copies to create, and how much memory to allocate.
Without manifests, Kubernetes wouldn’t know how to set up and manage your app automatically. This means you'd have to manually configure each container every time you deploy, which would be confusing, error-prone, and hard to repeat. Manifests make deployment simple, clear, and consistent.

**Tip:** In general (even outside of Kubernetes), a manifest is an instruction/manual that tells a system how to set up and manage something.

💡 **What was the command I just ran?**

The commands you ran did three things:

- `cd ..` navigates the terminal out of the amazon-eks-backend folder and back to the root.
- `mkdir manifests` creates a new directory called manifests.
- `cd manifests` navigates the terminal into the newly created manifests directory.

- Create the flask-deployment.yaml file:

```bash
cat << EOF > flask-deployment.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nextwork-flask-backend
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nextwork-flask-backend
  template:
    metadata:
      labels:
        app: nextwork-flask-backend
    spec:
      containers:
        - name: nextwork-flask-backend
          image: YOUR-ECR-IMAGE-URI-HERE
          ports:
            - containerPort: 8080
EOF
```

💡 **What is a Deployment manifest?**

When you deploy an app with Kubernetes, Kubernetes' job is to set up and manage several copies of your app across different containers in the cluster.
A Deployment manifest tells Kubernetes exactly how to manage these tasks. It includes details like the how many copies of your app should run across your cluster, and which settings to apply (e.g. CPU limits, container image, or network settings).
We’re only deploying the backend of our app, but a Deployment manifest can manage multiple components, like frontend servers, databases, and other services; each with its own specific configuration.

💡 **How does this command create the Deployment manifest?**

The cat command in Linux is a quick way to see what's inside a file or create a new one without opening a text editor.
When you use cat with << EOF, like cat << EOF > flask-deployment.yaml, you're telling the system: "Take the content I type here and save it in a new file called flask-deployment.yaml."

- Run `vim flask-deployment.yaml` in your terminal to see your work.

💡 **What does the containers section of this file mean?**

The containers section tells Kubernetes how it should set up each container that will run inside the pods created by this deployment. It includes:

- The name of the container.
- The container image to use.
- The ports that the container will use, which lets the application communicate within the cluster and with external traffic.

💡 **What should I be replacing 'YOUR-ECR-IMAGE-URI-HERE' with?**

You should replace YOUR-ECR-IMAGE-URL-HERE with the URI of the Docker image you pushed to Amazon ECR. This lets Kubernetes know where to pull the container image from when it deploys your application.

> Change this part `image: YOUR-ECR-IMAGE-URI-HERE` with your actual image uri.

It would look like this `image: 752225465361.dkr.ecr.us-east-1.amazonaws.com/nextwork-flask-backend:latest`

### Create your Service Manifest

**In this step, you're going to:**

- Create the Service manifest, which tells Kubernetes how to expose your application and route traffic to it.

**Create the flask-service.yaml file:**

```bash
cat << EOF > flask-service.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: nextwork-flask-backend
spec:
  selector:
    app: nextwork-flask-backend
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
      protocol: TCP
EOF
```

💡 **What is a Service manifest?**

A Service in Kubernetes is like a traffic controller that makes sure traffic gets to the right pods inside your app.
When you create a Kubernetes Service, you're essentially exposing your application i.e. making it accessible to network traffic (from within the cluster or from external sources).
A Service manifest tells Kubernetes to create/update a Service using details like which pods to route traffic to, the type of traffic it should handle, and which ports should be used.
In our case, the Service manifest we created sets up a Service that routes external traffic to port 8080 on pods labeled app: nextwork-flask-backend. This allows traffic from outside the cluster (e.g., your users) to reach your app!

💡 **What's the difference between the the Deployment and the Service manifests?**

The Deployment manifest focuses on deploying and managing your app inside Kubernetes, while the Service manifest is what you use to expose your app to the outside world or other parts of your system. Both work together to create a fully functional application setup.

### Annotate your Deployment Manifest

By annotating your deployment manifest, you'll gain a much deeper understanding of Kubernetes architecture and how Kubernetes deployments work.

```bash
vim flask-deployment.yaml
```

`apiVersion: apps/v1 line` # Specifies the API version for this Deployment

💡 **What is apiVersion?**

**apiVersion** tells Kubernetes which version of its API you’re using to create this resource. Since we’re defining a Deployment, we use `apps/v1`, which is the version for managing applications.

💡 **What's an API?**

An API (Application Programming Interface) is a way for different software components to communicate with each other.
In Kubernetes, the API is how you tell Kubernetes what resources to create, update, or delete.
The `apps/v1` API lets Kubernetes know you’re working with applications. Other options include security policies (policy/v1), storage resources (storage/k8s.io/v1) and more.

💡 **What does the # do?**

The **#** in a YAML file is used to add comments. Anything written after **#** is ignored by Kubernetes and is not processed as part of the manifest.
Comments are useful for adding explanations, reminders, or notes about what each part of the manifest does. This helps you (and your team) understand the file better when you revisit it later.
In this line, we’re using a comment to explain the purpose of the apiVersion line.

`kind: Deployment` # This is a Deployment object

💡 **What is kind?**

**kind** specifies the type of Kubernetes resource we’re creating, such as a `Deployment` or a `Service`.
**kind** is a key part of a Kubernetes manifest because it sets the tone for how Kubernetes will process the rest of the file. For example, if the **kind** is `Deployment`, Kubernetes will focus on managing and scaling groups of containers. If the **kind** is `Service`, Kubernetes will set up networking to expose those groups of containers.

💡 **What is metadata?**

**metadata** holds details about the resource, like its name and labels to help organize it.
`name` is the unique name we’re giving to this `Deployment`. Other **metadata** fields that could get added in a `Deployment` manifest are `labels` (e.g. if you wanted to note that this Deployment manifest is for the production environment, you could add an `environment`: production label), descriptions, or a unique ID you'd like to assing it.

💡 **What is namespace?**

A `namespace` is like a virtual "folder" within a Kubernetes cluster. All resources in Kubernetes belong to a `namespace`, and namespaces help you organize resources by separating them into different groups.
In this example, the `Deployment` is placed in the default `namespace`, which is the built-in, catch-all namespace for resources that don’t belong to a specific group.

**Extra for PROs**: In real-world examples, you could use namespaces to split resources across teams or environments i.e. `'dev'`, `'staging'` and `'prod'` **namespaces**.

💡 **What is spec?**

`spec` stands for **specification**. This section is the “blueprint” for how Kubernetes should set up and manage this Deployment.
The `spec` section tells Kubernetes things like what type of containers it should use, how many groups of containers it should be running, and any special configurations it needs.

> **Importantly, the Deployment uses spec to define the desired state of your application, and Kubernetes will constantly work to maintain that state.**

💡 **What are replicas?**

`Replicas` are identical copies (or instances) of the app Kubernetes should run. Here, we’re asking for 3 replicas, which means 3 identical `Pods` will be created.
The `Deployment` is responsible for making sure that if one Pod crashes, a new one is created to maintain 3 replicas. Don't forget that anything under the spec section defines a desired state that Kubernetes will always maintain!
You can scale up replicas to handle increased traffic or scale down to save resources during low-traffic periods.

💡 **What is a Pod?**

While we've been thinking about a Kubernetes cluster as a bunch of nodes running your containerized applications together, you can zoom in and break down a node even further... introducing `Pods`!
In Kubernetes, `Pods` are **groups of containers** that are bundled together so they can work as a unit.
`Pods` are the **smallest things** you can deploy - you can't deploy containers on their own; they must be inside a Pod. In more technical terms, this means `Pods` are the **smallest deployable units in Kubernetes.**
> **Containers in a Pod share the same network space and storage so they can communicate and share data more efficiently.**

💡 **What is selector?**

The `selector` acts like a **filter**. Kubernetes looks for `Pods` with `labels` that match the filters you define here, and only the `Pods` that match all of them will be a part of this Deployment.
`matchLabels` matches `Pods` with a specific `label` (app: nextwork-flask-backend). Without matchLabels, Kubernetes wouldn’t know which Pods belong to this Deployment!

💡 **What is template?**

The `template` is the blueprint for the `Pods` that this Deployment will create. It defines exactly how each Pod should look and behave.

💡 **What is labels?**

`labels` are tags that help you organize and identify `Pods`. Here, every Pod will be labeled with app: `nextwork-flask-backend`.
**Extra for PROs:** Remember matchLabels from the previous section? Note that the label created in this section is exactly the same as matchLabels!
When a Deployment creates new Pods, it uses the template section to define the labels for those Pods. If the labels match the matchLabels filter, Kubernetes automatically links those Pods to the Deployment.
Kubernetes uses this match to monitor and enforce the desired state of the Deployment. For example, if a Pod crashes or is removed, Kubernetes will create a new Pod with the matching labels to replace it.
If the matchLabels and labels don’t align, the Deployment will try to create and maintain the desired number of Pods, but it won’t manage them correctly because it can’t identify them.

💡 **What does spec mean here?**

You might notice that there was already a spec section earlier in this manifest!
In a Kubernetes Deployment manifest, the two spec sections serve different purposes, because they work at two different levels:

1. The first spec earlier in this file tells Kubernetes what to do with the Deployment as a whole. It defines things like the number of replicas (Pods) in the Deployment, or which Pods the Deployment should manage (via selector). In other words, it works at the Deployment level.

2. The spec you see in this template section is the blueprint for the individual Pods that the Deployment will create. It defines what each Pod is running, so it works at the Pod level.

💡 **What's the difference between the Pod level spec and template?**

The `template` is the overall blueprint for Pods, think of it as an "outer envelope" that packages the Pod level spec together with metadata like labels and names.
The `Pod-level spec` focuses only on the containers and operational details of the Pods themselves, such as what’s running inside them and how they behave.

```yaml
    spec:
      containers:
        - name: nextwork-flask-backend
          image: YOUR-ECR-IMAGE-URI-HERE
```

💡 **What is containers?**

`containers` define the actual applications running inside each Pod:

- **name** is the name of the container.
- **image** is the container image that Kubernetes will use to run the app.

- The last bit of the containers section is all about ports

```yaml
          ports:
            - containerPort: 8080
```

💡 **What is ports?**

A `port` is like an **entry point** that allows data to enter or leave an application running inside a container. A container can have many different ports, and each port handles a specific type of traffic.
In other words, if your container is a house, the ports are doors, and each door leads to a specific app (or part of an app, e.g. the backend). In Kubernetes, ports allow Kubernetes to map incoming traffic to the correct container inside a Pod.
Typically, a Deployment manifest might have more ports if the app handles multiple types of traffic (e.g., one port for HTTP requests and another for secure HTTPS).

💡 **What is containerPort?**

`containerPort` is the port number used by the app inside the container to send and receive traffic.
Here, our backend listens on port 8080, which means it’s ready to receive requests sent to that port. Requests need to specify the correct port to reach the app. If the port is incorrect or missing, the app won’t receive the request.

💡 **Extra for PROs: How does a request end up at the right port?**

Requests are routed to the correct port through a combination of DNS, port numbers, and Kubernetes Services. Here's how it works:

- **DNS and IP addressing:**

When you visit a website or make a request, DNS translates the domain name (e.g., example.com) to the server's IP address. This makes sure your request is sent to the correct server.

- **Port numbers in the request:**

The client (e.g., your browser or API tool) specifies the port number in the request.

 1. Example: `http://example.com:8080` explicitly tells the system to send the request to port 8080.
 2. If the port is omitted (e.g., `http://example.com`), a default port is used (e.g., 80 for HTTP or 443 for HTTPS).

- **Routing and Services in Kubernetes:**

In Kubernetes, Services are responsible for directing traffic to the correct application too.
Even if external traffic arrives on a different port, you can set up the Service to forward it to the correct containerPort inside the container.

> Notice how as we move down the Deployment manifest file, we’re essentially peeling back layers of Kubernetes’ architecture, going from the API level all the way down to the container level.

1. At the very top of the file, we’re interacting with the Kubernetes API, which is the interface that lets us define resources. At this level, we’re telling Kubernetes, “Hey, I want to create a Deployment using this version of the API.”

2. Then, we’re looking at the big-picture configuration for the entire Deployment. How many Pods we need (via replicas), which Pods to manage (via selector), and how Kubernetes should maintain them. The Deployment makes sure Kubernetes maintains the desired state of your app by watching over the Pods it manages and recreating them if they fail.

3. As we move deeper, we reach the template section, which defines the blueprint for the Pods the Deployment will create.

4. Finally, we reach the core of each Pod: the containers. This is where the actual application (in this case, our backend) lives and runs. Our containers section defines the image and port for the app inside the Pod.

### Delete the EKS Cluster (this is not part of the free tier)

```bash
eksctl delete cluster --name nextwork-eks-cluster --region YOUR-REGION
```

- Replace `YOUR-REGION` with your actual region, for example `us-east-1`

## Project 4: Deploy Backend with Kubernetes

**In this step, you're going to:**

- Install kubectl, a command-line tool for using Kubernetes.
- Deploy your app with Kubernetes.

💡 **What is kubectl?**

`kubectl` is the command-line tool for interacting with Kubernetes resources (e.g. , Deployment or Service resources) once your cluster is up and running. We're using it to apply our manifests and deploy our application.

💡 **Don't we already have another tool called eksctl?**

Good question! `eksctl`, which you installed in Step #1 of this project, is great for setting up and deleting your EKS cluster and configuring its settings.
But, when it comes to deploying applications and managing resources within the cluster, `kubectl` is the tool to use.

- **Install kubectl**

```bash
sudo curl -o /usr/local/bin/kubectl \
https://s3.us-west-2.amazonaws.com/amazon-eks/1.31.0/2024-09-12/bin/linux/amd64/kubectl
```

- Just like what you did with Docker, you'll need to give yourself the permission to use kubectl:

```bash
sudo chmod +x /usr/local/bin/kubectl
```

- Check you've installed kubectl properly:

```bash
kubectl version
```

- **Apply the manifests:**

```bash
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
```

💡 **What does it mean to 'apply' manifests?**

Now that you've set up the manifest files, this command tells `Kubernetes` to create or update resources (i.e. the `Deployment` and `Service`) based on the instructions in your manifest files.
Since this is our first time running apply, `Kubernetes` will create the `Deployment` and `Service` resources in the cluster.
The next time you run the same apply command with updated manifests, `Kubernetes` will recognize that these resources already exist and will update them to match the new configurations.

- **In a new tab, open up the EKS console.**

- Select the nextwork-eks-cluster cluster you've created for this project.
- Welcome to your EKS cluster's dedicated page!
- Select the Compute tab.
- Scroll down to the Node groups panel - nice, your node group is here.
- Scroll back up to the top of your cluster's page. Notice that there are two banners in blue.

💡 **What are these two banners saying?**

The two blue banners in the EKS console are letting you know that while you have created a node group, you might not have the permission to see the nodes inside.

💡 **But my IAM User has AdministratorAccess - how could it not have permission?**

AWS permissions alone don’t automatically carry over to Kubernetes — Kubernetes has its own way of managing access within a cluster.
Even if you have AdministratorAccess in AWS, which grants full access to AWS resources, Kubernetes will only let you into different parts of the cluster if you have permissions under its own system too.

- **Set up access to your cluster nodes**

> To give yourself access to your cluster, we went through how to set up IAM access policies in the EKS console in the first project of this Kubernetes series.

This time, we'll use terminal commands instead for a faster, more efficient way to grant access.

- Head back to your EC2 Instance Connect tab.
- Use the following command to give yourself access to your Kubernetes clusters
- Don't forget to replace [USER_ARN] and [YOUR-REGION] with your actual IAM User ARN:

```bash
eksctl create iamidentitymapping --cluster nextwork-eks-cluster --arn [USER_ARN] --group system:masters --username admin --region [YOUR-REGION]
```

🙋‍♀️ **Where's my IAM User ARN?**

- In a new tab, visit the IAM console.
- From the left hand sidebar, select Users.
- Select the IAM User you've logged into for today's project. If you're not sure about your IAM User's name, check the top right corner of your console.
- Copy your IAM User's ARN from the Summary panel.
- Select the refresh button on the console. You should now see your nodes listed under your node group.

💡 **Why are there three nodes but one node group?**

The node group is a group of nodes that share the same configuration, like instance type and scaling settings. In this case, our node group has three nodes, which means there are three EC2 instances that will run containers like a single unit.

- Click into one of the nodes to view its details.
- Scroll down to the Pods section.

💡 **What is a pod?**

While we've been thinking about a `Kubernetes cluster as a bunch of nodes` running your containerized applications together, you can actually zoom in and break down a node even further... introducing `pods`!
In `Kubernetes`, `pods` bundle containers together so they can work as a unit. `Pods` are the smallest things you can deploy - you can't deploy containers on their own; they must be inside a `Pod`. In more technical terms, we'd call `pods` the smallest deployable units in `Kubernetes`.
> Containers in a pod share the same network space and storage so they can communicate and share data more efficiently.

- Select the pod that starts with demo-flask-backend-xx.
- Check out the Events section to see the latest updates for this pod.

💡 **What do these events mean?**

These events show the steps that happened when Kubernetes was creating your pod:

Here's what each step means:

1. Successfully assigned default/demo-flask-backend...: *Your pod has been given an internal IP address in the cluster.*
2. Pulling image...: *The pod is getting your container image from Amazon ECR.*
3. Successfully pulled image...: *The image was successfully downloaded to the cluster.*
4. Created container demo-flask-backend: *The container was built from your image and prepared for launch.*
5. Started container demo-flask-backend: *The container has been successfully started and is now running in your pod.*

💡 **What does assigning an image to an IP mean?**

This means a container, built from the container image your pod pulled, is now deployed and running at an internal IP address inside the cluster.
In simpler terms, your backend is up and working and can be accessed within the cluster's network.

## Delete EKS Cluster

- Delete your EKS cluster within the terminal this time (replace [YOUR-REGION] with your region's code):

```bash
eksctl delete cluster --name nextwork-eks-cluster --region us-east-1
```

> This command deletes the EKS cluster and all associated resources! It takes a bit of time before everything's gone, so we can move on to other resources.

- Terminate EC2 Instance
  - Head to the EC2 console.
  - Select the checkbox next to nextwork-eks-instance.
  - Select Instance state -> Terminate (delete) instance.

- Delete ECR Repository
  - Head in the AWS console.
  - Select the nextwork-flask-backend repository.
  - Click Delete and confirm.

💡 **Please check - is your EKS cluster removed? Is everything in the CloudFormation stack deleted?**

Please also check for an `Elastic IP address in the EC2 console` - some students have been charged by the Elastic IP still being in their account!
