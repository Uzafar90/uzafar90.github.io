---
title: My Homelab Journey
description: An overview of my homelab
date: 2024-05-28 12:00:00 -400
categories: [Homelab]
tags: [server, hardware]     # TAG names should always be lowercase
image:
    path: /assets/content/homelab/Homelab-Journey.jpg
---


## Why homelab?

What was the main reason for starting my homelab? I wanted to delve into the ever-evolving world of technology. Sometimes, in a company setting, it's challenging to gain hands-on experience due to either role-based constraints or a lack of opportunities. Building your own lab, whether with cloud VMs or bare-metal computing setups, offers the best way to learn about various technologies. Whether it's experimenting with different Linux distributions, setting up Windows environments, exploring containerization, or scripting, the possibilities are endless. These can be achieved with minimal setup, which we'll delve into further down the road.

## Homelab planning
![home](/assets/content/homelab/Homelab_diagram.jpg)

As we transition from discussing the "why" to the "how," it becomes crucial to plan the homelab meticulously to ensure we don't invest in unnecessary or expensive equipment that doesn't align with the main purpose. I've created a diagram that encompasses all the key technologies I aim to learn.


## Goals of my Hardwares choice 

Although you don't necessarily have to purchase additional devices since you can achieve your learning objectives on cloud platforms like Linode or DigitalOcean, I wanted to set up my own lab with budget-friendly physical devices. Instead of building my server from scratch, I went for an old prebuilt machine that was powerful enough to handle everything outlined in the diagram.

Here are some objectives for the hardware in my homelab:

- Cost-effective
- Capable of running all the tasks I throw at it
- Maintains performance levels over the long term
- Offers space for multiple hard drives for RAID configuration
- Upgradable in terms of CPU, memory, power supply, networking, and storage

Additionally, I aim to collect some single-board devices for small projects that won't heavily impact my budget.

## Purchasing the hardware

After planning and search through multiple sites, I stumbled upon a couple of devices that were powerful enough to help me achieve my goals. Here are the devices I purchased:

| Device Type         | Make         | Model           | CPU                                     | RAM    | Storage               | Quantity  |
|---------------------|--------------|-----------------|-----------------------------------------|--------|-----------------------|----------------|
| Main Server         | Dell         | Precision T5810 | Intel Xeon E5-2680 V3 2.5GHz 12 Core   | 128GB  | 2x1TB SSD, 1x4TB HDD  | 1  |
| NAS           | Datto        | S3B2000         | Intel Xeon D-1521 2.40GHz               | 32GB   | 2x4TB, 2x6TB          | 1 | 
| SingleBoard | Raspberry Pi | RP 3 Model B   | Quad Core 1.2GHz Broadcom BCM2837 64bit | 1GB    | 32GB MicroSD             | 8 |


These devices, while minimal, are incredibly powerful and fulfill every requirement for my homelab.

## Scalability Option

If you're someone who's either planning to enter or already exploring the world of homelab setups, I highly encourage starting small and gradually scaling up according to your needs. You don't have to invest heavily in industrial-level servers right away. Your first homelab server could be an old PC sitting in your closet or a budget-friendly Raspberry Pi device for learning Linux. You can even find free old devices by reaching out to local recycling companies. Once you've begun with small devices, you can then scale up your homelab according to your evolving needs and requirements. This incremental approach allows for practical learning and ensures that your homelab grows organically alongside your skills and interests.


## Main Operating System choice

IIn my opinion, selecting the right operating system for your homelab server is crucial. Nobody can afford an expensive device solely dedicated to running a single operating system without virtualization capabilities. Here are my chosen operating systems for my homelab:

#### Hypervisor
Proxmox is an excellent choice for setting up my homelab because it's free, eliminating concerns about spending money. It allows me to run various types of virtual machines and lightweight containers, making it convenient to experiment with different OS, whether it's Linux or Windows.

#### TrueNAS Scale
This serves as the primary OS for my NAS. It's the ideal option for my homelab because it's built on Linux, providing compatibility with a wide range of software and hardware. TrueNAS Scale utilizes ZFS, a robust file system that ensures data safety through features like snapshots and replication. My main intention is to use it for backing up all the VMs and data 

#### Containerization
For simplicity and efficiency, I have chosen Docker as my main choice of containerization platform due to its lightweight nature and highly efficient container technology. Unlike traditional virtualization, where each virtual machine requires its own operating system, Docker shares the main system's kernel, resulting in significant resource savings and faster startup times. 

## Additional Software running my homelab

While the choice of operating system forms the backbone of my homelab, it's the services I'm going to utilize within those operating systems that truly drive its purpose. Each service serves a specific function, channelizing the capabilities of my homelab. Here's a rundown of the services I want to tested out:

| Category         | Items                                 |
|-----------------|---------------------------------------|
| Virtual Machine  | Ubuntu Server 20.04, Kali Linux, Windows Server 2019, Windows 10 Pro, Ubuntu Desktop, Rocky Linux |
| Networking       | Packet Tracer, GNS3, OPNsense, Mesh VPN |
| Security         | Wazuh                                 |
| DNS              | PiHole, Bind9                         |
| Development      | Gitea, Gitlab, Code Server, Wordpress         |
| Automation       | Ansible, Terraform, Cloudinit, Python            |
| Monitoring       | Uptime Kuma, NetData, Grafana         |
| Reverse Manager  | NGINX Proxy Manager, Traefik          |
| Containerization | Docker, Docker Swarm, Kubernetes |



## Best Practice for Homelab

Before you started using VM in proxmox, it's crucial to follow the proper best practices to ensure a hassle-free process in the future.

#### Hardware Resources Allocation
- CPU Usage: Allocate specific CPU cores to individual virtual machines or containers for optimal performance.
- RAM Allocation: Avoid overcommitting RAM; assign only the necessary amount and leave some headroom for the host OS.

#### Storage Strategy
- ZFS Pool Setup: Utilize ZFS storage for VM storage to benefit from its features.
- SSD Cache: Employ internal SSDs for caching files, enhancing storage performance.

#### Networking
- VLANs: Implement separate networks for security and network management purposes.
- Firewall: Utilize Proxmox's built-in firewall to manage inbound and outbound traffic per VM.

#### Backup and Snapshots
- Automated Backups: Schedule regular automated backups, storing data on a separate disk mounted from NAS. Set these backups to run weekly, ensuring data safety and integrity.

## Document your process

I cannot stress enough the importance of documenting your homelab. As humans, our memory can only retain so much, and documentation becomes your best friend in the process. Begin by listing all hardware components, providing detailed information about servers, networking gear, and storage devices. Describe your network configuration thoroughly, including IP addresses, VLANs, and any special setups. Outline your server configurations, noting hardware specifications, installed operating systems, and additional software.

By maintaining this documentation, you'll gain a clear understanding of your homelab's architecture, making it easier to troubleshoot issues, plan upgrades, and ensure smooth operation. Whether it's a hardware failure, network reconfiguration, or software upgrade, having comprehensive documentation will save you time and effort in managing your homelab effectively.

## Conclusion

Homelab offers an excellent opportunity to enhance your learning and elevate your technical skills. The beauty of homelab is that you get to decide how you want to build it and what you want to learn. Don't wait for the perfect equipment or timing to start your homelab journey. Begin small, and then expand your environment based on your needs and interests.

I've planned my homelab with the goal of future-proofing my learning and experimenting with various technologies through a plethora of projects. Along the way, you may encounter hurdles, but don't let that discourage you. In homelab, everything is about creating, breaking, fixing, and carrying on. Once you dive deep into it, you'll find it difficult to leave because homelab itself is a vast world waiting to be explored. So, take the leap, start your homelab adventure, and enjoy the endless possibilities it offers for growth and exploration.
