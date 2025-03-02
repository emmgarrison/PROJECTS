<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Host a Website on Amazon S3

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-host-a-website-on-s3)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Introducing Today's Project!

### What is Amazon S3

Amazon s3 is a service to store files that are hosted for the provision of publicly accessible websites

### How I used Amazon S3 in this project

To host a website

### One thing I didn't expect in this project was...

ACLs and the misconfiguration of file locations

### This project took me...

1hr

---

## How I Set Up an S3 Bucket

Creating an S3 bucket took me 2 minutes

The Region I picked for my S3 bucket was N.Virginia US East 1...It was automatically selected. I assume because its the closest to my network IP location. 

S3 bucket names are globally unique! This means no AWS account can use that bucket name anywhere in the world

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_ba6d42ad)

---

## Upload Website Files to S3

### index.html and image assets

I uploaded two files to my S3 bucket - they were:
1. index.html
2. NextWork - Everyone should be in a job they love_files

Both files are necessary for this project as the index.html file sets up the structure of the website and the Nextwork folder contains all the images for the website

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_a265af88)

---

## Static Website Hosting on S3

Website hosting means storing HTML files (and the other files/content for your website) on a web server, so it's accessible online or publicly available on the internet.

To enable website hosting with my S3 bucket, I go to the Properties tab and enable static website hosting

ACL = Access Control List which is a set rules that specify an object such as files and the list of subjects who have access or control of it.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_c22c54c0)

---

## Bucket Endpoints

Once static website is enabled, S3 produces a bucket endpoint URL, which is just like a public URL of the s3 bucket

When I first visited the bucket endpoint URL, I saw an error message 403 Forbidden..The reason for this error was, even though the s3 bucket is publicly available the contents HTML/image files of the buckets are still private by default.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_22ce4daf)

---

## Success!

To resolve this connection error, I uploaded the subfolder inside the unzipped folder and also placed the index.html file inside that folder and made all public using ACL

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Bucket Policies

An alternative to ACLs are bucket policies, which are... The benefit of using bucket policies is more specified control of who has not only access but also ability to delete obects in a bucket... while ACLs are useful for controlling who has access to ojects

My bucket policy prevented me from deleting the index.html file in the s3 bucket.. I tested this by attempting to delete the file... and saw an error failed to delete file

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-host-a-website-on-s3_sm2sm2sm)

---

---
