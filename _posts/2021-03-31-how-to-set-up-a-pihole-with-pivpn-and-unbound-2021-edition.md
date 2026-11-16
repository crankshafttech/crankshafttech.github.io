---
layout: post
title: "How to set up a PiHole with PiVPN and Unbound - 2021 Edition"
date: 2021-03-31 05:40:00 +0000
tags:
  - "Raspberry Pi"
  - "Tutorials"
  - "Servers and Networking"
redirect_from:
  - "/2021/03/how-to-setup-pihole-pivpn-unbound.html"
---

![](/assets/images/pihole_logo.png){: width="200" height="300" }

Since the [last time I wrote about PiHole in 2019](/posts/how-to-set-up-a-pihole-with-pivpn-and-dns-over-https/), that tutorial has accumulated over 25,000 views and is by far the most viewed post on this blog. Even to this day, that tutorial racks up hundreds of views every month, even though it is sorely out of date.

A lot has changed since 2019, including a global pandemic, and more relevant to us, the ease of configuring and installing a network wide ad-blocker. As I alluded to earlier, the old tutorial is sorely out of date, making use of OpenVPN, and DNS-over-HTTPS, both of which are much easier to configure or have been superseded since.

I realised how much easier it is now when I moved my PiHole and VPN to a VM on my new Homelab, which made me realise how sorely an update was needed.

Without further ado, here is the much simplified 2021 edition of installing PiHole and PiVPN on a your Raspberry Pi or any Debian-based Linux Distro. 

If you need instructions on how to install Raspbian, please read that section of the [old tutorial](/posts/how-to-set-up-a-pihole-with-pivpn-and-dns-over-https/) as nothing notable has changed on that front. If you are using something other than a Raspberry Pi, I recommend using [Ubuntu Server](https://ubuntu.com/download/server), although any Debian-based distribution should be able to follow this tutorial.

All screenshots are from Ubuntu Server 18.04 LTS, although it should look mostly identical on other platforms, including Raspberry Pi.

## Installing PiHole

Installing PiHole first is crucial, as it allows much of the auto configuration later to occur. 

1. Copy and paste the following command into the terminal on your system:

    ```bash
    curl -sSL https://install.pi-hole.net | bash
    ```

2. If the installer asks for a root password, provide it. 

    ![](/assets/images/root_pw_pihole.png)

3. After clicking ENTER a few times, you should be presented with a list of Upstream DNS Providers. This doesn't matter for now if you plan to install unbound, otherwise select whichever one you prefer. This can be changed later from the Web UI. 

4. Leave the default list selected unless you prefer others.

5. Leave both IPv4 and IPv6 selected unless you know your network only uses one or the other.

6. Next you will be asked about setting a static IP. Select yes.

    ![](/assets/images/static_ip_q.png)

7. Make note of the warning, as you should go to your router and set it as a static IP in the DHCP server so another device doesn't get given it.
Go to your router's website to find out how for your model.

8. Select On for the web interface and web server / PHP when prompted.

9. Select On for log queries if you're not a privacy freak, and then select your level of logging you are comfortable with. This will break some charts on the Web UI if 0 is not selected.

10. Now the installer will proceed to install the Web UI and other components.

11. Once the installer finishes, now is a good time to reset the Web UI password from the default. This can be done with the following command:
    `pihole -a -p` followed by your new password twice.

12. Once this is done, you can login to the Web UI with it's IP address and use the password you just created.

13. Once there, make sure everything shows up correctly and there are no errors.

    ![](/assets/images/pihole_webUI.png)

## Configuring devices to use PiHole

Now that it is setup, you need to configure your devices to use PiHole. This can be done in one of 3 ways:

1. Network-wide at the router level

2. Network-wide with the PiHole as DHCP

3. Specific devices only

They are all easy to set up, but some take longer than others. 

### 1. Network-wide at the router level

1. Open your router's web control panel. The login can usually found on a sticker on the router.

2. Go to the LAN section, then the DHCP server.

3. Find the DNS server under this, and enter the IP address of the PiHole.
    (If you haven't set a static IP, this is also likely the place to do so)

4. Click save and it should propogate to all your devices the next time they connect.

### 2. Network-wide with PiHole as DHCP

One awesome feature is that you can [set the PiHole as the DHCP server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026) which assigns IP addresses to devices on the network, which also solves the static IP issue.

This method takes a few more steps, but is still quite simple.

1. Open your router's web control panel. This can usually found on a sticker on the router.

2. Go to the LAN section, then the DHCP server.

3. Disable the DHCP server. Make sure it is off before proceeding.
    NOTE: This will prevent devices from easily connecting to the network. You will need to manually assign IPs **for the time being**.
    
4. Go to the PiHole Web UI and login

5. Go to Settings and then the DHCP tab.
    
    ![](/assets/images/PiHole_DHCP_Tab.png)

6. Toggle the DHCP Server to ON.

7. Make sure the Gateway/Router and IP range are set correctly. If you have a simple setup, it should be correct.

8. Scroll to the bottom and click Save. 

9. Now if you connect a device to the network, it should be assigned an IP address automatically.

### 3. Specific Devices Only

This is by far the easiest to configure, but can take the longest and varies between devices. Go to your network settings and find the WiFi or Ethernet connection you have.  However your device lets you, change **both** DNS servers to the IP Address of the PiHole. Click Save.Alternate Specific Device OnlyOne awesome feature that has been added to PiHole as part of version 5 is the ability to set different blocklists for different devices. One way you could set devices to not use PiHole is follow method 1 or 2 and add the devices to a group that has no blocklists or vice versa. This means that on devices with no lists everything is forwarded directly to the Upstream DNS provider with no blocks. These steps can also be used with method 3 if you want different lists for different devices.
Go to the PiHole Web UI and go the the Group Management section. Select Groups.

![](/assets/images/Blocking_group.png)
Create a group for the device/s you do/don't want using PiHole's blocklists.Go to the Clients section. [![](/assets/images/PiHole_Domains_Clients.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiqRBokGW_OqNR3EFLzqNWhR1L5hg0ngMGf0erZBCai9Fr1fafHLonbIS1veNPcmICeaVE30EVLamc1v-fUi52RcO_3eBNKiYG60wpz_3nXa8wmtA9HF_OpDG5UC6D4iazoUpRHn1JQIlc/s1250/PiHole_Domains_Clients.png)Select a client device from the dropdown that you want to add to the list and click Add.Click on the group assignment and select the group/s you want the device to belong to. These changes automatically save as you go. If you want to add list specific black and whitelists, this can be done under the Domains section. [![](/assets/images/PiHole_Domains_Groups.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg53ZA6PT2xaiZuqtvNM8EDR0ohcSixCdxJAG4Pck3QMjXk-9S3sQooHVNEdI8SvmhiW75zTbLOf8rQnswLB-LtsX0dSjwUHsbU3ZVrvcnsCcOsn49D1nrhlO5WqqLIiuc7zHnP8jWXNBw/s1258/PiHole_Domains_Groups.png)To add or remove blocklists/adlists, go to the adlists section and enter the URL to the list. [![](/assets/images/PiHole_Domains_Adlists.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjXpRBiUrm2pPv7rZ7-Cg5LC5COf1aL0RYTiivR3jWhM7PLnPoUG8ItKJ0qrQZh2U4qc4XA5-w0YvKSuzKQdOXGbo0jUgqwjhiJkTwjI_8naaEaOVCjxSvEc9HsMkwlx1tTSWSP2y0-5-I/s1250/PiHole_Domains_Adlists.png)Select the group/groups you want the list to be used or not used by. PiHole installation is now complete! For more information on things you can change, like local DNS, check out the PiHole documentation at [https://docs.pi-hole.net/](https://docs.pi-hole.net/). [![](/assets/images/pivpnlogo.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEitrY1Ktfulwhx2eL-IeIrw2zJyYo5lX7_qxBR0s0Q1T4JRCCHOmrZr8hehU5luWg2qzYhZLQsi73Ye_OtJ_URH9XTcLY635PSayvRPIdmEqwhrE19yWUEjZ77u4n58F6LqvYPX8MznHCI/s512/pivpnlogo.png)
Installing PiVPNPiVPN has changed a lot since 2019. Since then, Wireguard has been released, which is a simpler, and better in every way VPN protocol. They also added auto-PiHole configuration so that profile names appear in the Web UI and you don't have to manually configure files to make it work. Also, split tunnels are much easier to configure (more on them later).**NOTE:**If you cannot get a static IP from your ISP, or your dynamic one changes frequently, I highly recommend you looking into a Dynamic DNS service like [afraid.org](http://afraid.org)  which provides you with a URL to your IP and a script to update it to always point to the right place.
If you go this route, enter the URL created in Step 10 under the DNS Entry option.
Run the following command in the terminal:
curl -L https://install.pivpn.io | bashEnter the root password when asked. Eventually a screen should appear for the Installer. [![](/assets/images/PiVPN_Intro.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjrUGf885p-DtE-eDraaIAn3J-JZub62y7-zmnO0OcbJ3ZybYVn6TyY1RTGWtkB_AqkkjzyGTuLcu6aLTwKpYFE0mu88oJ4ambqZ9MH22fecnzAMdIJI-ZqqTynCng5T8ibq25NRLtQgk4/s737/PiVPN_Intro.png)Next it will start setting up a Static IP.
If you are on anything other than Raspbian, it will show the following prompt, which is not a problem. [![](/assets/images/PiVPN_Static_IP.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjUPI6rnSqw7MDZmvLd8GBqt13Wo-0GDmrSX6Nq1Xo8EzUwX4vbFlYnj0_sS7R1XonuVhUywgGj-aYtn3aA98KIVy0od4FJikDkskjz5y38FCKFCXQbHfMIakwIDZf_giY1RsXNWIh3Atg/s737/PiVPN_Static_IP.png)After that it will ask which user should hold the VPN configurations for clients. Select the default unless you've configured a separate user for this purpose. Next you will be asked if you want WireGuard or OpenVPN. For this use case, I **strongly** recommend WireGuard, but if you want to stick with OpenVPN, this is where the tutorial ends and you need to look at the old tutorial. After the packages are installed, it will ask for a port number. You can leave it on the default, but changing it can slightly improve security. Make sure whatever you set it to is not used by another service. Also make sure you remember what port you set for later.
Confirm the port is correct. Next it should detect the PiHole installation and ask if you want it to be used for the VPN. Select Yes. [![](/assets/images/PiVPN_PiHole_Detection.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhK9tS-h-NagI-6DseoTuT4oK_NrCp83IlgbbNJZMEEeg4Zu_2CB70LY0YRQvTXR5ect7jtMSkUBY-j6QjiU7fwE31sWHL6ehfp4AsF_YlRyZ2kLyZrUpEEVjIl_frxOypWivMsN_DBYZ8/s737/PiVPN_PiHole_Detection.png)Next it will ask if you want to use the IP or a URL to connect to the VPN. Unless you have a URL, select the IP Address. [![](/assets/images/PiVPN_Public_IP.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiufDUXIK8cZ9BwuqIbhc-t3Mn0aorLM3gK9NlQRROqjpK8GL4n2UgwDhgLUeFXFTCS8vwNclYb7LTyIgMfGLUoyc3dLbNPCHnTcjvwTkZArqjqVNsO74m0xsr0ypBVTXw5LbOUrc454Og/s737/PiVPN_Public_IP.png)PiVPN will generate the Server Keys. Next it will ask about unattended-upgrades. It is recommended you enable it, as otherwise your installation may be vulnerable to attacks. Now the install is done, it tells you how to create profiles. [![](/assets/images/PiVPN_Complete.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgNTVPd6lAbtddCxLNjviMhIyD_HDYQAyC9ISS7Y4mj86oh8XYdAOfcIeNHdoo5hXygdVeDR9eeaK4tU9uxP2x6R1Zhs1231TJn2Ga38XlLLfgZVTENdKmgD5xxk1pUBwzbm9Upsyn1Rn4/s737/PiVPN_Complete.png)The server will now reboot. 
[![](/assets/images/PiVPN_Reboot.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjwKb5w_-XZZFTBq3OZpJ3URy9vvhqU2NsetWNiBN6yM4qqMx-NNM9Yf191SMOklAfgwgTXVkEbukPx_hPqfhS56umLL291UhmdbJFRBMUFw3YoXvwhiJHgs4J8qp_8_dRLGAgd5F7uIpw/s737/PiVPN_Reboot.png)
Once rebooted, log back in and run the following command:
pivpn -a Enter the root password if prompted.Enter a client name.
[![](/assets/images/PiVPN_Profile_Add.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhw5YJRSWAdZ7-NwyJguM2Xu3tmt0j5dAZkXa5GbOqk4AhgU-3P5_lvdt6DEYQcQzznXgXWtW0QwjeXJLwkFtexzN_Qhk-kD6JOOb8n4PUmFW75jaEbjijvggUQE621DrKjr7CfJAf-Xsg/s737/PiVPN_Profile_Add.png)Now it will show where the file is saved. There are many ways to load the configuration onto your client device. If it is a **mobile**, you can use the handy QR Code feature to load the config file onto your device. To do this, simply:Run the command pivpn -qr and enter the profile name/number when prompted. Scan the QR that appears on the screen with your WireGuard app. **NOTE:** The QR code is based off the config file in /etc/wireguard/configs and not the one in the home directory. If you want the QR code to include config file changes, you need to change that file instead.   If you are using a **computer** or a device that cannot scan QR Codes, you will need to manually copy the configuration file to it. It is recommended to copy the version in /etc/wireguard/configs, but the one in ~/configs would work too.
Setting up the TunnelThere are a few different ways you can configure a VPN. One is a Full Tunnel, where everything goes through it, although it is much slower. The second method is a Split Tunnel,  which is where only some traffic goes through it. This is optimal you only want DNS queries going to PiHole, while everything goes over the normal internet. This can also be used to access local IPs and therefore devices remotely. A Full Tunnel is configured by default, but a split tunnel requires a small amount of effort to work. These steps can be followed from either in the WireGuard program or by opening the file in a text editor.
 Locate the line beginning with AllowedIPs, and remove everything after the =. Now we enter the IPs/ranges of IPs we want forwarded over the VPN. To make sure DNS works, Enter 10.6.0.1/24 or 10.6.0.0/24, the second allowing access to all other devices connected over the VPN, while the first only allowing connections specifically to the PiHole. 

![Specifics](/assets/images/PiVPN_IP_Specific.png)

_Specific Devices/IPs_

If you want to be able to access local devices, enter their IPs followed by /24 or a range to pass through multiple, with the form *.*.*.0 followed by /24. Substitute the * for your IP range. 

![Ranges](/assets/images/PiVPN_IP_Ranges.png)

_Ranges of Devices/IPs_

Load the file into your VPN program and you should be good to go!Setting up Port ForwardingThis service requires a port to be opened to the internet, and this may not be possible in all cases depending on your ISP. Go to your router's Web UI.
 Find Port Forwarding, which is likely around a WAN section. This varies between models and manufacturers. Use the port you assigned earlier as the internal and external ports and forward both TCP and UDP through it. If it asks for a internal device IP, enter the Pi's IP. Click save and attempt to use your VPN. If the VPN refuses to work, your ISP may use CG-NAT (Carrier-Grade NAT). You will need to contact them and attempt to get it disabled, which may not always be possible. **Without it disabled, Port Forwarding is not possible**.
Installing Unbound
Unbound is a recursive, caching DNS resolver that allows for fast, secure DNS resolution with support for features like DNS-over-TLS and DNS-over-HTTPS. More info on how it works and the source of these instructions can be found [here](https://docs.pi-hole.net/guides/dns/unbound/). Command-line
Install unbound using the following command: sudo apt install unbound. [![](/assets/images/Unbound_Install_1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhCns7F7XCIcl9a1vyTAk7C2xq-9m0m7aUKBvInFzGaTjfs4y68BH35yHacziPI8DGRIsoaFBAWQuwV-Xg_Wvq1j14iJwEF2QTLtZ7-aEcKNa4v8tkx_nBnLggS1Z67jbM6K-ofwlcvigU/s979/Unbound_Install_1.png)Enter the sudo password when asked.Press Y to confirm the installation. Once the install is finished, check that  dns-root-data is installed with the command apt show dns-root-data. It should show info about the package. [![](/assets/images/Unbound_dns-root-data.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRdF-D2u-j6KzmYEACFlJadupxVKwnoijpWKwghtJAO-LVx05woayOTCcX75SzbOhVYbvNcJd0ajBJTHkNlun58lX6gvA_7wR7Y3u51eGr9H7OaLer_Dd5hTCzTlLc54GwWWxz96jiP4g/s979/Unbound_dns-root-data.png)If the package is not on your platform, run the following command every 6 months:
wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints
Open the configuration using the following command: sudo nano /etc/unbound/unbound.conf.d/pi-hole.confPaste the contents of the following link into the file: [https://www.crankshafttech.com/unbound-config.txt](https://www.crankshafttech.com/unbound-config.txt)[](https://www.crankshafttech.com/unbound-config.txt)[![](/assets/images/Unbound_Config_File.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7RDLn0yi9Uyx6qoSUSZqstHsKtytYNfFpqUEn4GUXttqlGoqivNUjHFpNfCNDpkces4lNiedbjNT2VA9zpVpRIdVwUschFPLQ3H936oOyDLXLRpmV_LHfxmi4kCSHXSxlpCsxboztowg/s979/Unbound_Config_File.png)Close and save the file.  Restart unbound to apply changes: sudo service unbound restartYou can test if it is working using the following command: dig pi-hole.net @127.0.0.1 -p 5335 [![](/assets/images/Unbound_Dig_test.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh8Zn4Z7s91Ek0w8gUyU5iisCBWvOtFUWhxDygb4IRzV2M0H-AqGaMBtEufpgiKRXK-BLaR97-KZxWZ5ErWfSdwpv5dPtuSt-zveeU4qZmKWEpm6odCz68phZ71A1FrEijnP-VNJnrnkPM/s979/Unbound_Dig_test.png)PiHole Web UI Go to the Settings section, then the DNS tab. Untick all of the DNS Servers in the left columns. Tick the Custom 1 Server and enter the following IP: 127.0.0.1#5335 [![](/assets/images/PiHole_DNS.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjtzv6eC5Ndyxhe3t2fOMsF46ad-aO9Dp1pujGTPreYTsxWczv3mMxG644HwsQHCmw40vBEdu2NMERYdH1qt1RmcYA97dPa31TFFpzbUvyeelKmIMdBR244aduyhVuXDHX_5SBEkwnIF0I/s1257/PiHole_DNS.png)Scroll to the bottom and click Save.

**OPTIONAL**: You can enable DNSSEC to validate that DNS queries are legitimate, but it is not required.

## Conclusion

I hope that this tutorial is useful and easy to follow, and of course if you have any feedback or have a problem, please leave a comment or contact me on Facebook.

As I think this is the right group of people to ask, if anyone knows of a way to monetise this site without using intrusive ads, please send me an email at contact@crankshafttech.com with your suggestions.

If you liked this tutorial, you can find all my other tutorials [here](https://blog.crankshafttech.com/search/label/Tutorials), and also through the tags below.

more

Update #1: Added Port Forwarding to PiVPN setup. Thanks u/blast-inc for pointing it out!

Update #2: Added a note about Dynamic DNS for non-static IP Addresses. Thanks u/FlashDerPlatte for making me aware!

Update #3: Added instructions for generating the QR Code for mobile VPN clients, and for investigating what file it is generated from. Thanks u/ThisUserEatingBEANS for reminding me of this amazing feature!
