---
title: How to self-host GitLab and GitLab runner using Docker container
description: The article is a detailed guide on how to self-host GitLab. It covers its wide range of DevOps features, the installation requirements, and the advantages of managing your code on your own servers.
date: 2024-07-29 12:00:00 -400
categories: [Development]
tags: [Linux, Gitlab, Git, Version Control]     # TAG names should always be lowercase
image:
    path: /assets/content/gitlab-runner/gitlab-runners.jpg
---

# Part 1: GitLab-CE Deployment
## What is GitLab? 

GitLab is a source control management tool, similar to GitHub, but with the additional advantage of being self-hosted. This capability allows you to manage and maintain your code on your own servers. One might question the necessity of using GitLab when GitHub already exists for repository storage. The rationale is found in GitLab's extensive features, which can be utilized within your local environment. It is particularly beneficial for individuals learning DevOps, as it offers comprehensive tools for building and deploying applications in containers.


## What are the Requirements?

![gitllab diagram](/assets/content/gitlab-runner/gitlab-diagram.png)

Now that we know what GitLab is, let's go over the requirements before installing this amazing application. The requirements are pretty straightforward. You'll need the following with adequate resources:

- Two Linux environments (these can be virtual machines, VPS, or bare metal servers)
- Docker Engine and Docker Compose
- SSH access to those machines

## Let's prepare the machines

I could walk you through the entire process of setting up a Linux server and installing Docker on it. But instead, I thought it would be more helpful to share a bash script that can speed things up by automatically installing the necessary packages and Docker for you. If you still prefer to follow the step-by-step guide, you can check out my previous blog post by clicking the link.

- [Docker Installation Guide](https://utbazafar.com/posts/docker-portainer/)
- [Proxmox Onboarding Guide](https://utbazafar.com/posts/proxmox-onboarding/)


### Initial Linux Packages and Docker Installation Bash Script

I have a GitHub repository with some bash scripts that can help speed up the installation of initial packages and Docker on a Linux server. Before running these scripts, please review their contents to ensure the package utilities match your Linux distribution. My primary choice is Ubuntu Server, so I've included "sudo" in the scripts.

#### Repository Links
- [Linux Package Script](https://github.com/Uzafar90/useful-bash-scripts/blob/main/initial_linux_setup.sh)
- [Docker Installation Script](https://github.com/Uzafar90/useful-bash-scripts/blob/main/docker_installation.sh)

### Make the shell executable to run 

After cloning the repository for both scripts, take a moment to review the content. Don't forget to change the file permissions to make them executable. You can refer to my codebase below for an example.

```bash

# Change the read/write permission on files
chmod +x [--bash file name--].sh

# Run the follow file afterward
./[--bash file name--].sh

```

## Let's install GitLab in Docker 

Now that you've run the two bash scripts mentioned above, your machine is all set with the necessary Linux packages and Docker environment. Next, we'll create a Docker Compose file and write the script to install GitLab.

### Step 1 - Create Folder and File 

First, let's create a folder where we'll store our Docker Compose file and all the necessary data.

```bash
#create a folder
mkdir gitlab_server

# access that folder
cd gitlab_server

#create a file 
touch docker-compose.yaml

```

### Step 2 - Docker Compose Script

After you've created a folder and a docker-compose file, the next step is to open the docker-compose file in edit mode so you can add your script.

```bash

#open the file in the text editor
sudo nano docker-compose.yaml

```

```yaml

---
services:
  gitlab:
    image: 'gitlab/gitlab-ce:latest'
    container_name: gitlab
    restart: always
    hostname: [Server IP]
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://[Server IP or Domain]'
        gitlab_rails['gitlab_shell_ssh_port'] = 2222
        puma['worker_processes'] = 0 
        sidekiq['max_concurrency'] = 5
        prometheus_monitoring['enable'] = false
    ports:
      - '80:80'
      - '443:443'
      - '2222:22'
    volumes:
      - '/[absolute path]:/etc/gitlab'
      - '/[absolute path]:/var/log/gitlab'
      - '/[absolute path]:/var/opt/gitlab'
    shm_size: 256m

```

Finally, deploy the container using Docker Compose command.

```bash 

sudo docker-compose up -d

```


## Access GitLab Web UI

Now that your Docker container is up and running, it's time to access the GitLab web UI. But first, we need to retrieve the temporary password to log in to GitLab.


```text

gitlab_server -> config -> gitlab_temporary_password

cat gitlab_temporary_password
[temporary password]

```

After copying the temporary password, open your web browser and go to the GitLab instance using the URL specified in your Docker Compose file under 'external-url' (For example, 'http://ServerIPorDomain').

![gitllab ui](/assets/content/gitlab-runner/gitlab-ce.png)

# Part 2: GitLab-Runner Deployment
## What is GitLab Runner?

Now that we understand what GitLab is and have gone through the process of installing it using Docker, it's crucial to understand the importance of GitLab Runner. GitLab Runner plays an important role in executing CI/CD pipelines for each project. It manages the dependencies required for these pipelines and can be easily installed using Docker Compose. 

## Let's install GitLab Runner in Docker 

Although you can install the gitlab runner on the same virtual machine running docker by creating a separate docker compose file. However, I like to keep them separate because GitLab instance itself is a very resource heavy so I find it helpful to run my runners on the separate machine.  

### Docker compose for GitLab Runner

Just like in Part 1, start by creating a separate folder and setting up your Docker Compose file. Once that's done, you can add the GitLab Runner script to the Docker Compose file.


```yaml

---
services:
  gitlab-runner:
    image: gitlab/gitlab-runner:latest
    container_name: gitlab-runner
    restart: always
    volumes:
     - '/[absolute path]:/etc/gitlab-runner'
     - '/[absolute path]:/var/run/docker.sock'

```

Finally, deploy the container using Docker Compose command.

```bash 

sudo docker-compose up -d

```

# Part 3 - Connect GitLab-CE with GitLab Runner
## Let's connect GitLab Runner to your GitLab CI/CD Pipeline. 

Now that we have our GitLab instance up and running in Docker, let's explore how to connect a GitLab Runner to our project's CI/CD pipeline. ***While I won't go through the entire process in detail—there are plenty of YouTube videos that can guide you.*** I will give you an overview to help you get started. 

### Create a project in GitLab to access GitLab Runner. 

First, you'll need to create a project in GitLab. Once that's done, head over to the repository settings where you'll find an option to access the runner. I've included a screenshots to help you locate this setting. Next, create a "New Project Runner" and follow the prompts in the next window to generate the runner keys.

![gitllab runner](/assets/content/gitlab-runner/gitlab-runner.png)

Ensure you create tags to identify your registered job. The configuration part is optional, so tailor it to fit your project's needs.

![gitllab create runner](/assets/content/gitlab-runner/create-runner.png)

The steps shown in this screenshot are detailed in the next section.

![gitllab runner info](/assets/content/gitlab-runner/runner-info.png)

### Let's head over to the GitLab Runner CLI

Now that we've generated the runner keys, the next step is to register them with our GitLab runner inside the container. To do this, simply enter these following commands step-by-step in the CLI to activate it.

#### Step 1 - Download and Install the dependencies to activate the Runner.

```bash

# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner 

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start

```

![gitllab runner question1](/assets/content/gitlab-runner/runner-question1.png)


#### Step 2 - Register the generated token

```bash

sudo gitlab-runner register  --url [http://[](http://%5B) [IP](http://%5Bip) or Domain ]  --token glrt-[Token ID]

```

Next, you'll be guided through a few steps to register our CI/CD Pipeline. Here's what you'll need to provide:

- The GitLab instance URL
- The registration token
- A description for the runner
- Tags for the runner
- An optional maintenance note for the runner
- The default Docker image


![gitllab runner question2](/assets/content/gitlab-runner/runner-question2.png)


#### Step 3 - Start and Enable the Runner

When you run this command, the GitLab Runner will start and begin polling the GitLab instance for jobs to execute. 

```bash

sudo gitlab-runner run

```

![gitllab runner question3](/assets/content/gitlab-runner/runner-question3.png)

### Let's head back to GitLab

After registering the GitLab Runner with your GitLab CI/CD Pipeline, go back to your GitLab instance and refresh the runners page. You should see the new runner listed under "Assigned project runners."

![gitllab runner registered](/assets/content/gitlab-runner/gitlab-ce-runner.png)


## What's Next? 

Once you've registered the runner, the next thing to do is create a ***.gitlab-ci.yml*** file in your project. This file will allow you to run jobs every time you commit your code. There are many tutorials available online to help you set up a pipeline that fits your project's needs. However, I wanted to share some screenshots of sample job with you to give you an idea of what your job configuration might look like.

```yaml

# DUMMY 
stages:
  - build
  - test
  - deploy

# Build stage
build_job:
  stage: build
  script:
    - echo "Compiling the code..."
    - echo "Code compiled successfully."

# Test stage
test_job:
  stage: test
  script:
    - echo "Running tests..."
    - echo "All tests passed."

# Deploy stage
deploy_job:
  stage: deploy
  script:
    - echo "Deploying application..."
    - echo "Application deployed successfully."


```
The simplest way to add a GitLab Runner job to your project is by using the integrated GitLab Web IDE available within your project.

![gitllab webview](/assets/content/gitlab-runner/gitlab-webeditor.png)

Next, you'll need to create a ***.gitlab-ci.yml*** file and outline your job within it.

![gitllab editor](/assets/content/gitlab-runner/gitlab-editor.png)

After you commit your code, you can view a visual representation of your job deployment.

![gitllab deployment](/assets/content/gitlab-runner/gitlab-deployment.png)


## Couple of things to keep in Mind

- Running a GitLab instance can be quite resource-intensive, so you'll need sufficient RAM and CPU power to ensure it runs smoothly.
- You'll need to register a GitLab Runner for each project individually, as you can't use the same runner across multiple projects, at least not to my knowledge.
- This guide should help you get GitLab up and running, but you'll need to do some additional research to optimize it and tailor it to your specific needs.

## Final Thought 

Sure, you don't necessarily have to go through the entire process of installing GitLab and GitLab Runner, especially when you have free and easy-to-use platforms like GitHub available. However, hosting your own version control system along with a CI/CD pipeline can be a lot of fun. If you're on a journey to learn more about technology, experimenting with different tools is crucial. Personally, I enjoy hosting services on my own server and experimenting with them to gain knowledge and skills. This hands-on experience can be incredibly beneficial for your DevOps career, as it allows you to build, test, deploy, and host your own code in a local environment.
