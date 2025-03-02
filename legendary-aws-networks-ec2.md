<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launching VPC Resources

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-ec2)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Launching VPC Resources

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-ec2_8ee57662)

---

## Introducing Today's Project!

### What is Amazon VPC?

VPC is a virtual private cloud or an isolated network or subdivision of a larger cloud network with specified resources. 
Otherwise resources would be randomly scattered with no privacy or personal space, so everyone could see and access everyone else's resources
If we imagine your AWS Region as a country, a Virtual Private Cloud (VPC) is like your own private city inside that country.

### How I used Amazon VPC in this project

Today I configured an EC2 instance in a public subnet.
and also in a private subnet, all in the same VPC.
I also used the Amazon VPC's wizard to create VPCs at a lightning fast pace.

### One thing I didn't expect in this project was...

Key takeaways for me was the use of AWS VPC's wizard to create VPCs at a faster rate and its possible scalability across larger configurations. Also using VPC's resource map to visualize how different components like subnets and route tables are connected.

Another takeaway for me is the versatility of name tags and its potential scalability across larger and complex configurations.

### This project took me...

1.30 hrs

---

## Setting Up Direct VM Access

Directly accessing a virtual machine means logging into and managing the operating system or software of the machine as if you were using it in front of you, but over the internet.

The AWS Management Console gives you a user-friendly interface to set up and manage AWS resources (we love it), but it doesn't typically provide direct access to the operating systems of your EC2 instances. 

For operations that require direct OS-level access, like installing software, editing configuration files, or running scripts, you'd use your key pair to connect directly. This method of access is essential for deeper administrative tasks or specific kinds of troubleshooting that can't be performed through the console.

### SSH is a key method for directly accessing a VM

SSH traffic means Secure Shell, and its the protocol we use for this secure access to a remote machine. When you connect to the instance, SSH verifies you possess the correct private key corresponding to the public key on the server, ensuring only authorized users can access the instance.

SSH is also as a type of network traffic. Once SSH has established a secure connection between you and the EC2 instance, all data transmitted (including your commands and the responses from the instance) is encrypted. This encryption makes SSH an ideal method for securely exchanging confidential data e.g. login credentials!

### To enable direct access, I set up key pairs

Key pairs consist of two cryptographic keys: one private and one public. The public key is installed on the virtual machine, and the private key remains with the user. When you attempt to connect, the machine uses the public key to create an encrypted challenge that can only be decrypted with the private key. Key pairs make sure that access to your EC2 instances is secure and authenticated.

They help engineers directly access their virtual machines, like EC2 instances.

A private key's file format means, just like how documents can be saved in various file formats like PDF, DOCX, or TXT, each suited for different applications or systems, private keys also come in different file formats. Not every system or application can process all these formats, so choosing the right one is crucial.

My private key's file format was .pem format, which stands for Privacy Enhanced Mail, started off as a way to secure emails but has since become the go-to format for managing cryptographic keys because it is supported by many different types of servers e.g. EC2 instances, open SSH, etc.. 

---

## Launching a public server

I had to change my EC2 instance's networking settings by going to the Instances page.
Selecting the checkbox next to my instance, and a Details panel pops up!
And switching to the Networking tab.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-ec2_88727bef)

---

## Launching a private server

My private server instance has its own dedicated security group because only resources that are part of the NextWork Public Security Group can communicate with my instance. This restricts access to a much smaller group of trusted resources, rather than allowing potentially any IP address on the internet (0.0.0.0/0) to access your instance.

My private server's security group's source is the NextWork Public Security Group... which means only resources that are part of the NextWork Public Security Group can communicate with my instance.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-ec2_4a9e8014)

---

## Speeding up VPC creation

I used an alternative way to set up an Amazon VPC! This time, I used VPC wizard option.

A VPC resource map is a visual flow diagram pops up that displays other VPC resources.

My new VPC has a CIDR block of 10.0.0.0/16... It is possible for my new VPC to have the same IPv4 CIDR block as my existing VPC because AWS VPCs are isolated from each other by default, so there won't be any IP conflicts unless you explicitly connect them using VPC peering. Multiple VPCs can use the same IPv4 CIDR block in the same AWS region and account.  

Bottom line, it's possible for your new VPC to share the same CIDR block as an existing one, but this set up will mean your overlapping VPCs can't talk to each other directly. 

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-ec2_1cbb1b88)

---

## Speeding up VPC creation

### Tips for using the VPC resource map

When determining the number of public subnets in my VPC, I only had two options: 0 or 2... 

This was because
This is AWS' best practise advice at work! When you pick 2 Availability Zones, the wizard makes sure you have a public subnet in each one. This way, your public resources stay up even if one of the two Availaiblity Zones goes down.
This setup is called redundancy (having backups in different places) and high availability (making sure your resources are always accessible). Just one public subnet wouldn’t offer this kind of reliability, so AWS doesn't let you create just one!

The VPC wizard limits you to two public subnets to keep things straightforward. If you need more, you can always add them manually later

The set up page also offered to create NAT gateways, which are access to the internet gateway for updates and patches, while blocking inbound traffic while in instances in private subnets.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-ec2_8ee57662)

---

---
