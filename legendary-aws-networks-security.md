<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Traffic Flow and Security

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-security)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## VPC Traffic Flow and Security

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-security_92b0b0b4)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a virtual private cloud or subdivision or section of a larger cloud network with specified resources. Without it, resources will be scattered everythere.

### How I used Amazon VPC in this project

I created route tables, security groups and network ACLs for a subnet within a VPC.

### One thing I didn't expect in this project was...

The key takeaways for me was that I gained a deeper understanding of Route Tables, Security Groups and Network ACLs and their position and scalability in the cloud architecture.

The diagrams and other visuals used to illustrate the configurations, workflows and project architecture also helped me gain clarity.

### This project took me...

1 hour

---

## Route tables

Route tables are like a GPS for the resources in your subnet. Just like a GPS helps people get to their destination in a city, a route table is a table of rules, called routes, that decide where the data in your network should go.

Routes tables are needed to make a subnet public because it has a route or rules that directs internet-bound traffic to the internet gateway.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-security_0a07b191)

---

## Route destination and target

Routes are defined by their destination and target, which means 
○ Destination: The IP address range that traffic wants to reach.
○ Target: The road or path that the traffic will have to take to get to its destination.
--igw-xxxxxx: Means the traffic is routed to the internet via the Internet Gateway.
--local: Means the traffic stays within the VPC, allowing internal communication between resources.

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 10.0.0.0/16 .... and a target of local 

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-security_0a07b191)

---

## Security groups

Security groups are responsible for checking who comes in and out. They have strict rules about what kind of traffic can enter or leave the resource based on its IP address, protocols and port numbers.

### Inbound vs Outbound rules

Inbound rules are control the data that can enter the resources in your security group.

I configured an inbound rule that allows HTTP traffic from "0.0.0.0/0" (meaning any IP address) is typical and necessary for public subnets, since this setting makes sure that anyone on the internet can access your public resources.

Outbound rules control that data that your resources can send out. By default, my security group's outbound rule is not specified because AWS security groups already allow all outbound traffic.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-security_92b0b0b4)

---

## Network ACLs

Network ACLs are like traffic cops stationed at every entry and exit point of your subnet, checking each data packet against a table of ACL rules before allowing them through.

### Security groups vs. network ACLs

The difference between a security group and a network ACL is that:
○ Network ACLs are used to set broad traffic rules that apply to an entire subnet. For example, blocking incoming traffic from a particular range of IP addresses or denying all outbound traffic to certain ports.

○ Security groups allow for more granular control, managing access to individual resource. You can specify which ports and protocols are allowed for each connected resource.


---

## Default vs Custom Network ACLs

### Similar to security groups, network ACLs use inbound and outbound rules

By default, a network ACL's inbound and outbound rules will allow all inbound and outbound traffic. 

In contrast, a custom ACL’s inbound and outbound rules are automatically set to deny.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-security_4faeb056)

---

## Tracking VPC Resources

---

---
