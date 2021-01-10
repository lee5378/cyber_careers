---
ID: 21
post_title: How to Build a Home Cyber Range, Part 1
post_name: how-to-build-a-home-cyber-range-part-1
author: cyberteach
post_date: 2021-01-02 14:20:39
layout: post
link: >
  https://alphacyberlabs.com/how-to-build-a-home-cyber-range-part-1/
published: true
tags: [ ]
categories:
  - Uncategorized
---
<!-- wp:paragraph -->
<p>### Why a Lab Computer?</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Cybersecurity professionals make use of a wide range of tools that run primarily on a Windows or Linux operating system (OS). Security techniques often involve tracking the communication between multiple machines on a network. Rather than building out dozens of special-purpose physical machines on a network, this environment can be simulated with virtual machines (“VMs”). VM platforms are capable of hosting a “computer within a computer,” allowing for a “host” computer to run multiple isolated operating system environments, within the operating system of the host.&nbsp;</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So, a dedicated lab computer at home means you can spin up virtual machines in a contained, secure sandbox without dealing with those pesky cloud charges or having to consume precious resources on your personal desktop system. Most security professionals have home lab environments used for learning and simulating scenarios. In fact, many cybersecurity interviews include the question: “What is your home lab like?” In this series, I’ll show you what a complete answer looks like.&nbsp;</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>### Hardware</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For my home cyber range, I wanted a computer that, while affordable, still offers specifications that facilitate virtualization. Having used server blades in the past, I ruled those out early on due to their noisy fans and steep power consumption. In the IT home lab community, the Dell Precision T3600 is highly regarded as a unique value point due to its support for an 8-core CPU (a CPU with an astonishing $30 price tag on eBay). I sourced mine from a local computer shop, but sadly it only came with a quad-core CPU with the following specs:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":23,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://alphacyberlabs.com/wp-content/uploads/2021/01/pasted-image-0-1.png" alt="" class="wp-image-23"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>CPU: Intel Xeon E5-1603 Quad Core 2.8GHz Processor</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>GPU: 2x AMD/ATI Turks GL FirePro V4900</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>RAM: 32GB</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Hard drive: 128GB SSD, 1TB HDD</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Your first virtualization server computer does not necessarily need to be a T3600 model. This article calls out a few specific models that are good value points for these desirable hardware specifications, but you can use any model computer so long as you keep in mind the desirable specifications. A virtualization computer should have a high CPU core count prioritized over raw CPU speed. At a minimum, you’ll want 4 cores, on a CPU that supports VT-x instructions, like an Intel Core i5-2300 series or higher. RAM (16GB at the minimum) and hard drive space (at least a few hundred GB) are critical to hosting multiple VMs. Using an SSD for a hard drive also provides a very helpful performance gain. I picked up some extra RAM at the shop for my machine, and I’m planning on upgrading the CPU to an 8-core in the near future.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you’re interested in the louder rackmount server blades, check out the Dell Poweredge R720 or R710. These offer a similar price and value point, and generally boast a higher CPU core count than the T3600. Downsides of running a server blade at home include higher power consumption, additional hardware complexity, and loud fan noise. For the sake of having a beginner-friendly lab, I don’t think a person’s first Virtualbox host should be an enterprise server blade, unless the learner has worked in IT in the past.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>### Operating System</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Next, I needed a lightweight, resource-efficient operating system (OS), so I went with Ubuntu. Ubuntu is a free, open-source OS based on the Linux architecture. After downloading Ubuntu 20.04.1 LTS to my Windows 10 PC, I needed a way to boot into external media to install Ubuntu, so I created a bootable USB flash drive using <a href="https://linuxconfig.org/create-a-bootable-ubuntu-20-04-usb-stick-on-ms-windows-10">UNetbootin</a> on my Windows PC. These steps and what follows below are all covered in the Code Fellows <a href="https://www.codefellows.org/courses/ops-102/intro-to-computer-operations/">Ops 102</a> course. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":25,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://alphacyberlabs.com/wp-content/uploads/2021/01/pasted-image-0-1-1.png" alt="" class="wp-image-25"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After plugging in the USB to my T3600 and booting off it, my installation process went smoothly.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":26,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://alphacyberlabs.com/wp-content/uploads/2021/01/pasted-image-0-2.png" alt="" class="wp-image-26"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>### Virtualization Software</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For my virtualization software, I elected to use Virtualbox. This software allows me to spin up VMs of any operating system I have the installer image for. I’ve tinkered with VMWare in the past, but the free version does not allow the user to export a .ova file, whereas Virtualbox does. Otherwise, the choice between the two is largely a matter of preference and brand allegiance. There are other more advanced virtualization solutions such as ESXI but for my purposes, I’m wanting to setup a beginner-friendly configuration that others can easily reproduce.&nbsp;</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In Linux, installing software is generally a breeze thanks to the Apt package manager tool. However, in Virtualbox’s case, we’ll need to execute some additional scripting in order to download latest version. The Apt repository’s entry for Virtualbox is outdated, so the usual sudo apt install appname command in this case won’t actually fetch the latest version of Virtualbox. I tossed the below code into a Bash script to load Virtualbox and its extension pack.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add –</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add –</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>echo “deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian eoan contrib” | sudo tee /etc/apt/sources.list.d/virtualbox.list</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>sudo apt update</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>sudo apt install linux-headers-$(uname -r) dkms</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>sudo apt-get install virtualbox-6.1 -y</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>wget https://download.virtualbox.org/virtualbox/6.1.6/Oracle_VM_VirtualBox_Extension_Pack-6.1.6.vbox-extpack</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>vboxmanage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.1.6.vbox-extpack</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The above script has a small bug in that you’ll need to manually update the extension pack in Virtualbox Manager GUI after it’s loaded, but otherwise it automates the installation of the latest Virtualbox (as of writing). In Ubuntu, pasting this into a script.sh file and executing sudo bash script.sh will execute the script.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once VirtualBox was installed, I next deployed a Windows 10 VM and a Kali Linux VM. The Windows ISO is easily generated using Microsoft’s&nbsp;<a href="https://www.microsoft.com/en-us/software-download/windows10">Media Creation Tool</a>, and the Kali Linux ISO is downloadable at&nbsp;<a href="https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/">Offensive Security’s site</a>. Once you have an ISO image, installing a new VM consists of selecting “New” on Virtualbox Manager GUI, selecting the OS, then pointing Virtualbox to the corresponding ISO file. The remaining installation procedure will be unique to each OS.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I’m ultimately wanting to simulate an enterprise network. I can start with a Windows 10 PC as a basic target system. Windows 10 is now the most common OS in the business endpoint world, and Microsoft Active Directory is the most common domain deployment in enterprises. I’ll need to add more systems to this virtual sandbox, such as Windows Server 2019, but loading Windows 10 is a good start as a target, and also just to make sure Virtualbox is working as expected.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Kali Linux is a free open source Linux distribution that includes attack tools that pentesters can use to perform attacks on computer systems. I’m loading a Kali VM early on to be able to perform basic attacks. I can also use Kali Linux against cloud targets, such as Hack The Box.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Here’s what VirtualBox looks like, once I’ve completed the setup for  a Windows 10 VM and a Kali Linux VM:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":28,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://alphacyberlabs.com/wp-content/uploads/2021/01/pasted-image-0-3-1024x644.png" alt="" class="wp-image-28"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>### To Be Continued</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is the humble beginning of our cyber range, but we’re not quite there yet! In Part 2 of this series we’ll explore how to remote into your lab computer using XRDP and create a firewalled network subnet using pfSense within Virtualbox.By&nbsp;<a href="https://tips.azipress.com/?author=1">lee5378</a></p>
<!-- /wp:paragraph -->