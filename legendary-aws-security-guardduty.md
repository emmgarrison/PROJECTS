<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Threat Detection with GuardDuty

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-guardduty)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_sm42x3y4)

---

## Introducing Today's Project!

### Tools and concepts

The services I used were AWS GuardDuty, EC2, IAM, CloudShell, OWASP Juice Shop, Javascript. Key concepts I learnt include -Deploying a vulnerable web application -Using hacking techniques to steal credential info. -Exfiltration of sensitive data using stolen credentials. -Detecting and analyzing attacks using GuardDuty. -Implementing malware protection to safeguard data.

### Project reflection

This project took me approximately 5 hours...The most challenging part was understanding the Java code used as the hacking techniques to steal credential information and exfiltration of sensitive data using stolen credentials. ...It was most rewarding to see that GuardDuty does not only provide notification and alerts 
 in detecting and analyzing attacks BUT also implements malware protection to safeguard data.

I did this project to be equipped with hands-on practical and most in-demand skills in the market. Also to understand the concepts and build complex solutions/applications that integrate more tools such as AWS GuardDuty, CloudShell, AWS KMS (Key Management Service), DynamoDB database, IAM,  Kubernetes, Terraform, S3, AWS, Jenkins, Ansible, EKS, eksctl, ECR, kubectl, Docker, Github, nano, EC2, IAM, Dockerfile, Linux, JSON, Bash and multi-cloud architectures to solve real world problems that companies face.

---

## Project Setup

To set up for this project, I deployed a CloudFormation template that launches 27 resources that can be grouped into three main categories....The three main components are:
🌐 Web app infrastructure
 🪣 S3 Bucket for storage
🛡️ GuardDuty for security monitoring



The web app deployed is called OWASP Juice Shop...To practice my GuardDuty skills, I will hack it straight away, so that I can spend more time in GuardDuty and analyze whether it's picked up on my attacks.

GuardDuty is a threat detection service, which means it helps you find potential security risks or attacks in your apps and AWS environment. It uses machine learning to look for unusual activity in your AWS account, like your network traffic and CloudTrail activity logs. If it finds something suspicious, it will alert you so you can investigate.

In this project, it is automatically enabled for monitoring and threat detection, to see whether it's picked up on any incoming threats to my web app.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

The first attack I performed on the web app is SQL injection, which means a web vulnerability that happens when an attacker can insert malicious SQL code into an application's database queries. SQL injection is a security risk because it has the ability to bypass authentication, steal data, or even change the database structure.

My SQL injection attack involved inserting ' or 1=1;-- into the email field. This means it makes the query always evaluate to true, which lets me avoid the authentication check.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Next, I used command injection, which is a security vulnerability where the web server mistakenly executes my commands when it should merely be storing it....The Juice Shop web app is vulnerable to this because it doesn't clean up or check the data it receives before processing it.

Ideally, the app should have the ability to strip out or block any harmful scripts or commands, so only harmless, intended data is processed....process called sanitization. In this case, my web app did not sanitize. That's why I (as the hacker) can run that malicious code!

To run command injection, I inserted a JavaScript command into the Username field. The script will give the web app server (i.e. the EC2 instance running your web app's code) a series of instructions.

All together, these instructions fetch highly sensitive information (IAM credentials to my AWS environment) and stores them in a place where anyone on the internet can access it!

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

To verify the attack's success, I navigated to a public stolen credentials URL: https://d38otv7ar0ag3g.cloudfront.net/assets/public/credentials.json.....The credentials page showed me information about my AWS access credentials:

-The AccessKeyId and SecretAccessKey are my main keys for accessing AWS services.
-The Token adds extra security to my session and is used with my access keys.
-The Expiration tells you when my credentials will stop working.
-The Code and Type show the status and type of my credentials.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

The attack continues in CloudShell, because I want to simulate as the hacker and 'steal data' while logged into my own AWS account.

When I use CloudShell, it doesn't just run commands under my main AWS account. Instead, AWS assigns the session a temporary AWS account with a different account ID than my main one. AWS does this to better monitor and log each command I run via CloudShell, linking them back to the specific session and user.

In CloudShell, I used wget to download files from the internet. In this case, we're using it to download the credentials.json file from our web app and into the CloudShell environment. This means there's now a local file in CloudShell that contains the stolen credentaisl!

Next, I ran a command using cat and jq to to display what's inside a file, while jq is used to process JSON data.

In this case, we're using both commands to tell CloudShell to display what's inside credentials.json, but use jq to process the JSON data inside to make it easier to read.

I then set up a profile, called stolen using the stolen downloaded credentials I've extracted from my attack on the web application to authenticate and access AWS services. I had to create a new profile because by default, AWS CloudShell uses a profile that takes all the permissions granted to the user currently logged into in the Management Console.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, GuardDuty reported a finding within 5 minutes. Findings are alerts/notifications that something suspicious has happened in your AWS environment. The finding tells you what happened, who did it, and how they did it. This helps you understand the attack and fix the problem.

GuardDuty's finding was called UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS... which means someone tried to do something they shouldn’t have the rights to within my AWS environment.

IAMUser/InstanceCredentialExfiltration is the unauthorized activity that GuardDuty detected. Credentials assigned to an EC2 instance (i.e. web server of your web app) were used in a suspicious way, so it's likely that they were stolen. This data breach is called exfiltration. InsideAWS tells us that the unauthorized access happened within AWS, but by a different AWS account to mine........

Anomaly detection was used because an algorithm that always monitor and analyze my account's activity ...and thus picked up on an unsual use of my EC2 instance's credentials. GuardDuty would compare this activity against the instance's typical use of its AWS account credentials, and decide that copying an S3 bucket object is out of the ordinary.

GuardDuty's detailed finding reported that:
1. Resource affected - which tells me the attack used an IAM role to access my juice shop's S3 bucket name and EC2 instance affected.
2. Action - which tells me that an object (i.e. the important information text file) was retrieved...suing an API GetObject.
3. Location and IP address of the attacker - which would be a location in my AWS Region at the time of using CloudShell as the attacker.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

For my project extension, I enabled GuardDuty's Malware Protection for S3...Malware is a type of software designed to harm computers. It can damage computers, steal data, or disrupt operations.

To test Malware Protection, I uploaded a Malware file into my S3 bucket....The uploaded file won't actually cause damage because it's an EICAR test file -a harmless file that is programmed to make antivirus software think it's malware. It's like a fake virus that helps test if your antivirus software is working properly without actually using a real virus. In my case, I'm using the test file to verify whether GuardDuty can detect malware.

Once I uploaded the file, GuardDuty instantly triggered a Finding or an alert....A malware scan on your S3 object EICAR-test-file.txt has detected a security risk EICAR-Test-File (not a virus)....This verified that Malware Protection for the S3 bucket is working as it should.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-guardduty_sm42x3y4)

---
