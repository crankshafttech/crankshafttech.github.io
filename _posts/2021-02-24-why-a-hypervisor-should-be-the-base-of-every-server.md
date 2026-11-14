---
layout: post
title: "Why A Hypervisor Should Be The Base Of Every Server"
date: 2021-02-24 11:14:00 +0000
tags:
  - "Opinion"
  - "Servers and Networking"
redirect_from:
  - "/2021/02/why-hypervisor-should-be-base-of-every-server.html"
---

Hypervisors allow for many virtual systems to be run on one physical device or cluster of devices, with benefits ranging from reduced energy use compared to running multiple physical servers to being able to easily and effectively deploy, backup and restore different environments.

This post strays from what I usually post about to discuss some of the benefits that every IT person who has their own system running 24/7 should make use of, and why I plan to virtualise my servers.

[![](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F3.bp.blogspot.com%2F-7CAXKJdhO8A%2FW4Qn8wAxInI%2FAAAAAAAABOY%2F471lE8MeWR4OC9dTqz6bsNk0jdXzQhbogCLcBGAs%2Fs1600%2Flogo_vmware_esxil.png&f=1&nofb=1)](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F3.bp.blogspot.com%2F-7CAXKJdhO8A%2FW4Qn8wAxInI%2FAAAAAAAABOY%2F471lE8MeWR4OC9dTqz6bsNk0jdXzQhbogCLcBGAs%2Fs1600%2Flogo_vmware_esxil.png&f=1&nofb=1)

Throughout this post I will be using examples of features from VMWare Hypervisor, also known as ESXi. This is due to a number of reasons, mainly:

1. ESXi has the largest marketshare by [a huge amount](https://www.controlup.com/hypervisor-market-share-controlup-perspective/)
1. Licenses can be obtained legally for free from [VMWare](https://my.vmware.com/group/vmware/evalcenter?p=free-esxi7)
1. It's very easy to use and learn

With that out of the way, let's get into it!

# What is a Hypervisor?

A hypervisor is a software and/or firmware layer that allows virtual machines to be run on the hardware. A virtual machine is basically a box inside the physical computer that only has access to some processing power and storage of the physical computer's resources. This allows multiple virtual servers to be run at once on one machine.

A hypervisor is whats called [Type One Virtualisation](https://medium.com/teamresellerclub/type-1-and-type-2-hypervisors-what-makes-them-different-6a1755d6ae2c), which is different from a Type Two like [VirtualBox](https://www.virtualbox.org/) as it does not run on top of an OS such as Microsoft Windows. Instead it runs directly on the hardware, allowing more resources to be dedicated to the Virtual Machines and less to the Host overhead.

VMWare ESXi is the most popular Type One Hypervisor, but some others are also used such as [Citrix Hypervisor](https://www.citrix.com/products/citrix-hypervisor/) and [Microsoft Hyper-V Server](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/hyper-v-server-2016).

Hypervisors have many benefits over running the software on bare metal, i.e. installing it directly onto the Hard Drive and not in a virtual environment.

# Benefits Of A Hypervisor

## 1. Easy Deployment and Management

The biggest advantage of using a hypervisor is the ability to easily deploy, manage and destroy software environments, all without touching the hardware of the server.

To begin with, ISO files are loaded into a [datastore](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.storage.doc/GUID-3CC7078E-9C30-402C-B2E1-2542BEE67E8F.html), which is basically a folder that can be used with ESXi to store disk images and Virtual Machine files.

Once that is done, a virtual machine can be created using that ISO image. If something goes wrong and it either crashes or doesn't boot, all the settings you could need are at the tips of your fingers, including changing allocated system resources all the way to changing the virtual boot order.

Once it is booted and working, you have the ability to reboot, shut down or forcefully power off the virtual system, all without turning the host off, retaining the ability to control the system.

Also, when you want to change what you are running on your system or you want to run multiple VMs at the same time, just power on and off the VMs accordingly using the GUI without risking loosing connection from accidentally shutting down.

## 2. Full Virtual Disk Snapshots

One of the other big advantages of using a hypervisor is that they have the ability to take 'snapshots' or 'savestates' of VMs, so that if you install an update or run a malicious script, you can revert back to a full working copy. This has saved me multiple times on other machines like laptops when I installed an update and the system failed to boot, with the snapshot enabling me to go back before the broken update and take a different path.

This can be utilised when attempting to transition between OS versions in a VM, such as moving from Windows Server 2012 - 2016. Having the snapshots means that any changes that don't work can easily be reverted, and it also means you could work on a testing and a production version of a VM on the same hardware, of course utilising different [virtual networks](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-92DF3F3C-D17E-42BD-8620-9B0D780FE7F6.html).

Please note that this approach is not recommended in high stakes environments, but when there is only enough cash to buy one server, this is one way of effectively testing changes.

## 3. Scheduling of Management Tasks

Another useful feature is the ability to schedule tasks to be run at different times on the hypervisor. Paired with #2, this can allow for [automated snapshots of VMs](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenterhost.doc/GUID-03B90638-6C70-4379-8CAF-D66866D115F4.html) so that you are always prepared for the worst. As well as scheduling snapshots, other tasks can also be automated, such as [turning VMs on and off or changing available resources.](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenterhost.doc/GUID-F9DC9B18-625E-4ACB-AA3A-17BEB109A206.html) These tasks can be scheduled to run at various different times, such as after booting, hourly, or anywhere up to monthly. 4. Power SavingsThis point mainly applies to people that have multiple servers running that don't each require much processing power. Bundling them into one physical device makes management easier through one UI, as well as reducing power usage by more effectively utilising every watt instead of generating more waste heat and noise per application. It also has the added effect of requiring less hardware, therefore being cheaper to purchase and maintain, as well as being better for the environment by using less non-recyclable resources that are sent to landfill at the end of their life. 5. Migration Between Physical SystemsThe last major advantage of using a hypervisor is the ability to easily transfer a working system between different physical machines without having to make sure the hardware is compatible. As long as the hypervisor runs fine on the new hardware, the VM will too. This is achieved through the fact that all of the VM data is saved in files in the datastore that can be easily exported and imported into the new system's hypervisor. Once it is transferred, it will just boot up like normal, not even noticing the change in hardware. Of course there are exceptions, but they are not very common and this is still far more seamless then transferring bare metal systems to new hardware.
SummaryBased on these findings, I plan to make a follow up to this post to move my [OpenMediaVault NAS](https://www.openmediavault.org/), [Docker](https://www.docker.com/) containers and [PiHole](https://blog.crankshafttech.com/2019/12/set-up-pihole-with-doh-and-pivpn.html) to a virtualised environment, utilising many of the features listed above. 
If there are any other benefits to a hypervisor I didn't list or major disadvantages, let me know in the comments below. If you liked this new style of content, let me know by leaving a comment, reaction or sharing it with your friends. If you liked this topic, you can find all my Server and Networking content [here](https://blog.crankshafttech.com/search/label/Servers%20and%20Networking). 
EDIT #1: Added the ESXi logo
