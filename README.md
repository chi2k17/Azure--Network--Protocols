On-premises Active Directory Deployed in the Cloud (Azure)

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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setup Resources in Azure<br />
Create the Domain Controller VM (Windows Server 2022) named “DC-1”<br />
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time<br />
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
Set Domain Controller’s NIC Private IP address to be static<br />
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a<br />
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher<br />

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensure Connectivity between the client and Domain Controller <br />
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) <br />
  
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall <br />
Check back at Client-1 to see the ping succeed <br />

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Install Active Directory<br />
Login to DC-1 and install Active Directory Domain Services<br />
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)<br />
Restart and then log back into DC-1 as user: mydomain.com\labuser<br />
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create an Admin and Normal User Account in AD<br />
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”<br />
Create a new OU named “_ADMINS”<br />
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”<br />
Add jane_admin to the “Domain Admins” Security Group<br /><br />
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on<br />

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain (mydomain.com)<br /><br />
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address<br />
From the Azure Portal, restart Client-1<br />
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)<br />
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the<br /> root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there<br />
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setup Remote Desktop for non-administrative users on Client-1 <br />
Log into Client-1 as mydomain.com\jane_admin and open system properties <br />
Click “Remote Desktop” <br />
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Allow “domain users” access to remote desktop <br />
You can now log into Client-1 as a normal, non-administrative user now <br />
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab) <br />
</p>
<br />



<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a bunch of additional users and attempt to log into client-1 with one of the users <br />
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator <br />
  
  <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Run the script and observe the accounts being created <br />
When finished, open ADUC and observe the accounts in the appropriate OU <br />
attempt to log into Client-1 with one of the accounts (take note of the password in the script) <br />

</p>
<br />












