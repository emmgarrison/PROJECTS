<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_1c864649)

---

## Introducing today's project!

### What is AWS IAM?

IAM users are used to represent individuals (like developers or administrators or interns) that need to access AWS console resources...through authentication and is assigned specific permissions to perform actions. 

### How I'm using AWS IAM in this project

In this project, I created IAM user and user group which is just a collection/folder of IAM users. I also configured an IAM policy using JSON and I attached the policy to this user group, which means it allows me to manage permissions for all the users in my group at the same time rather than individual users.

### One thing I didn't expect...

The IAM Policy Simulator is definitely a game changer in the area of testing user access permissions/policies for developers, engineers and administrators in both dev and prod environments.

It has the ability to test users' permissions in a faster, reliable and more efficient way -a very useful asset for validating policies without disrupting actual AWS resources.

### This project took me...

This project took me about 4 hours

---

## Tags

Tags are like labels I can attach to my AWS resources for proper organization...they are useful filters when searching for something.

The tag I’ve used on my EC2 instances is called "Env". The value I’ve assigned for my instances are "production" and "development" to label the instances used in production vs development environments.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

IAM Policies are rules for who can do what with my AWS resources. It's all about giving permissions to IAM users, groups, or roles, saying what they can or can't do on certain resources, and when those rules kick in.

### The policy I set up

For this project, I’ve set up a policy using JSON.

I’ve created a policy that allows some actions (like starting, stopping, and describing EC2 instances) for instances tagged with "Env = development" while denying the ability to create or delete tags for all instances.

### When creating a JSON policy, you have to define its Effect, Action and Resource.

The Effect, Action, and Resource attributes of a JSON policy means:

Effect -‍ This can have two values - either Allow or Deny, to indicate whether the policy allows or denies a certain action. Deny has priority. Looking at the first statement, "Effect": "Allow" means this statement is to allow for an action.

‍Action -‍ A list of the actions that the policy allows or denies. In this case, "Action": "ec2:*" means all actions that you could possibly take on EC2 instances are allowed. 

‍Resource - ‍Which resources does this policy apply to? Specifying "*" means all resources within the defined scope.

---

## My JSON Policy

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_1c864649)

---

## Account Alias

An account alias is a friendly name for my AWS account that I can use instead of my account ID (which is usually a bunch of digits) to sign in to the AWS Management Console.

Creating an account alias took me 15 seconds... Now, my new AWS console sign-in URL is https://nextwork-alias-emmanuelgarrison.signin.aws.amazon.com/console

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### Users

IAM users are used to represent individuals (like developers or administrators or interns) that need to access AWS console resources...through authentication and is assigned specific permissions to perform actions. 

### User Groups

IAM user groups are a collection/folder of IAM users.

I attached the policy I created to this user group, which means it allows me to manage permissions for all the users in my group at the same time rather than individual users.

---

## Logging in as an IAM User

The first way is through sending an email. The second is by downloading the .csv file and passing it to the intern.

Once I logged in as my IAM user, I noticed some of my dashboard panels are showing Access denied...This was because those AWS resources have been denied by the IAM policy in the JSON file I initially configured for the development environment.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_6f2ab446)

---

## Testing IAM Policies

I tested my JSON IAM policy by attempting to STOP both the development and production instances from running, using the newly created IAM user/intern (credentials).

### Stopping the production instance

When I tried to stop the production instance I received an ERROR: Failed to stop the instance i-0ae1ecffc366d5026. You are not authorized to perform this operation.

This was because of the Condition Block of the IAM‍ policy is in action. The condition is that only resources tagged with "Env - development" are allowed...all others are denied/not authorized. 

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_0e7a9d6a)

---

## Testing IAM Policies

### Stopping the development instance

Next, when I tried to stop the development instance, it stopped successfully. This was because, this action is allowed in the IAM JSON policy attached to the user.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_1811801c)

---

## The IAM Policy Simulator

The IAM Policy Simulator is a special IAM tool to test users' permissions in a faster, more efficient way. It's useful for validating policies without disrupting actual AWS resources.

### How I used the simulator

I set up a simulation for Deleting Tags ans Stopping All Instances... The results were both Denied, even though the user/intern is allowed to stop the EC2 Development instance...I had to adjust the Stop Instance simulator to specify for only Development and run the Simulator test again and this time, it was validated successfully.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-iam_069d8a621)

---

---
