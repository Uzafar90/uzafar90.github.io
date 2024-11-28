---
title: The Perfect Combination of Jenkins and Gitlab CE 
description: A comprehensive guide on automating virtual machine deployment in Proxmox with Terraform.
date: 2024-11-28 12:00:00 -400
categories: [Development]
tags: [gitlab, jenkins, development]     # TAG names should always be lowercase
image:
    path: /assets/content/jenkins/jenkins_gitlab.jpg
---

## What is Jenkins?

Jenkins is a popular open-source tool that automates tasks in software development, especially for continuous integration and delivery (CI/CD). It streamlines the process of building, testing, and deploying software, making it easier to integrate changes continuously. Through it's widely available plugins, Jenkins can connect with many different tools and platforms used throughout the software development process.

## What makes Jenkins a better choice?

Jenkins can be a great option for GitLab projects, especially when setting up continuous integration and continuous delivery (CI/CD) pipelines. Here are some reasons why Jenkins might be a better fit:

1. **Plugin Ecosystem**: Jenkins offers a huge selection of plugins, allowing connections to many different tools and services. This flexibility is useful for trying out various technologies in your homelab.

2. **Pipeline as Code**: Create your build and deployment processes as code in Jenkins, which is great for experimenting with complex workflows or learning how to automate tasks.

3. **Distributed Builds**: Jenkins can run tasks on multiple machines, speeding up testing and building processes. Handy for running many tests or working with large projects in your homelab.

4. **Customizability and Extensibility**: Tweak Jenkins to fit specific needs, perfect for a homelab with unique requirements or for trying out new ideas.

5. **Community and Support**: A large community surrounds Jenkins, offering numerous resources and support. Helpful for resolving issues or learning more about using Jenkins effectively.

6. **Independence from GitLab**: Jenkins allows separation of CI/CD processes from GitLab, useful for experimenting with different version control systems or maintaining a flexible setup.


## Installation of Gitlab (Community Edition)

While Jenkins is compatible with various version control systems, I'll focus on the installation process for GitLab , as that's what I use.

***If you're interested in setting up GitLab CE, you can check out my blog post on the GitLab installation process, which is available through this [link](https://utbazafar.com/posts/gitlab-runner/).***

## Easiest way to install Jenkins

Jenkins can be installed on various platforms, but since my homelab is primarily based on Ubuntu Server, I'll focus on the installation process for a Linux environment, specifically Ubuntu Server.

### Prerequisites

- To install Jenkins, you'll need a Linux virtual environment that operates continuously. I suggest using either a Raspberry Pi  or a hypervisor for this purpose.
- Ideally, aim for a setup with at least 4GB RAM and a minimum of 20GB of storage.
- That's it!!

### Prepare the Linux environment 

#### Download Updates and Dependencies

First, you need to create a separate user account with sudo privileges, as using the root account might risk breaking things.

```shell

# User these commands and follow the prompt
sudo adduser username
sudo passwd username
sudo usermod -aG sudo username

# Then exit from the root user and then login into new user acccount
exit

```

Download all the pending updates

```shell

sudo apt update -y

```

Upgrade all the packages to the last build

```shell

sudo apt upgrade -y 

```

### Installation Process of Jenkins and Java Env.

Installs the Jenkins package using the package manager. The apt-get install command resolves and installs all necessary dependencies for Jenkins.

#### Download Jenkins GPG Key:

```shell

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

```

#### Add Jenkins Package Repository:

```Shell

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

```

#### Update Package Index:

```shell

sudo apt-get update

```

#### Install Jenkins:

```shell

sudo apt-get install jenkins

```

#### Install Java Environment to Support Jenkins

These commands are used to update your system's package list, install the necessary Java runtime environment, and verify the installation by checking the Java version.

```shell

sudo apt update 
sudo apt install fontconfig openjdk-17-jre

```

### Start Jenkins Services

Use the following command to configure the Jenkins service to automatically start when the system boots:

```shell

sudo systemctl enable jenkins

```

Use the following command to initiate the Jenkins service:

```shell

sudo systemctl start jenkins

```

Check the status of the Jenkins service using the command:

```shell

sudo systemctl status jenkins

```

Lastly, retrieve the temporary password to login into Web app.

```shell 

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

```

## Activate Jenkins Projects 

### Let's login into Jenkins

Now that you obtain the temporary password by using command above, it's time to login into Jenkins and create a new account. 

- Open your web browser.
- Enter your server's IP address followed by :8080 in the address bar to access the Jenkins Web UI.
- When prompted, log in using the temporary password you obtained during the Jenkins installation process.
- Follow the on-screen prompts to create a new Jenkins account.

![Getting Started](/assets/content/jenkins/jenkins_get_start.png)

![Jenkins Login](/assets/content/jenkins/jenkins_login.png)


### Install the Gitlab Plugin

After that you login into Jenkins with your new account, it's time to install the Gitlab plugin so that we can start the integration in between you projects. 

- Navigate to the Dashboard.
- Click on "Manage Jenkins.
- Under "System Configuration," select "Plugins."
- Go to the "Available" tab.
- Search for the "GitLab Plugin."

![Jenkins Plugin](/assets/content/jenkins/jenkis_plugin.png)

### Generate Access Token in Gitlab

Before you start you configuring Gitlab plugin in Jenkins we need to create Access Token in Gitlab to create integration between Gitlab and Jenkins. 

- Click on your profile picture in the top-right corner of the GitLab interface.
- Select "Edit Profile" from the dropdown menu.
- In the left sidebar, click on "Access Tokens."
- Click on "Add new token."
- Enter a name for your token.
- Set the expiration date for the token.
- Check the "API" checkbox to grant the necessary permissions.
- Click "Create token."

Save the generated token securely, as you will not be able to view it again after this point.

![Access token](/assets/content/jenkins/jenkins_access_token.png)

### Configure Gitlab plugin in Jenkins

After generating your Access token in GitLab, navigate to the GitLab session under System Configuration in Manage Jenkins and enter the following information:

- Navigate to the Dashboard in Jenkins.
- Click on Manage Jenkins.
- Select System Configuration.
- Scroll down to find the GitLab section.
- Create a new connection by specifying:
	- **Connection Name**
	- **GitLab Host URL**
	- **Credential**
- To create credentials:
	- Click on the Add button under credentials.
	- Add a new credential by selecting Jenkins.
	- Under Kind, choose GitLab API Token.
	- Click on Add to save the credential.


![Jenkins Connections](/assets/content/jenkins/jenkins_connection.png)

![Jenkins Credentials](/assets/content/jenkins/jenkins_credentials.png)


## Connect Jenkins project with Gitlab. 

Since you have follow everything mentioned above, it's now time that we create a Jenkins project so that we run link with Gitlab project to activate the pipeline. 

- Navigate to the dashboard.
- Click on "New Item."
- Enter the project name.
- Select "Freestyle project."
- Click "OK."


![jenkins Project Generate](/assets/content/jenkins/jenkins_project.png)

### Configure the Jenkins project

Now you have to generate your new project, it's time to configure the project setting to activate. 

- Click on the Jenkins project you wish to configure.
- Navigate to the "Configure" option for the selected project.
- Scroll down to the "Build Triggers" section.
- Ensure that you select all the options listed in the provided screenshot.
- In the "Advanced" option, generate the key.
- click "Save."

![Jenkins Configuration](/assets/content/jenkins/jenkins_config.png)

### Integrate Jenkins projects with Gitlab using Webhooks. 

After generating the project key under Advanced settings in the previous step, it's time to connect the Jenkins project with the GitLab project using webhooks. This will allow the pipeline described in the Jenkinsfile in your GitLab project to trigger your pipeline in Jenkins.

- Navigate to your project in GitLab.
- Click on the "Settings" option.
- Under "Settings," select "Webhooks."
- Click on "Add new webhook."
- Fill out all the necessary information as shown in the screenshot.
- Click "Add webhook" to complete the process.

![jenkins Gitlab project](/assets/content/jenkins/jenkins_gitlab_project.png)

![jenkins Webhook](/assets/content/jenkins/jenkins_webhook.png)


## Test out the Pipeline.

After completing all the steps outlined in this guideline, you can test the pipeline by making changes to the code and pushing it. This will trigger the new pipeline in the Jenkins project. If there are no errors in the code, you will see a green checkmark indicating successful completion. You can also review the logs, changes, or other settings for debugging purposes.

![Running Pipeline](/assets/content/jenkins/Jenkins_mov_pic.png)

## Ending Point

There are countless ways to use Jenkins, but if you're into homelab and want to practice real world DevOps tools in your local setup, I highly recommend giving GitLab and Jenkins a try. They’re super easy to set up, as I’ve shown in this guide. That said, don’t just stick to my guide, take some time to research and explore other solutions that might better suit your needs. You can also experiment with automation tools like Terraform or Ansible to take your GitLab and Jenkins projects to the next level by automating your workflows even further.