---
categories:
- DevOps
- Article
date: "2022-01-30T16:20:24Z"
guid: https://mischavandenburg.com/?p=68
id: 68
tags:
  - DevOps
title: Docker LEMP Stack deployed with Ansible
url: /docker-lemp-stack-deployed-with-ansible/
---

In order to learn more about Docker and Ansible I am working on an assignment to take an existing application and to break it down into containers. However, in order to be able to understand this process properly, I first needed to understand more about Docker and containerisation.

I wrote a playbook that installs Docker and deploys a fully containerised LEMP stack on a virtual machine.

[You can have a look at the Github repo with the result here. ](https://github.com/mischavandenburg/lemp_docker_ansible)

The repo is using the “ansible-galaxy init” role structure. You will find the playbooks as follows: roles/your\_choice/tasks/main.yml

## Docker

I was very excited to learn more about Docker and containerisation. I was familiar with the concept of virtualisation, which is creating virtual versions of fully functional machines on a host operating system. But the concept of containerisation was new to me.

As I understand it, containerisation differs drastically from virtualisation because containers are able to use resources from host directly. They do not need an entire operating system to run, and therefore they are a much more lightweight.

This means that resources can be used much more efficiently which eventually can mean cost reduction in your cloud infrastructure.

Docker is a very popular platform for building and running containers. It seemed like the best option to get started with deploying my own containers.

## LEMP Stack

My colleague recommended me [this tutorial](https://tech.osteel.me/posts/docker-for-local-web-development-part-1-a-basic-lemp-stack) to become more familiar with Docker. It uses a LEMP stack as an example application. When I told friends about the fact that I was building a LEMP stack, they corrected me and said it was a LAMP stack.

The LAMP stack is a collection of software built out of these elements:

L – Linux: the operating system

A – Apace: webserver

M – MySQL: database

P – PHP: server scripting language

However, in a LEMP stack, we use NGINX as a webserver, which is pronounced “Engine X”, hence the E in LEMP stack. Therefore, LEMP is the correct way to spell it, and it is used in all the tutorials that I have been using.

I highly recommend [the tutorial](https://tech.osteel.me/posts/docker-for-local-web-development-part-1-a-basic-lemp-stack) in order to learn how to deploy your first collection of containers. Deploying one container is relatively easy with Docker, but it gets a little more complicated when deploying several containers and making them communicate with each other in order to combine them into one application. But this tutorial does a great job at showing you how it’s done and it is especially good at explaining the steps along the way.

## Docker Compose vs. Ansible

Docker Compose is a tool you can use to run multi-container applications. With the help of the tutorial, it was fairly easy to understand and hit the ground running by deploying multiple containers into one network.

Now let’s have a look at how we actually set up the containers. In the Docker Compose file, the NGINX container was defined like this:

```yaml
version: '3.8'

#Services
services:
  
  #Nginx Service
  nginx:
    image: nginx:1.19
    ports:
      - 80:80
    volumes:
      - ./src:/var/www/php
      - ./.docker/nginx/conf.d:/etc/nginx/conf.d
    depends_on:
      - php
```

It looks pretty straightforward, right? Almost like pseudocode. We tell Docker which image to pull from the Docker Hub, and we tell it to route container’s port 80 to our host’s port 80. This ensures that the web server can be accessed from the outside, provided you have opened this port in the firewall.

Next there is the volumes section: this mounts certain directories on the host into the container so it is accessible. In this case this was necessary to transfer the web server configuration and the index.php which we wanted to serve to the outside.

Having successfully deployed my LEMP using Docker Compose, the next step was to automate this process by using Ansible. Ansible is a very powerful tool which enables you to automate configuration management and application deployment by writing scripts called playbooks.

##### Why was it necessary to introduce Ansible?

By using Docker Compose, you would need to have Docker and Docker Compose installed on the virtual machine before you could start running the containers.

However, Ansible gives you the power to take a completely fresh virtual machine, configure it from scratch, and install Docker and its necessary dependencies, followed by deploying the containers.

#### Now let’s take a look at the same container defined in Ansible:

```yaml
- name: start nginx 
  docker_container:
    name: nginx
    image: nginx:1.19
    detach: yes
    ports:
      - 80:80
    networks:
      - name: network_one
    volumes:
      - /src:/var/www/php
      - /.docker/nginx/conf.d:/etc/nginx/conf.d

```

Although there are some differences, they look very similar. Converting my Docker Compose file to an Ansible playbook was quite a natural and easy experience. It also helps that both are written in YAML and therefore use the same indentation conventions.

A few differences we can observe:

In the Ansible playbook we invoke the docker\_container module, whereas they are defined as services in the Docker Compose file. Another difference is that we need to set up the network ourselves. In the Docker Compose file, we just specified the containers and Docker Compose created a network automatically and made sure that all containers were connected to it.

However, it isn’t very complicated in Ansible either:

```yaml
- name: setup network
  docker_network:
    name: network_one
```

We simply call the docker\_network module and tell it to make a network called network\_one. All we need to do then is make sure to set the networks: parameter to network\_one in the docker\_container module as we saw above.

The last point to note is the detach parameter. This means that the container will keep running in the background after it is started.

### Result

After some debugging here and there and making sure all of the elements were in place, eventually we get the satisfying message that everything went according to plan:

![Successful play from Ansible](/success.png)

The result is a webpage being served on the server ip:

![The final web page served by Nginx ](/webpage.png)

I know, it is not the prettiest or most intricate design. But remember that I am working towards becoming a DevOps Engineer, not a Front End Developer 😉

We can also enter the phpMyAdmin dashboard by adding port 8080 to our ip in the browser:

![The PHP page.](/php.png)

## Conclusion

The assignment of deploying a LEMP stack in separate containers has been very useful and I learned a lot from the process. There were a few more modules that needed to be configured in Ansible as opposed to the Docker Compose method, but the tradeoff is that Ansible is much more powerful and enables you to configure the server from scratch. You can have a look at the code in the GitHub repo to see all of the changes I needed to do.

The only part that I needed to do by hand is to create the VM in the Microsoft Azure portal, open the ports and configure the SSH keys. The next step in my learning process will be to learn how I can automate this step as well. This means that I will need to learn Terraform.

By using Terraform I will be truly deploying this stack as Infrastructure as Code, but doing all of these steps with Ansible has given me a much better understanding of Infrastructure as Code already.
