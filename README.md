# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up a Domain Controler and Cliente Machine
- Install Active Directory on DC-1
- Create a Domain Controller
- Create Organizational Units and Admin
- Set DC-1 as Client-1's DNS server
- Create users to be able to log into Client-1

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/kDPPyp7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The first thing we are going to do is create a Virtual Machine. When we create the Virtual Machine, it will give us the ability to make a Resource group from the Virtual Machine screen. We are going to choose Windows Server 2022 x64 and make sure to pick a size with at least 2 cpu since 1 will be slow. Once youve made a name and password, check the bottom boxes off and hit the "Review and Create" button. As thats creating, we will create client 1.
</p>
<br />

<p>
<img src="https://i.imgur.com/5yJ1pyG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We are going to go to Virtual Machines and we will create a new one. Make sure to link it with the same Resource Group we made prior.Click the "Next" button again to Disk and click it again to get to the networking section. Make sure its in the same Virtual Network as the Domain Controler. Click on create.
</p>
<br />

<p>
<img src="https://i.imgur.com/xnpeqT4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we will set the Domain Controller's NIC private IP status from dynamic to static. Go back to Virtual Machines and click on the DC-1. Under the Setttings on the left of Azure, click on the Netwroking.  You should see the Network Interface and click the actual NIC it was given. Then under the Settings on the left, click on IP Configurations. You should see the Private IP address and it should say Dynamic. Click on the Dynamic and then click on static and save. That way the Domain Controller IP address won't change. 
</p>
<br />

<p>
<img src="https://i.imgur.com/seEj3Lj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to Virtual Machines and make sure they are both on the same Virtual network/Subnet. Once we've checked that they are on the same Subnet, we will log into the Cliente and see if we can ping the Domain Controller. It should fail with the ping. Copy the Public IP Address from Client and open Remote Desktop. While Client is loading, we will open DC-1 in Remote Desktop as well. Copy the DC Public IP address and put it in the Remote Desktop application and sign in.
</p>
<br />

<p>
<img src="https://i.imgur.com/1Fy2UQV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to Client-1 and say no on all the settings. Open the Command Prompt by going to the search bar near the Start button on the bottom left corner. Search CMD and open Command Prompt. Go back to Azure and get DC-1's Private IP adress. It is under the Networking section. Copy that and go back to Command Prompt on CLient. Type "ping -t (Private IP address of DC-1) without the brackets. The ping should come up with "Request timed out". We to have to enable ICMPv4 on the local Firewall of DC since Ping uses ICMPv4.
</p>
<br />

<p>
<img src="https://i.imgur.com/JIhmml5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to DC-1's Virtual Machine. Go to the search bar and type in "wf.msc" and open "Windows Defender Firewall with Advanced Security". Click on "Inbound Rules" on the left column. We are going to sort by Protocol. Click on the Protocol column. Search for both of the "Core Networking Diagnostics-IMCP Echo Request". Right click each one and enable. Now go Back to CLient-1 and ping the DC-1 again. You should now see a reply from DC-1. 
</p>
<br />

<p>
<img src="https://i.imgur.com/cYZv4uU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will now Install Active Directory on DC-1 and activate it as a Domain Controler. Then we will restart and log back in. Log back in to DC-1 and go to Server Manager and select "Add Roles and Features". Click on Next and under "Server Selection" make sure it is under the DC-1 which it should be. Hit next to bring up Server Roles and select "Active Directory Domain Services" and then select Add Features. Then just keep hitting Next and then finally Install and when it's done, just hit the Close button.
</p>
<br />

<p>
<img src="https://i.imgur.com/LBvgqiE.png" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 is still not a domain server yet. We have to set up a actual domain and name. You should see a Caution signal near a flag. Click on that and click on "Promote this server to a domain controller". You are going to select the bubble "Add a new forest". You can now create whatever name you would like. For this example, we will be using "mydomain.com". Hit the next and you will be prompted to create a password. Use whatever you would like and just hit next. Your domain name should pop up and just hit next until you get to the "Install" button.
</p>
<br />

<p>
<img src="https://i.imgur.com/fQeiIKX.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once it is done installing, it should automatically log you out. We are going to need to log back in to DC-1. Copy the public IP address and paste it into Remote Desktop application. When it has you log in, select use different user. Sine it is a Domain Controler, we need to log in with the domain name \ the user. For example our was mydomain.com\labuser. Use the password for the Domain Controller and then it will ask you for the password to "labuser".
</p>
<br />

<p>
<img src="https://i.imgur.com/Oh2tnHU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once it is finished loading, we will create a couple of organizational units and admin user. Under the "Server Manager", select the Tools tab near the top right and select "Active Directory Users and Computers". You should see that it created our domain "mydomain.com" and some default things. There shouldn't be any Computers in the domain and if you click on "Domain Controllers", we should see DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/CxpDqK5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Right click mydomain.com and select "New" and then select "Organizational Unit". Create one named "_EPLOYEES". and create another one named "_ADMINS". You should now see those organizational units under mydomain.com. 
</p>
<br />

<p>
<img src="https://i.imgur.com/pnxl6yS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We are now going to create our own admin account. Right clik the "_ADMIN" and select "New" and "User". Name the admin whatever you would like. We are going to use Jane Doe for example. You will now create user log on. For this example, we will use jane_admin and select the "Next" button. Create a password for the user and uncheck the "User must change password at next logon". Normally you would keep that on in real life since the user would make their own password. For this example, we will create it ourselves. 
</p>
<br />

<p>
<img src="https://i.imgur.com/HD5zujq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We should now see "Jane Doe" in the _ADMIN folder. We now have to give her admin authority. Right click Jane Doe and select "properties". Go to the "Member Of" tab and select the "Add" button. Under the type box type"domain" and click the check names button. There should be a list of Domain groups. Select "Domain Admin" group and select "Ok". Then select "Apply" and then "Ok". We are now going to log off as labuser and log in as jane_doe.
</p>
<br />

<p>
<img src="https://i.imgur.com/qEkTxer.png" alt="Disk Sanitization Steps"/>
</p>
<p>
Copy the public IP address for DC-1. Open Remote Desktop and paste address. Select "Use different user" and type in "mydomain.com\jane_doe". Type in the password for Jane Doe's account. You can double check who is loged in by opening the console commands and typing "whoami" and it should pop up with the admin.
</p>
<br />

<p>
<img src="https://i.imgur.com/t0JTmyC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will now be connecting Client-1 to DC-1. We want to be able to log into Client-1 as Jane Doe. We are going to do that by setting CLient-1's DNS settings to DC's private IP address. We need to change Client-1's Virtual DNS (which it gets when created) to DC-1 as the DNS server. Go to Azure and select DC-1 under Virtual Machines, then select "Networkin" on the left side. You should see the NIC Private IP address. Copy that address. Go to CLient-1 and select "Networking" on the right. Click on the "Network Interface" near the top. Select "DNS servers" on the left side of Azure. Click the "Custom" bubble and paste DC-1's private IP address that we copyed earlier. Make sure there are no spaces and click "Save".
</p>
<br />

<p>
<img src="https://i.imgur.com/1SIvKZf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once that is saved, go back to Virtual Machines in Azure and click "Restart" at the top to restart Client-1's Virtual Machine. Copy Client-1's public IP address and open Remote Desktop and paste it in. We are going to log in as labuser since it is fully connected to DC-1. Go to command prompt and type in "ipconfig /all" and witness that the DNS server IP is the same as our DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/KCq3PRo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will right click the start button and select "Settings". Click on the "Rename this PC (advanced)". Click on the "Change" button. Under the "Member of", select the "Domain" bubble and type in the Domain name. Ours for example is mydomain.com. This will point the domain name to the controller and the controller will be able to find it. Select "ok" buttton. It should pop up with a login window. Type mydomain.com\jane_doe and whatever you have as the password.  There should be a message that pops up saying "Welcome to the mydomain.com domain". Click ok and the computer will restart.
</p>
<br />

<p>
<img src="https://i.imgur.com/qEkTxer.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will now log in to CLient-1 as Jane Doe. Copy the public IP address and open Remote Desktop. Paste the IP address into it and connect. When it comes up with the login info, select "change" and select "Use different account". Enter in "mydomain.com\jane_doe" and enter the password you made for it. It will now log you into Client-1 as Jane Doe. We will now work in the settings so any user can log into CLient-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/0SbhaKt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go into Client-1 Virtual Machine. Right click on the start button and select "System". Click on "Remote Desktop" and under the User Accounts, select the "Select users that can remotley access this PC". It will have a type box. Type in "domain users" and check names and then click "ok". Go to DC-1 and open Active Directory Users and Computers.
</p>
<br />

<p>
<img src="https://i.imgur.com/8JTTxgD.png" alt="Disk Sanitization Steps"/>
</p>
<p>
We are now going to create new users and see if they can log into Client-1. Login as Jane Doe on DC-1. We are going to open Powershell ise. Go to the bottom near the start mutton and search for it and right click, and open it as a administrater. This is a scripting language for Windows. Create a new file by selecting the paper with the star on the right on the top left side. Paste the contents of the script into it and run the script with the green play button at the top. This should now be making thousands of randomly generated names. 
</p>
<br />

<p>
<img src="https://i.imgur.com/XX32L3D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the names are done. We will try and log in to CLient-1 with one of the names. In the Active Directory Users and Computers, select the "_Employees" folder and pick a name. Right click on the name and click on "properties". Get the user name that is provided and copy. Log out of Client-1 and log back in with generated name. The password for all the names are Password1. You can now log into Client-1 with any of the names.
</p>
<br />
