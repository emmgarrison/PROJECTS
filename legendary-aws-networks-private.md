<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Creating a Private Subnet

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-private)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

## Creating a Private Subnet

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-private_afe1fdbd)

---

## Introducing Today's Project!

### What is Amazon VPC?

VPC is a virtual private cloud or subdivision of a larger cloud network with specified resources. Otherwise resources will be scattered everywhere.

### How I used Amazon VPC in this project

I designed and configured  a virtual private subnet and a private network ACL within a VPC.

### One thing I didn't expect in this project was...

The key takeaways for me was that, the private subnet cannot have the same IP block as the public subnet. It is also detached from the internet gateway.

### This project took me...

1 hour

---

## Private vs Public Subnets

The difference between public and private subnets is that private subnets are not connected to the internet.

Having private subnets are useful because it can be used to host sensitive or private assets such as customers  database and users login details.

My private and public subnets cannot have the same CIDR IP block addresses.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-private_afe1fdbd)

---

## A dedicated route table

By default, my private subnet is associated with NextWork Private Route Table.

I had to set up a new route table because the other route table is connected to the internet via gateway.

My private subnet's dedicated route table only has one inbound and one outbound rule that allows traffic within the 10.0.0.0/16 CIDR block to flow within the network.
There is no route with an internet gateway as the target! This means there is no route for traffic to leave your VPC.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-private_b4b904b5)

---

## A new network ACL

By default, my private subnet is associated with the default network ACL is associated, since I haven't set up an explicit association between your private subnet and another network ACL.

A VPC's default network ACL allows all traffic, which exposes your private subnet to unrestricted access from the internet or other untrusted networks.

I set up a dedicated network ACL for my private subnet because I want to restrict traffic and protect my private subnet!

My new network ACL has two simple rules: to deny all inbound and outbound IPs

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-networks-private_1ed2cb07)

---

---
