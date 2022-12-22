# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The first thing we are going to do is create a Virtual Machine. When we create the Virtual Machine, it will give us the ability to make a Resource group from the Virtual Machine screen. We are going to choose Windows Server 2022 x64 and make sure to pick a size with at least 2 cpu since 1 will be slow. Once youve made a name and password, check the bottom boxes off and hit the "Review and Create" button. As thats creating, we will create client 1.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We are going to go to Virtual Machines and we will create a new one. Make sure to link it with the same Resource Group we made prior.Click the "Next" button again to Disk and click it again to get to the networking section. Make sure its in the same Virtual Network as the Domain Controler. Click on create.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we will set the Domain Controller's NIC private IP status from dynamic to static. Go back to Virtual Machines and click on the DC-1. Under the Setttings on the left of Azure, click on the Netwroking.  You should see the Network Interface and click the actual NIC it was given. Then under the Settings on the left, click on IP Configurations. You should see the Private IP address and it should say Dynamic. Click on the Dynamic and then click on static and save. That way the Domain Controller IP address won't change. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to Virtual Machines and make sure they are both on the same Virtual network/Subnet. Once we've checked that they are on the same Subnet, we will log into the Cliente and see if we can ping the Domain Controller. It should fail with the ping. Copy the Public IP Address from Client and open Remote Desktop. While Client is loading, we will open DC-1 in Remote Desktop as well. Copy the DC Public IP address and put it in the Remote Desktop application and sign in.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to Client-1 and say no on all the settings. Open the Command Prompt by going to the search bar near the Start button on the bottom left corner. Search CMD and open Command Prompt. Go back to Azure and get DC-1's Private IP adress. It is under the Networking section. Copy that and go back to Command Prompt on CLient. Type "ping -t (Private IP address of DC-1) without the brackets. The ping should come up with "Request timed out". We to have to enable ICMPv4 on the local Firewall of DC since Ping uses ICMPv4.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to DC-1's Virtual Machine. Go to the search bar and type in "wf.msc" and open "Windows Defender Firewall with Advanced Security". Click on "Inbound Rules" on the left column. We are going to sort by Protocol. Click on the Protocol column. Search for both of the "Core Networking Diagnostics-IMCP Echo Request". Right click each one and enable. Now go Back to CLient-1 and ping the DC-1 again. You should now see a reply from DC-1. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will now Install Active Directory on DC-1 and activate it as a Domain Controler. Then we will restart and log back in. Log back in to DC-1 and go to Server Manager and select "Add Roles and Features". Click on Next and under "Server Selection" make sure it is under the DC-1 which it should be. Hit next to bring up Server Roles and select "Active Directory Domain Services" and then select Add Features. Then just keep hitting Next and then finally Install.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
