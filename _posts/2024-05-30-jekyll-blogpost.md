---
title: How to set up a Blog Site using Jekyll?
description: Learn how to set up a blog site on an Ubuntu Server using Jekyll.
date: 2024-05-30 12:00:00 -400
categories: [Development]
tags: [programming, template, ubuntu, ssg]     # TAG names should always be lowercase
image:
    path: /assets/content/Jekyll/Jeykll-SSG.jpg
---

## What is Jekyll? 
Jekyll is a static site generator that transforms plain text into beautiful static websites and blogs. It can be used for documentation sites, blogs, event sites, or any other type of website you need. It’s fast, secure, easy to use, and open source. It's the same SSG I'm using to host all my blog posts.


## Why did I choose Ubuntu Server for this?
Although I mentioned that Jekyll is a simple choice for your website, there aren't many tutorials or documentation available online that provide instructions on how to install all the necessary dependencies, such as a specific version of Ruby, Bundler, and Jekyll, on an Ubuntu server. I prefer to use Ubuntu Server VMs in Proxmox for my development environment. If you want to take this route, continue reading this blog post to see how I achieve this.

## Fire up the Ubuntu Server Env
First of all, you need an environment running Ubuntu Server. How you set this up is entirely up to you, whether on a Raspberry Pi, a bare metal server, or in a Ubuntu Server VM like I did. Just make sure you have enough resources to ensure ease of use for your initial setup and future projects.

## Terminal Setup
Once you Ubuntu Server fully baked, then you need to open the terminal in the separate environment. You need to ssh into the server and preferably through SSH Keys. In case if you're in the Windows environment then you can use Windows Subsystem Linux. You can follow the guideline on this link:
- [WSL Installation Guide](https://www.howtogeek.com/744328/how-to-install-the-windows-subsystem-for-linux-on-windows-11/)

## Commands for the installation

### Download Updates and Dependencies

First, you need to create a separate user account with sudo privileges, as using the root account might risk breaking things.

```bash
# User these commands and follow the prompt
sudo adduser username
sudo passwd username
sudo usermod -aG sudo username

# Then exit from the root user and then login into new user acccount
exit

```

Download all the pending updates

```bash

sudo apt update

```

Download and install the necessary libraries and compilers required for Ruby to execute (This command might take time to finish).

```bash

sudo apt install git curl autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev -y

```

### Install Rbenv to get Ruby

Rbenv is a lightweight Ruby version management tool that allows users to install and manage multiple versions of Ruby on the same system.

```bash

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash

```

  
Next, you'll want to add the downloaded dependencies to either your Bash or ZSH configuration, depending on the environment you're using.

```bash

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc

```

```zsh

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc

```

Confirm that the installation was successful by checking the version of the program.

```bash

rbenv -v

```

### Now it's time for Ruby 

For Jekyll to recognize Ruby, it's crucial to have a specific version of Ruby installed in your environment. Ruby version 3.1.2 or higher is required for compatibility with Jekyll.

First, check the all the version provided by the downloaded dependencies.

```bash

rbenv install -l

```

Now we need to choose 3.3.1 version from the list and install it.

```bash
# need to run these 2 commands
rbenv install 3.3.1
rbenv global 3.3.1

```

After that you need to reboot the machine and then verify the version installed. 

```bash

ruby --version

```

Final commands is to install the Jekyll itself. 

```bash

gem install jekyll bundler

```

## All set for the Jekyll Template 

Currently, I'm using [chirpy](https://github.com/cotes2020/chirpy-starter) template sourced from the linked Github repository and integrated it into my GitHub account using _username.github.io_. After forking all the files to your GitHub repository, the next step is to clone the repository into the development environment.  

![git-template](/assets/content/Jekyll/jekyll-template.png) 

![git-repo](/assets/content/Jekyll/jekyll-repo.png) 

### Clone the repo

Initial the git and clone the repo

```bash

git init
git clone git@<YOUR-USER-NAME>/<YOUR-REPO-NAME>.git

```

Next, we'll need to install all the dependencies that come with the project into your environment.

```bash 

cd repo-name
bundle init

```

After installing all the dependencies, we'll proceed with starting the development process to ensure everything is functioning correctly. Following this, we'll open the project in VScode to begin building our website.

```bash
# start the dev enviroment 
bundle exec jekyll s

# Shortcut to open in VSCode 
code . 

```

### Final Step

Before pushing your code to the Github, make sure that you enter the URL of your site where you plan to host the project in the `config.yaml` file. If you don't specify the site's URL, the GitHub Action will fail the deployment.

Also, if you're planning to host the site using GitHub Pages, make sure to change the "Build and Deployment" setting to "GitHub Actions" under the Pages tab in your repository settings.

![git-git-action](/assets/content/Jekyll/jekyll-git-action.png) 

## Ending Points

Sometime, we may not have the luxury of starting our project development from scratch. Therefore, it's important that we choose the option that involve less work and more result. Utilizing Static Site Generators (SSGs) such as Jekyll can we incredibly beneficial to build and share your content without spending too much time. Hopefully, by following these instruction, you'll be able to set up your development environment on Ubuntu Server and start developing those stunning websites.