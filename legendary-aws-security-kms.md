<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Encrypt Data with AWS KMS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-kms)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-kms_w0x1y2z3)

---

## Introducing Today's Project!

In this project, I will demonstrate how to 
-Create encryption keys with AWS KMS (Key Management Service).
-Encrypt a DynamoDB database with a KMS key.
-Add and retrieve data from my database to test my encryption.
-Observe how AWS stops unauthorized access to my data.
-Give a user encryption access.

The goal is to protect a database using encryption with AWS KMS (Key Management Service)

### Tools and concepts

Services I used include AWS KMS (Key Management Service), Key Policies, DynamoDB database and IAM. Key concepts I learnt include creating encryption keys in KMS, creating DynamoDB table, adding items to Dynamao DB table and MAC (Message Authentication Code).


### Project reflection

This project took me approximately 3 hours...The most challenging part was understanding the differences between a digital signature, a MAC (Message Authentication Code) and a hash...another challenging part was understanding the difference between encryption and other ways to control access such as ACLs, security groups or bucket policies....It was most rewarding to confirm that a KMS key can be accessible to many users, BUT they may not have the permissions to decrypt the data it protects....only those with the right permissions can use it to do specific actions like encryption or decryption.

I did this project to be equipped with hands-on practical and most in-demand skills in the market. Also to understand the concepts and build complex solutions/applications that integrate more tools such as AWS KMS (Key Management Service), DynamoDB database, IAM,  Kubernetes, Terraform, S3, AWS, Jenkins, Ansible, EKS, eksctl, ECR, kubectl, Docker, Github, nano, EC2, IAM, Dockerfile, Linux, JSON, Bash and multi-cloud architectures to solve real world problems that companies face.

---

## Encryption and KMS

Encryption is the process that uses algorithms to convert data into a secure format called ciphertext. Only authorised users can decrypt and restore the data to its original, readable state. Otherwise, it looks like a scrambled piece of text like bihtueg34509ua. 

Companies and developers do this to encrypts customers' data.

Encryption keys are keys that tells the algorithm exactly how it would transform plain text into the jumbled up format called cipher text.

AWS KMS is a secure vault for your encryption keys. You use KMS to create, manage, and use encryption keys that protect the data in your AWS resources...

Key management systems KMS are important because it helps you manage all your encryption keys, like what it encrypts or who has access, all in one safe  place. KMS also gives you logs on every time a key was used. This helps companies/developers meet compliance requirements for data security!

Encryption keys are broadly categorized as symmetric and asymmetric... I set up a symmetric key because Symmetric encryption uses a single encryption key to both lock (encrypt) and unlock (decrypt) my data. They are generally faster and more efficient for encrypting large amounts of data, which is why I'm using one for my DynamoDB table.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-kms_a2b3c4d5)

---

## Encrypting Data

My encryption key will safeguard data in DynamoDB, which is one of AWS's database services. DynamoDB stands out as a fast and flexible way to store your data, which makes it a great choice for applications that need quick access to large volumes of data e.g. games.

The different encryption options in DynamoDB include:
-Owned by Amazon DynamoDB
-AWS managed key: AWS Key Management Service (KMS) 
-Stored in your account, and owned and managed by you: aka a customer managed key (CMK)

Their differences are based on where the key is stored, it's usage and who has control. I selected -Stored in your account, and owned and managed by you: aka a customer managed key (CMK).

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-kms_q8r9s0t1)

---

## Data Visibility

Rather than controlling who has access to the key, KMS manages user permissions by controlling who have the permissions to decrypt the data it protects.

Despite encrypting my DynamoDB table, I could still see the table's items because I am an authorized user who have permissions to use the encryption key in KMS. DynamoDB uses transparent data encryption, which means data is secure at rest, yet still accessible to authorized users that have the right permissions.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-kms_c0d1e2f3)

---

## Denying Access

I configured a new IAM user as the test user to test the KMS key's effect. The permission policies I granted this user are AmazonDynamoDBFullAccess but not KMS key permissions.

After accessing the DynamoDB table as the test user, I encountered "Access denied -You don't have permission to kms:Decrypt"...because new IAM user you're logged in as (nextwork-kms-user) does not have the permission to decrypt the data...This confirmed that a KMS key can be accessible to many users, but only those with the right permissions can use it to do specific actions like encryption or decryption.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-kms_w0x1y2z3)

---

## EXTRA: Granting Access

To let my test user use the encryption key, I added the test user to the Customer-managed key...My key's policy was automatically updated to allow the test user to allowed to perform the following actions:
-Encrypt: You can encrypt data using the key.
-Decrypt: You can decrypt data that was encrypted with the key.
-ReEncrypt*: You can transfer data from one key to another.
-GenerateDataKey*: You can create new "mini-keys" (data keys) for locking smaller chunks of data.
-DescribeKey: You can get details about the key, like its name or usage policies.

Using the test user, I retried to access the DynamoDB table...I observed that test user can access the database's items....which confirmed that the test user has access to decrypt data that was encrypted with the key.

Encryption secures data instead of controlling access to a resource (e.g. a DynamoDB table) or an entire service (e.g. DynamoDB).... I could combine encryption with Other access controls, like security groups or bucket policies to secure the data within the resource. Even if someone bypasses the access controls (like hacking into a server or intercepting data), they can’t understand the data unless they have the decryption key.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-security-kms_feffb2fb8)

---

---
