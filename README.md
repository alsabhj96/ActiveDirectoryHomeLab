# ActiveDirectoryHomeLab

Active Directory Home Lab 
In this lab I utilized Oracle Virtual Box to create an Active Directory home lab. It demonstrates how to configure a Domain controller (DC) and a client workstation in a secure, isolated environment.   
Technology that I have used in this lab:
•	VirtualBox 
•	Active Directory Domain Services
•	Powershell
Operating System that I have used in this lab:
•	Windows server 2019
•	Windows 10 

Step 1 – Download Oracle Virtual Box 
Link: https://www.virtualbox.org/wiki/Downloads
Download the VirtualBox 7.2.2 from the VirtualBox platform package by selecting the correct OS. Also, make sure to download the VirtualBox extension pack after you install VirtualBox.
 

Step 2 – Download Windows 10 OS and Server 2019 ISO 
Link for Windows 10 IOS: https://www.microsoft.com/en-us/software-download/windows10
Link for Server 2019 ISO: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
Follow the both link to download windows 10 iso and server 2019 iso, make sure you select iso while downloading also 64 bit if it ask. Download both file in same location so it will be easy to find later on (I choose in desktop).  
Step 3 – Create a new virtual machine on VirtualBox
Now we gonna open up virtual Box and click new to make new Virtual Machine (VM). 
 

After clicking the new, we will name it as a DC (for Domain Controller) and choose “other windows (64-bit) for version”. 
 

Next, we will configure the hardware settings for the VM by assigning the 2-4 GB RAM and 50-60 GB disk (Dynamically allocated). I have choose 4096 MB of memory and 2 CPUs. Click “next” when you get to Virtual Hard Disk and then Finish.
 

Step 4 – Configure DC Machine Basics
Go on DC machine settings, on general page, click “Advanced” tab and under Shared Clipboard choose “Bidirectional” instead of Disabled option also for Drag’n’Drop choose “Bidirectional” option. We select “Bidirectional” on shared clipboard because it will allow us to copy and paste in between our actual computer and the virtual machine. We select “Bidirectional” on Drag’n’Drop which means we can drag file from desktop into virtual machine. 
 

Next, we will go on “Network” Setting, where we will be creating domain controller so we need to have 2 NICs running: Adaptor 1 for internet “NAT” and Adaptor 2 for internal VMware network. So, the first Adaptor comes itself as an attached “NAT” that connect to our house internet. Then, we will add another adaptor as “Adaptor 2” where we select “internal network” from drop down menu, and then press OK.
 
Step 4 – Adding Server 2019 ISO
After that we will open the -DC VM then you will encounter an error because server 2019 iso is not attached.
 
 So, we will be adding Server 2019 iso that we have downloaded before. For that go on VM setting under Storage then in “Controller: IDE” you will see like a CD. Then Select your downloaded “Server 2019 iso”. 
 
Step 5 – Installing and Starting VM Server 2019
So, we are staring VM and installing server 2019. After installing we create password for Administration, then press Finish. After that server installation is finish. Then press Ctrl + Alt + Delete to unlock. (Easy way to unlock is by going on VM “Input -> keyboard-> Click (Ctrl + Alt + Delete)” which you will find on top of the setting). 
 

Step 6 – Setting up IP Address on DC
Next, we are setting up our IP addressing. We have two NICs: one that is Internet and another one that is Internal for our internal network. So, Internet NICs will automatically get an IP address from our home router and for the Internal NICs we will be setting up manually. For that Click “Network” icon at the right of the VM Screen in bottom and Select “Network and Internet Settings”. Under the setting Select “Change adapter options” and you will see two adapters. We will look out to them and change name accordingly which adapter is directing to the internal network and which adapter is directing to the internet. Right click on adapters and then Select “Status” and “Details”. We are looking for IPV4 address so for the first adaptor it has 10.0.2.15 which is like a proper home address that is connected to the internet (which is my home DNS Server). If we see other adaptors then you will find IPV4 it has 169.254.218.74 which is automatically assign from VM. So, after finding right-click on each adapter and rename them accordingly. So, Renaming home DNS Server to “INTERNET” and other one “INTERNAL”. 
 
Now Internal adaptor was unable to find DHCP server so, we need to assign an IP address for it. Right-click the “INTERNAL” adapters, then Select properties and choose “Internet Protocol Version 4 (TCP/IPV4) and again select properties. After that, select “Use the following IP Address” and assign IP address, Subnet mask and preferred DNS Server to allow the computer to ping, or loopback to itself accordingly as shown in the picture below. We are not going to use default gateway because Domain Controller itself serve as the default gateway. 
When we install AD it automatically install DNS so this server use itself as DNS Server, for that we can enter it own IP Address or loopback address. 
 

Step 7 – Install and Configure Active Directory Domain Services
Now we will install Active Directory Domain Services and create Domain. For that go to the Server Manager in the DC VM and click "Add Roles and Features".  Click next twice,  and select a server that we are using. Then when you come to Select Server role click 'Active Directory Domain Services' where “Add Roles and Features Wizard” popups then press “Add Features” and continue do next until it came finish and click “Install”.  
 
After the installation we will find, caution sign at the top of the screen. so, we will do our post deployment configuration for that (We install software for AD services but we haven’t created a domain yet). So, click 'Promote this sever to a domain controller'. Next, we will select 'Add a new forest' and enter a generic name into the domain name box where I have given (mydomain.com). Then click next. You will be prompted to enter a password, create a password. Then click next until you're done and the system will restart on its own.

 
 

Now after restart, login to the newly created domain account (we will see the MYDOMAIN\Administrator). Now we are going to create our own domain admin account instead of using built-in administrator account. For that go to Start and Windows Administrative Tools, then Active Directory Users and Computers. You'll see our newly created mydomain.com. Right-click on the mydomain.com, go to New and scroll down to Organizational Unit. Go ahead and name this folder ADMIN and uncheck the box asking to protect the container from accidental deletion.

Step 8 – Set Up RSA and NAT
Next, we are installing our Remote Access Server and Network Address Translation to allow us to give our client to be on private virtual network, but access the internet through the domain controller. Go to the Server Manager and click Add Roles and Features. Click next three times and once you get to the 'Select Server Roles' page, you want to select Remote Access. Click next until you get to the 'Select Role Services' page and select 'Routing'. DirectAccess and VPN (RAS) will auto populate, go ahead and leave it and then click next until you get to install and then install the new role and feature.

Step 9 – Configure DHCP Server and DNS
We will now set up our DHCP server to allow our client machine to browse the internet with a set range of IP addresses and subnet mask. This simulates how things work in your company and/or school. What you'll need to do is go to the Server Manager, click Add Roles and Features, next, next, next, and then on the 'Select Server Roles' page you'll select the DHCP Server box and add the required features. Then click next to the end and select install.
Resource Used 
https://www.youtube.com/watch?v=MHsI8hJmggI&list=PLqBeiU46hx1H--SNfTrohTOWeqkK-M2Y0

	

