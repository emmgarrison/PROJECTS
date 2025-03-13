<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Create S3 Buckets with Terraform

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-terraform1)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_9i0j1k2l)

---

## Introducing Today's Project!

In this project, I will demonstrate;
-Install and configure Terraform.
-Configure your AWS credentials in the terminal.
-Create and manage S3 buckets with Terraform.
-Upload files to S3 using Terraform.

The goal is to learn automation -a key factor to staying efficient in how I develop and deploy code. It's how DevOps teams cut out the manual work of creating new resources in the cloud environment, which reduces the chances of errors.

### Tools and concepts

Services I used were Terraform, AWS CLI, IAM, S3... Key concepts I learnt include infrastructure as code and AWS CLI, which is a (Command Line Interface) tool that lets me manage my AWS services from my local terminal. Instead of having to use the AWS Management Console, I can now run text commands from my local machine, so that Terraform can create AWS resources from my local computer. 

### Project reflection

This project took me approximately 8 hours...The most challenging part was when I was installing Terraform on my windows OS.

Also when I ran the "Terraform plan" command--I got an Error: No valid credential sources found...because Terraform doesn't have the necessary credentials to access my AWS account.

I also ran into an error when I ran the "terraform apply" command...Error: creating S3 Bucket (nextwork-unique-bucket-EmmanuelGarrison-2025): operation error S3: CreateBucket, https response error StatusCode: 400, api error InvalidBucketName: The specified bucket is not valid.
I troubleshooted, researched and resolved all issues successfully.

It was most rewarding to validate that I've updated my configuration successfully, by navigating back to my AWS Management Console and S3 console. Refreshed my bucket's page and voilaa..the image is there. I then downloaded the image and compared it to the original image and YES they match perfectly.

I did this project to be equipped with hands-on practical and most in-demand skills in the market. Also to understand the concepts and build complex solutions/applications that integrate more tools such as Kubernetes, Terraform, S3, AWS, Jenkins, Ansible, EKS, eksctl, ECR, kubectl, Docker, Github, nano, EC2, IAM, Dockerfile, Linux, Bash and multi-cloud architectures to solve real world problems that companies face.

---

## Introducing Terraform

Terraform is a tool that helps you build and manage your cloud infrastructure using code.

Instead of setting up resources manually in the AWS console or CLI, you write a script that tells Terraform exactly what you want in your cloud infrastructure, like servers, databases, and networks. Terraform then automatically builds all of this for you, using your script as a blueprint.

Terraform is one of the most popular tools used for infrastructure as code (IaC), which is a way to manage your IT infrastructure. Instead of manually setting up your resources e.g. creating resources one by one in the Console, you're writing configuration files in code.

Things built from code are built the same way every time. You can repair and rebuild things quickly, and other people can build identical instances of the same thing. This makes managing large-scale systems more efficient, less error-prone, and way faster than doing everything manually.

Terraform is a top cloud agnostic IaC tool because it supports multiple cloud providers, so you can set up multi-cloud infrastructures that use AWS, Azure, GCP and more. It also uses a simple configuration language.

Terraform uses configuration files to define and manage infrastructure. These files describe the desired state of your infrastructure, and Terraform figures out how to achieve that state.

main.tf is a central file in a Terraform project. It's where I write down what I want my infrastructure to look like, using Terraform's language. It's the blueprint for building my cloud resources.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_9i0j1k2l)

---

## Configuration files

The configuration is structured in blocks to help organize the code better. Each block handles a specific piece of my setup, like a resource or a configuration. 
The advantage of doing this is that, it makes it easier to read, manage, and adjust things separately without affecting the rest of my infrastructure. The style of organizing code into individual blocks is called modularity.

### My main.tf configuration has three blocks

The first block (Provider blocks) indicates which plugins (like AWS, Google Cloud, or Azure) Terraform uses to deploy and manage resources in those services.

The second block (Resource blocks) provisions what resources to manage, such as networks, virtual machines, or storage buckets. Each resource is individually managed and can be customized with parameters defined by the provider. 

The third block manages public access policies for the S3 bucket.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_ljvh9876)

---

## Customizing my S3 Bucket

For my project extension, I visited the official Terraform documentation to find and read documentation about aws provider, setup instructions, descriptions of each available resource, best practice tips and examples, and parameters for customization.

The documentation shows how to use Terraform to create and manage AWS S3 buckets. It covers how to configure these buckets, set their properties, and manage interactions with other AWS services.

I chose to customise my bucket by adding a Tag for the environment this s3 bucket will be deployed in...which is Production...because...I want to easily identify where in AWS the s3 bucket will be created....Dev, Test or Prod. When I launch my bucket, I can verify my customization by looking at the bucket's name tag.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_ffe757cd3)

---

## Terraform commands

I ran 'terraform init' as the first command you run to initialize a new Terraform project, meaning it sets up my project by:
-Downloading necessary plugins
-Setting up the backend
-Preparing modules
-Creating a lock file

Next, I ran 'terraform plan' to create an execution plan, showing me what changes Terraform will make to my infrastructure based on my main.tf configuration file...so I can review and confirm the configuration before any real changes are made.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_3g4h5i6j)

---

## AWS CLI and Access Keys

When I tried to plan my Terraform configuration, I received an error message that says Error: No valid credential sources found...because Terraform doesn't have the necessary credentials to access my AWS account.

To resolve my error, first I installed AWS CLI, which is (Command Line Interface) tool that lets me manage my AWS services from my local terminal. Instead of having to use the AWS Management Console, I can now run text commands from my local machine.

I'm using the CLI in this project so that Terraform can create AWS resources from my local computer.

I set up AWS access keys to enable CLI to be authenticated and authourized to connect to my AWS Management Console.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_7j8k9l0m)

---

## Lanching the S3 Bucket

'I ran 'terraform apply' to apply the changes I've written in my Terraform main.tf configuration. It creates, modifies, or deletes resources in my infrastructure based on my code. In my case, this command creates the S3 bucket in my AWS account.

The sequence of running terraform init, plan, and apply is crucial because if I run terraform apply before terraform init, I would run into an error because terraform init needs to set up my project first by downloading necessary plugins and setting up the state file, which is a file Terraform uses to track the current state of my infrastructure.

There actually aren't any errors if I skip terraform plan. This is because terraform apply will automatically run a plan and ask for confirmation before making any changes. But, skipping the explicit terraform plan means I might miss reviewing these changes in detail. It's best practice to run a plan to catch any potential errors or unexpected results before they affect my live infrastructure.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_1q2w3e4r)

---

## Uploading an S3 Object

I created a new resource block to upload an image file to my new S3 bucket using Terraform.

I need to run terraform apply again because anytime I change my Terraform configuration, this will make sure the changes are correctly applied to my infrastructure.

To validate that I've updated my configuration successfully, I navigated back to my AWS Management Console and S3 console. Refreshed my bucket's page and voilaa..the image is there. I then downloaded the image and compared it to the original image and YES they match perfectly.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-terraform1_9o0p1a2s)

---

---
