---
title: Deploy DNS Server and Ad-blocker with Pi-Hole (Raspberry PI focused)
description: Learn how to set up Pi-Hole to turn your network into a powerful DNS server and ad-blocker. This will help you enjoy a smoother browsing experience by getting rid of annoying ads and boosting your privacy.
date: 2024-08-17 12:00:00 -400
categories: [Homelab]
tags: [linux, dns, ubuntu, pi-hole]     # TAG names should always be lowercase
image:
    path: /assets/content/pi-hole/Pi-Hole.jpg
---

Have you ever thought about hosting your own DNS server but felt overwhelmed by the idea of running multiple commands and dealing with troubleshooting issues? That's where Pi-Hole comes in to save the day.

## What is Pi-Hole? 

[Pi-Hole](https://pi-hole.net/) is a network-wide ad blocker that acts as a DNS sinkhole. It essentially blocks ads at the network level, meaning any device connected to your network will benefit from ad blocking without needing individual ad blockers installed.

## Pi-Hole Installation Requirement 

Pi-Hole is typically installed on a small, low-power device like a Raspberry Pi, but it can also run on other hardware or VPS. It intercepts DNS requests (nslookup - Name Server lookup) and blocks requests to known ad-serving domains, effectively preventing ads from loading. This not only speeds up your browsing experience but also reduces bandwidth usage and enhances privacy.

Minimal Requirement:
- 1GB Memory
- 8GB Storage

## Pi-Hole Installation Process

If you're setting up a Raspberry Pi, you'll first need to install an operating system. The recommended option is Raspberry Pi OS. You can easily download it from the official Raspberry Pi [website](https://www.raspberrypi.com/software/) and then use the provided software to flash it onto an SD card.

![Raspberry Pi OS Application](/assets/content/pi-hole/raspberry-pi-os.png)

### Configuration Options for Flashing an SD Card

- **Raspberry Pi Device** - Choose the type of Raspberry Pi device you have.

![Raspberry Pi Devices](/assets/content/pi-hole/rpi-device.png)

- **Operating System** - We recommend using Raspberry Pi OS Lite (32-bit), which doesn't include a desktop environment.

![Raspberry Pi OS](/assets/content/pi-hole/rpi-os.png)

- **Storage** - Pick the SD card inserted in your computer and flash the selected OS onto it.

- **Edit Settings** - Here, you can configure your settings, such as hostname, credentials, and timezone.

### Locate the Pi-Hole IP address in Router

Next, follow the steps outlined below to locate your IP address in the router and establish an SSH connection to your Raspberry Pi:

1. **Insert SD Card**: Insert the prepared SD card into your Raspberry Pi.

2. **Power On**: Power on your Raspberry Pi by connecting it to a power source.

3. **Access Router Interface**: Open your router's user interface. This is typically done by entering the router's IP address into a web browser.

4. **Find IP Address**: Locate the IP address assigned to your Raspberry Pi. This can usually be found in the list of connected devices or DHCP client list.

5. **SSH into Raspberry Pi**: Use the identified IP address to SSH into your Raspberry Pi. You can do this by opening a terminal on your computer and typing:

```Shell

 # Replace `<user> and <IP_ADDRESS>` with the actual IP address
 ssh <user>@<IP_ADDRESS> 

```

6. **Enter Credentials**: When prompted, enter the password for the `pi` user. The default password is typically `raspberry` unless you have changed it during setup.


### Now configure a static IP address on your Raspberry Pi.

Pi-Hole needs a static IP address to work correctly and manage the DNS server configuration on your Raspberry Pi.

#### Update Your System

First, we need to ensure our system is up to date.

```shell

sudo apt update
sudo apt upgrade -y

```

#### Find out the network interface 

Run the command below to identify the network interface, which is typically named "eth0" or something similar.

```shell

$ ip a 

```

#### Head to network interface to set IP

Once you've identified the network interface, the next step is to configure it with a static IP address. 

```shell

$ sudo nano /etc/network/interfaces/etho

# Make sure to include your information 
allow-hotplug eth0
iface eth0 inet static
address 192.168.1.10  # IP address you want assign to RPI
network 192.168.1.0   # Overall network 
netmask 255.255.255.0 # Subnet mask
gateway 192.168.1.1   # Router IP Address

```

Next, we need to reboot the machine to apply the configuration changes.

### Let Install Pi-Hole Package 

Now that our environment is configured on the Raspberry Pi, it's time to proceed with the installation of Pi-hole.

```shell

$ curl -sSL https://install.pi-hole.net | bash

```

The installer will walk you through the setup process, asking a series of questions such as which DNS provider you prefer (e.g., Google, OpenDNS), whether you want to install the web interface, and if you wish to enable the DHCP server. I recommend selecting *"YES"* for all options, as they can be modified later if needed.

Once the installation is complete, you will be provided with the Web UI address and a temporary password.

![Raspberry Pi OS](/assets/content/pi-hole/pihole-address.png)

### Set the custom login password.

In the terminal, execute the following command and follow the instructions to set your desired password.

```shell

sudo pihole -a -p

```


## Let's install Unbound to set up the DNS server.

Before accessing the web UI, we need to install another package called "Unbound," which handles the Recursive DNS resolver. Luckily, it can be installed with just a single command.

```shell

sudo apt update
sudo apt install unbound -y

```

Next, we need to insert the script into the configuration file to manage the DNS resolver. Simply open the Pi-hole configuration file in editor mode and input the script provided below.

```shell 

sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf

```

Enter the entire script without making any modifications, as everything will be managed through the Pi-hole UI.

```shell

server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0

    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes

    # May be set to yes if you have IPv6 connectivity
    do-ip6: no

    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no

    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"

    # Trust glue only if it is within the server's authority
    harden-glue: yes

    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes

    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no

    # Reduce EDNS reassembly buffer size.
    # IP fragmentation is unreliable on the Internet today, and can cause
    # transmission failures when large DNS messages are sent via UDP. Even
    # when fragmentation does work, it may not be secure; it is theoretically
    # possible to spoof parts of a fragmented DNS message, without easy
    # detection at the receiving end. Recently, there was an excellent study
    # >>> Defragmenting DNS - Determining the optimal maximum UDP response size for DNS <<<
    # by Axel Koolhaas, and Tjeerd Slokker (https://indico.dns-oarc.net/event/36/contributions/776/)
    # in collaboration with NLnet Labs explored DNS using real world data from the
    # the RIPE Atlas probes and the researchers suggested different values for
    # IPv4 and IPv6 and in different scenarios. They advise that servers should
    # be configured to limit DNS messages sent over UDP to a size that will not
    # trigger fragmentation on typical network links. DNS servers can switch
    # from UDP to TCP when a DNS response is too big to fit in this limited
    # buffer size. This value has also been suggested in DNS Flag Day 2020.
    edns-buffer-size: 1232

    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes

    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1

    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m

    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10

```

## Let's access the Web UI.

Now that Pi-Hole is installed, it's time to access the web UI and configure some additional settings to prepare it for the DNS server.

Open your browser and navigate to the URL provided in the installation completion prompt. Then, enter the password you previously set.

![Pi-Hole Web UI](/assets/content/pi-hole/pihole-login.png)


### Some configuration to enable DNS.

After logging into the Web UI, follow these steps to configure your Pi-Hole to use the unbound resolver:

1. **Log In**: Access the system by logging in with your credentials.
2. **Navigate to Settings**: Once logged in, go to the *Settings* section.
3. **Select DNS Tab**: Within the *Settings* section, find and select the *DNS* tab.
4. **Uncheck Both Boxes**: In the *DNS* tab, locate the two checkboxes and uncheck both of them.
5. **Enter Custom DNS**: In the *Custom 1* field, input the following address: `127.0.0.1#5335`.
6. **Save Changes**: Click the *Save* button to apply the changes.

![Raspberry Pi OS](/assets/content/pi-hole/pihole-config.png)


### Finally, Configure your Router with DNS address

To ensure all devices on your network use Pi-Hole for DNS, you need to modify the DNS settings on your router. Follow these steps to activate it: 

1. **Log In to Router**: Access your router's admin interface by entering its IP address into your web browser. This is typically something like `192.168.1.1` or `192.168.0.1`.

2. **Enter Credentials**: Log in using your router's admin username and password. If you haven't changed these, they are often found on a sticker on the router or in the router's manual.

3. **Navigate to DNS Settings**: Once logged in, find the section for DNS settings. This is usually located under settings like *Network Settings*, *Internet Settings*, or *Advanced Settings*.

4. **Set DNS Server**: Change the primary DNS server to the IP address of your Pi-Hole device. This is the static IP address you assigned to your Pi-Hole.

5. **Save Changes**: Save the changes to apply the new DNS settings. This might require a reboot of the router to take effect.

6. **Verify Configuration**: After the router reboots, check a device on your network to ensure it is using the Pi-Hole for DNS. You can do this by visiting the Pi-Hole admin interface and looking at the query log.


## Ending Points

In conclusion, by following the steps outlined above, you will have successfully set up your Raspberry Pi with Pi-Hole for ad-free browsing and configured it as a DNS server using the Unbound package for DNS resolution. While the Raspberry Pi is not the only option for running Pi-Hole, it is, in my opinion, the best and easiest way to isolate it from your overall infrastructure. This isolation simplifies future troubleshooting and provides a straightforward path for implementing multi-node load balancing with additional Raspberry PIs. 