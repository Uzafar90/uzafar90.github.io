---
title: Essential Linux Commands for Beginners
description: This blog post will cover the most essential commands that every beginner should learn.
date: 2024-07-24 12:00:00 -400
categories: [Linux]
tags: [Linux, Commands, bash]     # TAG names should always be lowercase
image:
    path: /assets/content/linux-commands/linux_bg.jpg
---

## Why you should learn Linux Commands?

As you all know, Linux dominates the world of servers globally. The majority of companies across the globe have data centers that host multiple servers, and approximately 90% of those servers run some flavor of the Linux operating system. Mastering Linux commands is a fundamental skill that can significantly enhance your productivity and efficiency, whether you're a System Administrator, Developer, or just a Hobbyist. Knowing how to navigate through the entire system using the CLI (Command Line Interface) or Terminal is very important.

## What is this CLI or Terminal jargon in Linux?

The terminal is a text-based interface used to access the Command Line Interface (CLI). It allows users to interact with a computer system through text commands rather than a graphical user interface (GUI) with windows, buttons, and icons. The terminal serves as a base for the CLI, which processes and executes the commands entered by the user. While the terminal is the application that displays the CLI, the CLI itself is the interface that interprets and runs the commands. This distinction highlights that the terminal is a tool for interaction, while the CLI is the underlying system for command execution. Using the CLI through a terminal can enhance efficiency and power, particularly for performing complex or repetitive tasks, thanks to its streamlined and flexible approach.

![terminal-ui](/assets/content/linux-commands/terminal.png)

## Essential Commands to operate in Linux.

Now that we've understand the importance of Linux as a server operating system and have a basic understanding of using the CLI to navigate through text-based commands, let's dive into some essential commands for navigating Linux. We'll cover commands in the following categories:

- **File and Directory Management**
- **System Information and Monitoring**
- **User and Group Management**
- **Networking**
- **Package Management**
- **Text Processing**
- **File Permissions and Ownership**
- **Compression and Archiving**
- **Disk Usage and Quota Management**
- **Security and Permissions**
- **System Maintenance and Troubleshooting**

### File and Directory Management

Involves commands and tools for creating, deleting, moving, copying, and organizing files and directories. It includes operations like listing directory contents, changing file permissions, and searching for files.

#### ls: List directory contents

```bash

# You want to list all files and directories in the current directory.
ls

```

```bash

# You want to list all files, including hidden files, in the current directory.
ls -a

```

#### cd: Change the current directory

```bash

# You want to change the current directory to /home/user/Documents.
cd /home/user/Documents

```

```bash

# You want to go back to the previous directory.
cd -

```

#### cp: Copy files or directories

```bash

# You want to copy a file named file.txt to a directory named backup.
cp file.txt backup/

```

```bash

# You want to copy a directory named project and all its contents to a directory named backup.
cp -r project/ backup/


```

#### mv: Move or rename files or directories

```bash

# You want to move a file named file.txt to a directory named archive.
mv file.txt archive/

```

```bash

# You want to rename a file from oldname.txt to newname.txt.
mv oldname.txt newname.txt

```

#### rm: Remove files or directories

```bash

# You want to remove a file named file.txt.
rm file.txt

```

```bash

# You want to remove a directory named old_project and all its contents.
rm -r old_project/

```

#### mkdir: Create directories

```bash

# You want to create a new directory named new_folder.
mkdir new_folder

```

```bash

# You want to create a nested directory structure parent/child/grandchild.
mkdir -p parent/child/grandchild

```

#### rmdir: Remove empty directories

```bash

# You want to remove an empty directory named old_folder.
rmdir old_folder

```

```bash

# You want to remove multiple empty directories named dir1, dir2, and dir3.
rmdir dir1 dir2 dir3

```


### System Information and Monitoring

Encompasses tools and commands to display and monitor system performance and status. This includes viewing real-time system information, checking system uptime, monitoring processes, and displaying hardware and software details.

#### top: Display real-time system information and process activity

```bash

# You want to monitor the real-time performance of your system, including CPU and memory usage.
top

```

```bash

# You want to sort the processes by memory usage.
top -o %MEM

```

#### htop: An enhanced version of top with a more user-friendly interface

```bash

# You want to use a more interactive and visually appealing tool to monitor system performance.
htop

```

```bash

# You want to filter processes by a specific user.
htop -u username

```

#### df: Report file system disk space usage

```bash

# You want to check the disk space usage of all mounted file systems.
df

```

```bash

# You want to display the disk space usage in a human-readable format.
df -h

```


#### uname: Print system information

```bash

# You want to print the system's kernel name.
uname

```

```bash

# You want to print detailed system information, including kernel name, version, and machine hardware name.
uname -a

```


### User and Group Management

Covers commands and tools for managing user accounts and groups. This includes creating, modifying, and deleting users and groups, as well as setting passwords and permissions.

#### adduser: Add a new user

```bash

# You want to add a new user named john.
sudo adduser john

```

```bash

# You want to add a new user named alice and specify the home directory.
sudo adduser --home /custom/home/alice alice

```

#### usermod: Modify a user account

```bash

# You want to add the user john to the sudo group.
sudo usermod -aG sudo john

```

```bash

# You want to change the username from john to john_doe.
sudo usermod -l john_doe john

```

#### passwd: Change user password

```bash

# You want to change the password for the current user.
passwd

```

```bash

# You want to change the password for the user john.
sudo passwd john

```

#### groups: Show group memberships

```bash

# You want to display the groups the current user belongs to.
groups

```

```bash

# You want to display the groups the user john belongs to.
groups john

```

#### chown: Change file owner and group

```bash

# You want to change the owner of file.txt to john.
sudo chown john file.txt

```

```bash

# You want to change the owner and group of file.txt to john and staff.
sudo chown john:staff file.txt

```

#### chmod: Change file modes or Access Control Lists (ACLs)

```bash

# You want to give the owner read, write, and execute permissions on file.txt.
chmod u+rwx file.txt

```

```bash

# You want to set the permissions of file.txt to 755 (owner can read/write/execute, others can read/execute).
chmod 755 file.txt

```


### Networking

Includes tools and commands for configuring and managing network interfaces, connections, and services. This covers tasks like setting IP addresses, configuring firewalls, and monitoring network traffic.

#### ifconfig: Configure network interfaces

```bash

# You want to display the current network configuration of all interfaces.
ifconfig

```

```bash

# You want to assign an IP address to a specific network interface.
ifconfig eth0 192.168.1.100

```

#### ping: Check the network connection to a server

```bash

# You want to check if a server is reachable.
ping google.com

```

```bash

# You want to send a specific number of ping requests to a server.
ping -c 4 google.com

```

#### netstat: Print network connections, routing tables, interface statistics, etc.

```bash 

# You want to display all active network connections.
netstat -a

```

```bash

# You want to display the routing table.
netstat -r

```

#### ssh: Securely connect to remote machines

```bash

# You want to connect to a remote server using a specific username.
ssh user@remote_server

```

```bash

# You want to connect to a remote server using a specific port.
ssh -p 2222 user@remote_server

```

#### scp: Securely copy files between hosts

```bash

# You want to copy a file from your local machine to a remote server.
scp localfile.txt user@remote_server:/path/to/destination

```

```bash

# You want to copy a file from a remote server to your local machine.
scp user@remote_server:/path/to/remote/file.txt /local/destination

```

#### wget: Non-interactive network downloader

```bash

# You want to download a file from the internet.
wget http://example.com/file.zip

```

```bash

# You want to download a file and save it with a different name.
wget -O newname.zip http://example.com/file.zip

```


### Package Management

Encompasses tools for installing, updating, and removing software packages. This includes managing repositories, resolving dependencies, and handling package configurations.

#### apt-get: APT package handling utility (Debian/Ubuntu)

```bash

# You want to update the package list to get information on the newest versions of packages and their dependencies.
sudo apt-get update

```

```bash

# You want to upgrade all the installed packages to their latest versions.
sudo apt-get upgrade

```

```bash

# You want to install a specific package, for example, curl.
sudo apt-get install curl

```

```bash

# You want to remove a specific package, for example, curl.
sudo apt-get remove curl

```


### Text Processing

Involves commands and tools for manipulating and processing text files. This includes searching, filtering, editing, and transforming text data using various utilities.

#### cat: Concatenate and display files

```bash

# You want to display the contents of a file.
cat filename.txt

```

```bash

# You want to concatenate multiple files and display their contents.
cat file1.txt file2.txt

```

#### grep: Search text using patterns

```bash

# You want to search for a specific pattern in a file.
grep "pattern" filename.txt

```

```bash

# You want to search for a pattern recursively in all files within a directory.
grep -r "pattern" /path/to/directory

```

#### sed: Stream editor for filtering and transforming text

```bash

# You want to replace all occurrences of a string in a file with another string.
sed 's/old_string/new_string/g' filename.txt

```

```bash

# You want to delete lines containing a specific pattern.
sed '/pattern/d' filename.txt

```

#### awk: Pattern scanning and processing language

```bash

# You want to print the second column of a file.
awk '{print $2}' filename.txt

```

```bash

# You want to perform a calculation on a column and print the result.
awk '{sum += $2} END {print sum}' filename.txt

```

#### less: View file contents interactively

```bash

# You want to view the contents of a large file interactively.
less filename.txt

```

```bash

# 1. Open the file with less
less filename.txt

# 2. less filename.txtWhile in less, type /pattern and press Enter to search for the pattern.

```



### File Permissions and Ownership

Covers commands to set and modify file and directory permissions and ownership. This includes changing read, write, and execute permissions, as well as assigning ownership to users and groups.

#### chown: Change file owner and group

```bash

# You want to change the owner of document.txt to alice.
sudo chown alice document.txt

```

```bash

# sudo chown bob:developers project
sudo chown bob:developers project

```

#### chmod: Change file access permissions

```bash

# You want to give the owner read, write, and execute permissions on script.sh, and read and execute permissions to the group and others.
chmod 755 script.sh

```

```bash

# You want to remove write permissions for the group and others on data.txt.
chmod go-w data.txt

```

#### umask: Set default file permissions

```bash

# You want to set the default file creation permissions so that new files are created with rw-r----- (640) permissions.
umask 027

```

```bash

# You want to set the default directory creation permissions so that new directories are created with rwxr-x--- (750) permissions.
umask 027

```


### Compression and Archiving

Includes tools for compressing and decompressing files, as well as creating and extracting archive files. This helps in reducing file sizes and bundling multiple files into a single archive.

#### tar: Archive files

```bash

# You have a directory named project that contains multiple files and subdirectories. You want to create a single archive file named project.tar to easily share or store the entire directory.
tar -cvf project.tar project/

# -c: Create a new archive.
# -v: Verbosely list files processed.
# -f: Specify the filename of the archive.

```

```bash

# You received an archive file named backup.tar and you want to extract its contents into the current directory.
tar -xvf backup.tar

# -x: Extract files from an archive.
# -v: Verbosely list files processed.
# -f: Specify the filename of the archive.

```

#### zip: Package and compress files

```bash

# You have a directory named documents and you want to create a ZIP archive named documents.zip.
zip -r documents.zip documents/

```

```bash

# You have an existing ZIP archive named archive.zip and you want to add a new file named newfile.txt to it.
zip archive.zip newfile.txt

```

#### unzip: Extract compressed files from a ZIP archive

```bash

# You have a ZIP file named photos.zip and you want to extract its contents into the current directory.
unzip photos.zip

```

```bash

# You have a ZIP file named archive.zip and you only want to extract a specific file named report.pdf.
unzip archive.zip report.pdf

```


### Disk Usage and Quota Management

Involves commands to monitor and manage disk space usage. This includes checking available disk space, analyzing file and directory sizes, and setting disk quotas for users.

#### df: Display free disk space

```bash

# You want to check the disk space usage of all mounted file systems.
df

```

```bash

# You want to display the disk space usage in a human-readable format.
df -h

```

```bash

# You want to check the disk space usage of a specific file system.
df /dev/sda1

```


#### echo: Display message on screen

```bash

# The echo command is used to display a line of text or a variable value.
echo: Display message on screen

```

```bash

# Display the value of a variable.
name="Alice"
echo "Hello, $name!"

```


### Security and Permissions

Encompasses tools and commands for securing the system and managing permissions. This includes setting up firewalls, configuring access controls, and managing encryption and authentication.

#### chmod: Change File Modes or Access Control Lists (ACLs)

The chmod command is used to change the file access permissions. Permissions can be set using symbolic or numeric modes.

Symbolic Mode:
- r: Read permission
- w: Write permission
- x: Execute permission
- u: User (owner)
- g: Group
- o: Others
- a: All (user, group, and others)

```bash

# Give the owner read, write, and execute permissions, and read and execute permissions to the group and others:
chmod u=rwx,g=rx,o=rx file.txt

```

```bash

# Remove write permissions for the group and others:
chmod go-w file.txt

```

Numeric Mode:
- 4: Read
- 2: Write
- 1: Execute

```bash

Set permissions to rwxr-xr-x (755):
chmod 755 file.txt\

```


#### chown: Change File Owner and Group

```bash

# Change the owner of file.txt to alice:
sudo chown alice file.txt

```

```bash

# Change the owner and group of project directory to bob and developers:
sudo chown bob:developers project

```

#### sudo: Execute a Command as Another User

```bash

# Execute a command as the superuser:
sudo command

```

```bash

# Edit a file that requires superuser permissions:
sudo nano /etc/hosts

```

#### Combining Commands for Security and Permissions

```bash

# Combining Commands for Security and Permissions
sudo chown alice:developers file.txt
chmod 640 file.txt

```

```bash

# Create a script that requires superuser permissions to modify system files:

#!/bin/bash
sudo cp myconfig.conf /etc/myconfig.conf
sudo chmod 644 /etc/myconfig.conf


```



### System Maintenance and Troubleshooting

Involves commands and tools for maintaining and troubleshooting the system. This includes checking system logs, managing services, performing file system checks, and diagnosing hardware and software issues.

#### dmesg: Print or control the kernel ring buffer

```bash

# You want to filter the kernel messages to show only those related to USB devices.
dmesg | grep usb


```

#### journalctl: Query and display messages from the journal

```bash

# You want to view logs for a specific service, such as nginx.
journalctl -u nginx

```

#### systemctl: Control the systemd system and service manager

```bash

# You want to start a service, such as nginx.
sudo systemctl start nginx

```

#### fsck: File system consistency check and repair

```bash

# You want to check and repair the file system on /dev/sda1.
sudo fsck /dev/sda1

```


## Additional Resources for Useful Commands

I've shared some useful commands that I use and learn from my daily projects. Additionally, there are a few resources I consistently follow to learn more advanced and complex commands to streamline my workflow, which I've listed below.

- [An A-Z Index of the Linux command line](https://ss64.com/bash/)
- [25 Basic Linux Commands For Beginners](https://www.geeksforgeeks.org/basic-linux-commands/)
- [geeksforgeeks Linux Commands](https://www.geeksforgeeks.org/linux-commands/)


## Ending Point

In my opinion, mastering linux commands can definitely enhance your productivity and efficiency when working with linux system, which is the king of servers worldwide. Whether you're a system admin, a developer, or just tinkering around for fun, getting the hang of Linux commands can help in your career in a long run.   

I'm currently learning and using all the commands mentioned above in my local environment to practice my Linux skills. These commands are quite beginner-friendly, and with a bit of daily practice, you can really improve your proficiency with them.