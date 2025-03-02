<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Virtual Private Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-vpc)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Build a Virtual Private Cloud (VPC)

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-vpc_2facf927)

---

## Introducing Today's Project!

### What is Amazon VPC?

VPCs are virtual private cloud or subsections within a larger cloud network infrastructure. It is useful because it provides a curated section or group of networks or resources in a controlled environment.

Without it, resources will be scattered everywhere.

### How I used Amazon VPC in this project

Today I used VPC to create a public VPC that can host EC2 instances and web applications accessed from the internet.

### One thing I didn't expect in this project was...

I wasn't expecting that enabling a feature could automatically-assigned the public IPv4 address

### This project took me...

1 hour

---

## Virtual Private Clouds (VPCs)

VPCs are virtual private cloud networks within a broader or larger cloud network.

There was already a default VPC in my account ever since my AWS account was created. This is done so you could launch resources (e.g. EC2 instances) and connect services together from Day 1 of using AWS. If it didn't exist, you would've had to learn how to create a VPC before you can use some of the services that need VPCs to function.

To set up my VPC, I had to define an IPv4 CIDR block, which is 10.0.0.0/16 a private IP subnet

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-vpc_2facf927)

---

## Subnets

Subnets are If your VPC is a city, subnets are like different neighborhoods inside your city. You use subnets to group resources with similar access rules and restrictions.

A VPC can have as many public and private subnets as you need, but subnets in the same VPC cannot have overlapping IP address CIDR blocks! This means each subnet must have a unique range of IP addresses.

There are already subnets existing in my account, one for every AWS Availability Zone AZ

Once I created my subnet, I enabled auto-assign public IPv4 address. This setting makes sure any EC2 instance launched in that subnet will instantly get a public IP address so you won't have to create one manually... so that you can use it to connect to the internet.

The difference between public and private subnets are public subnets are connected to the internet...Resources inside a public subnet can communicate with external networks.

For a subnet to be considered public, it has to be connected to an internet gateway to call it public

A private subnet does not have direct internet access 🔐 You'd use it for internal resources that don’t need to be publicly accessible.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-vpc_157c4219)

---

## Internet gateways

Internet gateways are bridges that connects your VPC to the outside world (the internet), so your resources can communicate beyond your private space.

Attaching an internet gateway to a VPC means resources in your VPC can now access the internet. The EC2 instances with public IP addresses also become accessible to users, so your applications hosted on those servers become public too. 

If I missed this step, then resources inside my VPC subnet cannot be accessed on the internet.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-vpc_4ae90410)

---

## Using the AWS CLI

VPC resources could also be created with CloudShell, which is a shell in my AWS Console, which means it's a space for you to run code. The awesome thing about AWS CloudShell is that it already has AWS CLI pre-installed.

💡 What is CLI?
AWS CLI (Command Line Interface) is a software that lets you create, delete and update AWS resources with commands instead of clicking through your console.

To set up a VPC or a subnet, you can use the command....
aws ec2 create-subnet --vpc-id vpc-0af8af2b69af90486 --cidr-block 10.0.0.0/25

Make sure to avoid errors by including the CIDR address block for the subnet.
-Remember that a subnet is a subsection of your VPC, so its CIDR block has to be completely inside the VPC's range too:

--Is the number after the slash larger than the VPC's CIDR block? Make sure you're using a range from /25 to /32, otherwise your subnet is bigger than the VPC (not possible)!

--Is the range overlapping with the VPC's CIDR block? Since your VPC's range starts at 10.0.0.0 and ends at 10.0.0.255, you'll need to make sure your subnet's CIDR starts with 10.0.0 too

Compared to using the AWS Console, an advantage of using commands is the speed and versatility. Overall, I preferred the CLI

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-vpc_9b2465411)

---

---
