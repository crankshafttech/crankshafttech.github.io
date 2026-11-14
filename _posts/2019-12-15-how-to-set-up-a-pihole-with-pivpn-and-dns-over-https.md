---
layout: post
title: "How to set up a PiHole with PiVPN and DNS-Over-HTTPS"
date: 2019-12-15 01:23:00 +0000
tags:
  - "Raspberry Pi"
  - "Tutorials"
  - "Servers and Networking"
redirect_from:
  - "/2019/12/set-up-pihole-with-doh-and-pivpn.html"
---

# This tutorial has has been updated! Please follow the new version [here](https://blog.crankshafttech.com/2021/03/how-to-setup-pihole-pivpn-unbound.html). more

I was recently trying to configure my PiVPN to work how I wanted after many months of PiHole running flawlessly on my home WiFi. I ended up breaking something, so I decided to start from scratch.

This tutorial contains instructions on how I have recently setup my PiHole with this functionality on a Raspberry Pi Zero W with an Ethernet connection to my router.

I have tried to make this as easy to follow as possible. If anyone has suggestions on how to make it better, please leave a comment.

Go to [this link](https://pi-hole.net/) if you want to read about what PiHole is.

![](/assets/images/pihole_logo.png)

_PiHole Logo_

## **Equipment Required:**

- Raspberry Pi (Pi 2 or above recommended, or a Pi Zero)
- Official power supply or alternative that can provide the required amount of power to the Pi
- Micro SD card (minimum 8GB, but 16 or 32 GB recommended)
- Optional: Micro USB to Ethernet adapter for Pi Zero and Zero W (Ethernet is STRONGLY recommended)

## **Setting up Raspbian:**

1. Download a copy of the latest version of Raspbian Lite (or the full version, but you don't need it) from the official site [here](https://www.raspberrypi.org/downloads/raspbian/).
1. Extract the zip file
1. Install Etcher or any other image writing tool and flash the image to the micro SD card. Etcher can be downloaded from [here](https://www.balena.io/etcher/). NOTE: If etcher freezes while writing the image or goes to a white window, close the program and run it as Administrator.
1. To be able to gain access to the command line remotely, SSH access is required. This can be done by accessing the Windows readable partition of the SD Card (/boot) and creating an empty file called "ssh" or "ssh.txt". This is case-sensitive. Now SSH access will be enabled.
1. Insert the micro SD card into the Pi, plug in the Ethernet and then power it on.
1. Find the IP address of the Pi by either going to your router/switch control panel and looking for "raspberrypi" in connected devices or by downloading and using an IP scanner like Angry IP Scanner (download [here](https://angryip.org/download/)). Make sure you enter the correct IP range of your network. Once again look for "raspberrypi".
1. Once you have found the IP address, go to Command Prompt or Terminal and enter:
ssh pi@[IP ADDRESS]
The default password is "raspberry".
1. Once your are logged in, it is strongly advised you change the default password. This can be done by entering:
passwd
This will ask for the current password, then the password you want to set.
1. One more recommended thing is to upgrade all your packages. This can be done by entering:
sudo apt update
sudo apt upgrade
This may take a few minutes, depending if you installed the lite version or the full. Once complete, type:
sudo reboot
1. Once it has rebooted, reconnect using SSH and type:
sudo raspi-config
This menu allows you to change many settings on the Raspberry Pi, including setting up a WiFi connection if your board supports it, as well as setting up localisation and many other things.
1. In this menu, use the arrow keys to navigate to "Localisation Options" and hit Enter.
1. Change your Locale and Timezone to make sure your time and location are set correctly.
1. Once finished, use the arrow keys to select "Back", then hit Enter.
1. Go to 'Network Options" and hit Enter.
1. Select "Hostname" and click Enter. This allows you to change the name or your Pi.
1. Read the warning/instructions and enter the new hostname. Once complete, hit Enter.
1. If it asks you to reboot, hit "Yes"

**Installing and Configuring PiHole:**

Copy this command into the Raspberry Pi terminal:
sudo curl -sSL https://install.pi-hole.net | bashPaste this into the SSH session with the Raspberry Pi and hit Enter.
NOTE: The command is from [this link](https://github.com/pi-hole/pi-hole/#one-step-automated-install) and does not include sudo on the PiHole website, however when I was installing it, it only seemed to work with sudo. 
An ascii PiHole logo should appear and it should start doing some checks. 
The installer should start within 1 - 2 minutes
If your Raspberry Pi has more than one network interface, select the one you are using. If it is Ethernet, click "eth0", while WiFi is "wlan0".
The installer will ask if you want to use IPv4 and/or IPv6 to block ads. unless you know what you are doing, leave it as the default. 
Next, it will ask if you want to use your current IP address and subnet as the static IP for the PiHole. Unless you know what you are doing, click "Yes"
NOTE: Once you know the IP address for your PiHole, it is recommended you set an IP Address reservation on your DHCP server, which is most likely your router. To find out how to do this, type your router model and "IP Address reservation" into Google. This will make sure the PiHole continues to function correctly. 
The installer will ask you to confirm your settings. If they are correct, press "Yes".
Next, it will ask you to select your Upstream DNS provider. unless you prefer something else, leave it as the default and hit enter. 
When it asks you to confirm, press "Yes".
For all of the rest of the prompts, leave it as the defaults and press "Enter" unless you know what you are doing. 
Once it is complete, it will show you the Web Interface password. Write this down somewhere. 
To change this password enter this command into the terminal:
pihole -a -p
Then follow the prompts. 
Once you are back to the terminal, open up a web browser and go to:
http://pi.hole/admin or http://[Pi IP Address]/admin
Login using the password you set. 

You have now successfully setup the PiHole! To use it to block ads on your devices, you can set it up 2 ways:

Set the DNS servers on your router to only include the IP Address of your Raspberry Pi
Set the DNS servers of each device to the IP Address of your Raspberry Pi.

Currently, it is only setup to work on your WiFi. To make it work externally, you need to install PiVPN. 

**Installing and configuring PiVPN to work with PiHole:**
Installing PiVPN is quite straight-forward, and it allows you to use an encrypted connection to connect to your PiHole, as well as optionally devices on your home network. It can also be configured to encrypt and send all data through the VPN to make it look like you are at the location of the Raspberry Pi.

NOTE: For your VPN server to function its best, it is recommended you get a static Public IP address from your ISP, or (like me) your Public IP from your ISP needs to be mostly static and not change too often.

Open an SSH connection to your Raspberry Pi and enter this command:
curl -L https://install.pivpn.io | bash
This command downloads the setup script for PiVPN and runs it. 
If it asks for your password, type it in and hit Enter
After this, a window should pop-up saying that it will install a VPN server on your Pi. it will next warn you that you need a static IP address for it to work properly. NOTE: This was setup in the PiHole section, but if you need to set it up now, refer to step 6 above. 
Next it will ask you to select your network interface. If the Pi is connected over WiFi (NOT recommended), select wlan0 with by clicking Space, or eth0 if you are using ethernet. Once done, hit Enter.
Now it will ask you to select which user will store your OpenVPN configurations. Leave it as the default "Pi" unless you know what you are doing.
It will now ask you if you want to enable unattended upgrades. It is strongly recommended you enable this as it will ensure vulnerabilities are patched. 
It will ask if you want to use UDP or TCP. Like it says, only use TCP if you know what you are doing. 
Now it will ask you to select what port you would like to use for your VPN. It is recommended you change it slightly to make it harder for attackers to find your VPN server. I just add a random number on the end. NOTE: You need to remember this port number for later.
Next it asks what level of encryption you want. Leave this as the default as increasing encryption will make it more secure, but it will slow the connection down. 
Click enter to this window.
Next it will ask if you want clients to connect directly to your Public IP address or through a domain name. leave it as the default unless you know what you are doing. 
Now you need to select your DNS Provider. Select any as this will later be changed to the PiHole. 
PiVPN should now be installed on your Raspberry Pi!
It will ask you to reboot. This is recommended. 
Once it has rebooted, reconnect over SSH.
Next we need to setup a profile for you client device. This can be done by entering:
pivpn add or pivpn -a
Enter the required credentials.
You can create as many profiles as you want. It is recommended you create one profile per device. 
Now the profile has been created, you need to copy it off of your Raspberry Pi. There are multiple ways to do this, but the way I prefer (and is the easiest) is to use Filezilla. This can be downloaded from [here](https://filezilla-project.org/download.php?type=client) and installed on any PC running Windows, MacOS or Linux. 
Once Filezilla is installed, enter the IP address of your Pi into the Host box, the username of your Pi into the next box, that user's password into the last box, and 22 into the Port box. Then hit Quickconnect.
Navigate to the folder called "ovpns" and double click it
This folder should contain your client ovpn files. To download them, open the folder you want to save it in on the left and then double click the file on the right. It will then download to the folder on the left automatically. 
If this file is for a phone, you can now email it or transfer it to your phone however suits you. 
To use the icon file, download the OpenVPN Connect, OpenVPN for Android or OpenVPN desktop client programs and import the file. 
Now we have a config file on our client device, we need to configure the server itself. To do this we need to enter this command:
sudo nano /etc/openvpn/server.conf
This will open the server configuration file.
Remove all the lines which include "dhcp-option" and replace them with one line:
push "dhcp-option DNS [Pi Local IP Address]
This now makes your PiHole the DNS server for you PiVPN. 
Click Ctrl+X, followed by Y and Enter to exit the editor and save the changes.
To restart the VPN server and activate the changes, enter this command:
sudo systemctl restart openvpn
The VPN server will still not work as there are 2 more steps:
1. Go to your Router and find the Port Forwarding settings. In here, we need to forward the port we used for the VPN server to the Raspberry Pi with UDP. This is most likely in the Advanced settings. This is different for every router so a quick manual read or Google search should show you how to do it on your setup.
2. We need to change the listening settings on the PiHole. This done by navigating to the PiHole Admin Console, going to Settings, then the DNS tab. There is a section labelled 'Interface Listening Behaviour". In here, we need to select "Listen on all interfaces, permit all origins". PLEASE NOTE: Only do this if your Raspberry Pi is not accessible to the internet and is secured inside your WiFi network. If not, anyone could exploit your Raspberry Pi. 
If you have ticked the box, scroll down to the bottom and click Save. 

If you try to connect to your PiVPN, the client should be able to connect and go to websites. If everything is working, congratulations! If not, check your ISP doesn't use CG-NAT or block the port you are using. 

If it still doesn't work, leave a comment below. 

The current setup is forwarding all traffic through the VPN. If this is what you want, you are done. If you want only DNS queries, or only DNS and local IP addresses to be forwarded, read on.

**Setup 1: DNS queries only:**

Insert a "#" at the start of the line from the config file from step 23 that reads:
push "redirect-gateway def1"This line forwards all traffic through the VPN. 

Now only DNS queries will go through the VPN! To apply these changes, enter the command from step 26 above. 

**Setup 2: DNS queries and local IP addresses only:**

Follow the step from Setup 1.
Add a line into the file that reads:
push "route 192.168.x.x 255.255.x.x"
The first number is the IP range of your local network. If devices use the IPs 192.168.0.1 - 255, enter 192.168.0.0. If it uses 192.168.1.1 - 255, use 192.168.1.0.
For others, follow the pattern.
The second set of numbers is the subnet mask. It begins with 255.255 but can end differently. Mine is "255.255.255.0" but some peoples is "255.255.255.255". To find yours, go to your router setup and it will say the subnet mask. Alternatively, find a device connected to the network and you will find the subnet mask in the network configuration settings. 

Now only local IPs and DNS should go through the VPN! Once again, to apply these changes, follow Step 26 from above. 

**Optional: Installing and configuring Cloudflare DNS-Over-HTTPS:**

This step is only necessary if you want your DNS queries to be encrypted once they have passed through the PiHole. If you don't need this, ignore this section, as your setup will still function as intended. 

Instructions source: [link](https://docs.pi-hole.net/guides/dns-over-https/)

Download link source: [link](https://github.com/cloudflare/cloudflared/issues/38#issuecomment-533908555)

The download link above is unofficial, as the official one from the GitHub has a Segmentation Fault on most Raspberry pi's. I have used the download above and had no issues. 

Go to this [link](https://hobin.ca/cloudflared/releases/), scroll down to the very bottom and find the latest version. Click on that
Find the file that ends in ".tar.gz". Right click it and click "Copy Link Address" (Chrome), or a similar button in your browser. 
Go to your ssh session with the PiHole and paste it onto the end of this command:
wget [paste URL here]
Once that is complete, copy this command, substituting in the name of the downloaded file:
tar -xvzf [downloaded-file-name].tar.gz
Copy the following commands, one after the other (no substitution is required):
sudo cp ./cloudflared /usr/local/bin
sudo chmod +x /usr/local/bin/cloudflared
Enter this command:
cloudflared -v
If everything is working correctly, it should return something like this:
cloudflared version 2019.11.3 (built 2019-11-21-0625 UTC)
To create the cloudflared user to run the daemon, run this command:
sudo useradd -s /usr/sbin/nologin -r -M cloudflared
Next, you need to create a config in the file /etc/default/cloudflared. This is the command cloudflared uses on startup. It can be created by entering:
sudo nano /etc/default/cloudflared
Enter in file:CLOUDFLARED_OPTS=--port 5053 --upstream https://1.1.1.1/dns-query --upstream https://1.0.0.1/dns-query
To exit and save changes, press Ctrl+X, followed by Y, followed by Enter. 
Change the file permissions so cloudflared can access it:
sudo chown cloudflared:cloudflared /etc/default/cloudflared
sudo chown cloudflared:cloudflared /usr/local/bin/cloudflared
Next you need to create the systemd script to allow you to control cloudflared. This is done by creating the file /etc/systemd/cloudflared.service and entering some text:
sudo nano /etc/systemd/system/cloudflared.service
Enter in the file:[Unit]
Description=cloudflared DNS over HTTPS proxy
After=syslog.target network-online.target

[Service]
Type=simple
User=cloudflared
EnvironmentFile=/etc/default/cloudflared
ExecStart=/usr/local/bin/cloudflared proxy-dns $CLOUDFLARED_OPTS
Restart=on-failure
RestartSec=10
KillMode=process

[Install]
WantedBy=multi-user.target
Follow step 10 to exit nano
To enable cloudflared, enter these 3 commands:
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
sudo systemctl status cloudflared
Cloudflared should now be working. It is functioning if the third command above doesn't show any errors. If it does:1. Enter this command:
sudo systemctl restart cloudflared
Once this has completed, enter the status command from the last step. If errors still appear, try a few more times. 2. Make sure your time is set correctly in raspi-config under "Localisation Options".
3. According to [this issue report](https://github.com/cloudflare/cloudflared/issues/23), this behaviour can be caused by the internet dropping out. If you have unstable/unreliable internet, that might be your problem.
If it still doesn't work, leave a comment below. 
To set the PiHole DNS to cloudflared, go to the PiHole Admin Console, click on 'Settings" on the left, go the the "DNS" tab and enter this into the custom DNS 1 box:
127.0.0.1#5053
Then untick all the other boxes and tick the custom one. 
Scroll down to the very bottom and hit Save.

Cloudflared should now be working with PiHole clients! You can test it by going to [this site](https://www.cloudflare.com/ssl/encrypted-sni/). It should show a green tick unless you are running DNSSEC as well, which it will show a yellow question mark. 

****
Hopefully this tutorial has helped you setup your PiHole, PiVPN and optionally DNS-Over-HTTPS.

Please leave a comment if you have any feedback or questions.

Once I create more tutorials, you check them out [here](https://blog.crankshafttech.com/search/label/Tutorials).
