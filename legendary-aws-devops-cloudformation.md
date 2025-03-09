<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Automate with CloudFormation

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-cloudformation)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Deploy an IaC Template with CloudFormation

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_bd8b836b)

---

## Introducing today's project!

### What is AWS CloudFormation?

AWS CloudFormation is a service that Developers/DevOps engineers automate the modeling of resources needed for the deployment of applications using Infrastructure as a Code (IaC) tool to generate templates and stacks, in a single JSON or YAML file.

Stacks are the compilation of all the resources you need to deploy your application in a single YAML configuration file bound together with roles and policies. So I guess we should try to familiarise ourselves with writing YAML files and mastering it's indentations.

Developers/DevOp Engineers use AWS CloudFormation because it automates AWS resource modeling, ensures consistency, and less risk of human errors at lightning speed, saving time. CloudFormation definately makes infrastructure/resource modeling repeatable & efficient.

### How I used CloudFormation in this project

I used AWS Cloudformation to automate the modeling of resources needed for deployment of my application such as IAM roles, Policies, CodeBuild project, CodeArtifact domain, CodeArtifact repostiory, S3 Artifacts bucket and Establish connection to Github repository.

### One thing I didn't expect in this project was...

One thing I didnʼt expect in this project was debugging and troubleshooting complexity regarding several errors I encountered, such as invalid template resource property 'CodeBuildProject' error and Circular Dependency error.

To resolve that, I spent countless hours in mastering writing YAML file configurations and it's indentations. 
I also researched about using CodeConnections to GIthub and Resource dependencies -to ensure I created secured connections and proper resource sequencing to avoid conflicts and ensure successful deployment.

### This project took me...

3 days

---

## CloudFormation templates

A CloudFormation template is a stack of AWS resources stacked together in a JSON or YAML file format. 

No more clicking through your AWS Management Console to create every resource one at time - with CloudFormation, you can implement AWS
resources and configurations needed to automate infrastructure setup for your application in a code format.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_0495b046)

---

## IaC generator

I created a CloudFormation template using the IaC generator, a feature that scans through all my resources in AWS estate, collects everything and saves it to get ready for templating....with a strict Deletion and Update policy set to "Delete" for resource management...that instructs what should happen to the AWS resources in this template when the stack is deleted or if they need to be replaced during a stack update.

### Not all resources could be added to my template

The resources I couldn’t add to a template were VSCode and Github. The IaC generator can't include them as they requrie specific configuration details and security permissions that our generator might not handle automatically.

The resources that I could add to my template includes;
-CodeArtifact domain called nextwork
-local CodeArtifact repository i.e. nextwork-packages
-upstream CodeArtifact repository i.e. maven-central-store
-IAM Policy for CodeArtifact i.e. codeartifact-nextwork-consumer-policy
-IAM Service Role for CodeBuild i.e. codebuild-nextwork-web-build-service-role
-S3 bucket i.e. nextwork-build-artifacts-yourname

---

## Manually adding resources

After downloading the generated template, I manually defined two more resources:
Github repository and my CodeBuild project.


I had to manually define these because they CloudFormation still requires them once I've deployed them.

I also had to make sure the references were consistent in this template, so I made two edits:

-Replace the CodeBuildServiceRole placeholder with the right reference...with CodeBuild service role's configuration name.

-Replace  the Location of the ArtifactsBucket reference...with the configuration name of your S3 bucket!

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_1cee0428)

---

## Testing my template

Before testing my template, I had to:
Delete the resources that overlap with the resources described in my template, which are:
-CodeCommit repository
-2x CodeArtifact repositories
-CodeArtifact domain
-CodeBuild project
-S3 artifacts bucket
-IAM role for CodeBuild codebuild-nextwork-web-build-service-role
-3x IAM policies for CodeBuild i.e. policies starting with CodeBuildBasePolicy-nextwork-web-build, CodeBuildCloudWatchLogsPolicy-nextwork-web-build and codeartifact-nextwork-consumer-policy

CloudFormation's deployment will fail if a resource with the same name already exists -since the resources in the template and the existing resources in your account share the same name.

A stack is a compilation of all the resources I need to deploy my application in a single YAML configuration file bound together with roles and policies.

The result of my first template test was: invalid template resource property 'CodeBuildProject' error.

---

## Unpacking the first error

My first template test failed because of improper indentation of the Codebuild project section of the yaml configuration template...it was nested within the IAM role for Codebuild configuration resource.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_f56730fd)

### To fix this error, I edited my CloudFormation template.

I added the line: DependsOn: "IAMRole00codebuildnextworkwebbuildservicerole00TaGrO"

---

## Fixing the first error


The DependsOn attribute means the creation of a resource depends on another resource being created first...

DependsOn: "IAMRole00codebuildnextworkwebbuildservicerole00TaGrO"

Where "IAMRole00codebuildnextworkwebbuildservicerole00TaGrO" is the name of the IAM role in the template.

Meaning that CloudFormation will wait/pause the execution/creation of the specific policy/resource/configuration...until the
specified IAM role is fully created before attempting to attach any policies. 
This prevents errors due to resources not being available when policies are being set.

The DependsOn line was added to four different parts of my template three IAM policies to ensure they depend on the IAM role, and the CodeBuild resource project itself to ensure it depends on the IAM role setup as well.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_f0df8018)

---

## Second template test

I gave my CloudFormation template another test! But this time, I couldn’t create the stack because of an error. This is known as a "circular dependency" error.

This error means CloudFormation template is confused because parts of it are stuck in a loop—policies say they need the IAM role to be created first, but IAM role seems to be saying it also needs those policies in place before it can be created. It's like each part is waiting for the other to show up first, and CloudFormation doesn't know what to do!. chicken and egg situation.

To fix this error, I navigated to the template's configuration of IAM role and removed the ManagedPolicyArns sub-section with all it's references

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_e6fd85ed)

---

## My final template test 👏

### In my final test, creating the new stack was a great success

I could verify all the deployed resources by visiting the Resources tab, and select on the hyperlinks for all the resources that have a shortcut to the created resource.

Not all the resources in the list had a shortcut URL, because those resources (e.g. S3 artifacts bucket, CodeBuild project) have been deployed at a specific region, CloudFormation can't produce the same shortcut link to take you there.

Nevertheless, I verified the resource by navigating directly to the resource console. For example I verified my CodeBuild project in the CodeBuild console.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-devops-cloudformation_bd8b836b)

---

---
