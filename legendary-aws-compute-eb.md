<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy an App with Docker

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eb)

**Author:** Emmanuel Garrison  
**Email:** emmgarrison@gmail.com

---

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eb_c4df13c84)

---

## Introducing Today's Project!

### What is Docker?

Docker is a platform that helps you create, manage, and deploy containers efficiently. You can think of Docker as a tool that helps you create the cargo container and load them onto one ship.
Each container/cargo has its own unique app/goods inside, but they can all run on the same computer/same ship. This is possible because containers packags up everything an app needs to run, so it works consistently no matter where it is.

In this project I used Docker to build a custom images/container templates based on Nginx sample, developed my own custom container, tested it locally and then I leveraged Elastic Beanstalk to deploy my web app to the internet for public access. 

### One thing I didn't expect...

One thing I didn't expect in this project was Docker Hub - a container registry, which means it's an online library where people can share and find Pre-Built Container Images. Therefore I leveraged it to create a new container from an existing container image! That simplified the process of building, testing and deploying my application. 

### This project took me...

This project took me 5 hours to complete.

---

## Understanding Containers and Docker

### Containers

Containers are central repository platforms that contain packages for an application and everything it needs to run (i.e. dependencies) in one file from multiple developers. 

They are useful because they solve a common problem called the "it works on my machine" problem. Have you ever seen someone run code perfectly on their computer, but somehow get errors when you run the exact same thing on your own computer?..

For example, a software development team is developing a complex web app, they might use containers to make sure eveyone in the team is working in exactly the same environment, even though they are on different machines.

A container image is a blueprint or template for containers. It gives Docker instructions on what to include in a container, such as application code, libraries, dependencies, and necessary files.

You can make multiple, identical containers from the same image, and all of those containers will behave the same way regardless of where it's deployed (as long as there is a container platform e.g. Docker to run the container).

### Docker

Docker is a container management tool that helps you create, manage, and deploy containers efficiently. You can think of Docker as a tool that helps you create the cargo container and load them onto one ship.
Each container/cargo has its own unique app/goods inside, but they can all run on the same computer/same ship. This is possible because containers packags up everything an app needs to run, so it works consistently no matter where it is.

Docker Desktop is a GUI tool or an app that makes using Docker more interactive and efficient.

The Docker daemon is the engine that actually does the heavy lifting of building, running, and distributing your containers...when you run Docker commands you type into the terminal, or clicks you make through the Docker Desktop e.g. create a container.

---

## Running an Nginx Image

Nginx (pronounced as "engine-x") is a web server, which means it's a program that serves web pages to people on the internet. Engineers use Nginx because it can handle lots of web traffic smoothly and efficiently. 

Sometimes, you might hear Nginx referred to as a 'proxy server', which means it can also be used to forward requests from the internet to other servers, helping balance the load or handle more users.

The command I ran to start a new container was: 
docker run -d -p 80:80 nginx


![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eb_6245f5bb10)

---

## Creating a Custom Image

The Dockerfile is a document with all the instructions for building your Docker image. Docker would read a Dockerfile to understand how to set up your application's environment and which software packages it should install.

My Dockerfile tells Docker three things:
1. FROM nginx:latest, means our image starts as a copy of the latest Nginx image, but we'll make a few modifications/additions to it to customise it for what we need.
2. COPY index.html /usr/share/nginx/html/ replaces the default HTML file provided by Nginx with your own custom index.html file, so you're customizing the Nginx server to serve your own web content.
3. EXPOSE 80 means you want the container to receive web traffic through port 80, which makes it easier for users and other services to reach your web app.

The command I used to build a custom image with my Dockerfile was: docker build -t my-web-app .

-t my-web-app . names my container image my-web-app, and the '.' at the end of the command tells Docker to find the Dockerfile in the current directory.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eb_4c741d1913)

---

## Running My Custom Image

There was an error when I ran my custom image because there's already a container using port 80, so the new container I'm creating can't access it.

 I resolved this by running this command  docker ps --filter "publish=80"...to find the container. 
I then stopped the container by selecting the square stop icon or by running this command in my terminal:
docker stop <container_id>
=docker stop d23ee208b205b45b98f7579354fc71ad52dfc559c8f85c1cf8fdc3e7c36a0197

In this example, the container image is a template for creating a new container running an Nginx server that serves my custom index.html file in my custom container -It gives Docker instructions on what to include in my custom container, such as application code, libraries, dependencies, and necessary files.
...and the container is where I can start, stop, and manage my custom containers.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eb_74b5c3d619)

---

## Elastic Beanstalk

Elastic Beanstalk is a service that makes it easy to deploy cloud applications without worrying about the underlying infrastructure. You simply upload your code and Elastic Beanstalk handles everything needed to get it running, like setting up servers and managing scaling. This lets you focus on your application code without having to spend too much time on managing cloud infrastructure.

In more technical terms, Elastic Beanstalk handles things like:
-Capacity provisioning
-Load balancing
-Auto-scaling
-Application health monitoring

Deploying my custom image with Elastic Beanstalk took me about 10 minutes.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eb_26d5573b23)

---

## Deploying App Updates

To learn how to deploy app updates with Elastic Beanstalk, I updated my app by adding a new image via html. I verfied those changes by double clicking on my index.html file in the Compute folder.

My app updates didn't show up in my live environment immediately because I had to upload the updated zipped folder that contains the latest index.html file. To deploy my changes, I only had to upload the new zip file I've created.
For the Version label, I entered Version-2 and Selected Deploy.

![Image](http://learn.nextwork.org/stimulated_black_timid_rambutan/uploads/aws-compute-eb_5b7034684)

---

---
