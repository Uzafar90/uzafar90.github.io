---
title: Docker and Portainer on Ubuntu Server
description: The installation process of the Docker and Portainer.
date: 2024-07-02 12:00:00 -400
categories: [Containerization]
tags: [docker, container, ubuntu]     # TAG names should always be lowercase
image:
    path: /assets/content/docker/docker-portainer.jpg
---

Have you ever considered that using a Virtual Machine for every small project can be resource heavy? Whether you want to host a web server or a DNS server, which can be a small instance that doesn't need a full-fledged virtual machine, that's where Docker comes into play. Docker allows you to install small applications in isolated containers, making it an amazing way to test or experiment with different versions of an application. We’ll dive into more details about what Docker is and how you can leverage it in your homelab with the support of Portainer.

## What is docker? 

Docker is an open-source platform that simplifies setting up, running, and managing applications in lightweight, portable containers. These containers are self-contained environments that bundle everything an application needs, such as code, runtime, libraries, and dependencies, ensuring it runs consistently across various setups. Perfect for a homelab, Docker helps you efficiently manage and experiment with different applications and services without the overhead of full virtual machines.

## Ok, then what's Portainer?

Docker is primarily used and managed through the command-line interface (CLI), which requires some foundational knowledge of Linux to control and manage containers using commands. However, Portainer is a tool that installs as a container on top of the Docker Engine, providing a user-friendly graphical interface for managing Docker environments. This makes it much easier for those with limited CLI knowledge to manage containers through a visual interface. Portainer allows you to see all containers, networks, volumes, and other metrics in one place, making it very helpful for anyone hosting applications in Docker containers.

## Next, installation process

### Before proceeding

Before we proceed with the installation process, it's important to have a Linux virtual machine with a built-in terminal ready to execute our installation commands. The platform you choose for your Docker environment is entirely up to you, whether it's on-premise or cloud-based. Additionally, ensure that the VM configuration is set according to your preferences for persistent runtime.

### Install Docker Engine

The installation of Docker Engine will vary depending on your choice of Linux distribution. However, the fundamental steps are similar since the Linux kernel is the same. I recommend following the official documentation from [Docker](https://docs.docker.com/engine/install). For this guide, I will be using Ubuntu Server as my chosen distribution.

- First, we need to set up the Docker repository to ensure we have all the necessary packages to install Docker. *Please run each line separately:*

```shell

# Add Docker's official GPG key:
sudo apt-get update

sudo apt-get install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc


# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


# Run apt update to update all the packages
sudo apt-get update

```

- Now that the repository is set up and all dependencies are installed, we are ready to install the Docker packages. Run the following commands:

```shell

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```

- Finally, let's verify if Docker is running in our environment. Run the following commands:

```shell

sudo docker --version
# or
sudo docker ps

```

## Now, we're going to install Portainer

Since we have successfully installed Docker Engine and verified it, it's time to install our first container. Let's start with installing Portainer. There are two ways to deploy containers in your Docker environment: using an ad-hoc command or creating a Docker Compose file in YAML format. I'll show you both methods.

### Method 1: Ad-Hoc Command

- Create a Docker volume for Portainer data:

```shell

sudo docker volume create portainer_data

```

- Run the Portainer container:

```shell

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

```

- Verified if the container is running

```shell

root@server:~# docker ps
CONTAINER ID   IMAGE                          COMMAND                  CREATED       STATUS      PORTS                                                                                  NAMES             
de5b28eb2fa9   portainer/portainer-ce:latest  "/portainer"             2 weeks ago   Up 9 days   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp, 0.0.0.0:9443->9443/tcp, :::9443->9443/tcp   portainer

```

*This command will create a volume for Portainer’s data and start the Portainer container, exposing it on ports 8000 and 9443*

### Method 2: Docker Compose File

- Create a folder on your server named "Portainer," navigate into that folder, and then create a file named "docker-compose.yaml."

```shell

mkdir Portainer 
cd Portainer
touch docker-compose.yaml

```

- Then, open the file with your preferred text editor, such as Nano or Vim.

```shell 

sudo nano docker-compose.yaml

```

- Enter your configuration in format of yaml file. (Example Config):

```yaml

version: '3.8'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: always
    ports:
      - "8000:8000"
      - "9443:9443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

volumes:
  portainer_data:


```

- Finally, deploy the container using Docker Compose command.

```shell

sudo docker-compose up -d

```

## Access your Portainer UI 

Once the container is up and running, navigate to the Portainer UI by using your VM's IP address and the container's port number, which in this case is 9443. Open a web browser and enter this address to access the Portainer web interface.

- ipaddress:9443 or localhost:9443
- Create an account to access your Portainer portal

![Portainer UI](/assets/content/docker/portainer.png)

![Portainer Home](/assets/content/docker/port-home.png)


**Note:** *Please take your time to read the documentation on the official [Portainer](https://docs.portainer.io/) site to understand how to navigate and deploy your containers. There are many tutorials available online that provide step-by-step guidance. However, I recommend learning Docker through ad-hoc commands, as using the terminal can be faster and more enjoyable.*

## Ending Points

Docker and Portainer make a great team for managing containerized applications. Docker is the core technology that powers containerization, while Portainer adds an intuitive interface that makes managing those containers a breeze. Whether you're a developer, a system admin, or a DevOps engineer, using Docker and Portainer together can really simplify your workflow and boost your efficiency in handling containers.