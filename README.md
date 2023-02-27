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

- Step 1: Setup Resources in Azure
- Step 2: Ensure Connectivity between the client and Domain Controller
- Step 3: Install Active Directory
- Step 4: Create an Admin and Normal User Account in AD
- Step 5: Join Client-1 to domain
- Step 6: Setup Remote Desktop for non-administrative users on Client-1


<h2>Deployment and Configuration Steps</h2>

<p>
<img src= "https://i.imgur.com/pyyXFoH.png" height="80%" width="80%" 
</p>
<p>
Step 1: Setup Resources in Azure
  
  Create the Domain Controller VM (Windows Server 2022) named “DC-1”.

  -Take note of the Resource Group and Virtual Network (Vnet) that got created at this time

  Set Domain Controller’s NIC Private IP address to be static

  Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created above.

  Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

</p>
<br />

<p>
<img src= "https://i.imgur.com/rqpjy37.png"  height="80%" width="80%" 
</p>
<p>
Step 2: Ensure Connectivity between the client and Domain Controller
  
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). It'll show as timed out since DC-1's windows firewall is blocking ICMP traffic.
  
<p>
  <img src= "https://i.imgur.com/zNBC9jY.png" height="80%" width="80%" 
<p>

 Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

 <p>
  <img src= "https://i.imgur.com/Mjw7yKz.png" height="80%" width="80%" 
<p>

   Check back at Client-1 to see the ping succeed

</p>
<br />

  Step 3: Install Active Directory Services
<p>
<img src= "https://i.imgur.com/jmxHtF2.png" height="80%" width="80%" 
</p>
<p>
Login to DC-1 and install Active Directory Domain Services

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

Restart and then log back into DC-1 as user: mydomain.com\labuser

  Step 4: Create an Admin and Normal User Account in AD
  </p>
<br />
<img src="https://i.imgur.com/79l9ce1.png" height="80%" width="80%" 
</p>
<p>
  
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” & “_ADMINS”

 </p>
<br />
<img src="https://i.imgur.com/CCgLKCO.png" height="80%" width="80%" 
</p>
  
  Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

 Add jane_admin to the “Domain Admins” Security Group

Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”

 User jane_admin as your admin account from now on

  Step 5: Join Client-1 to domain
 <p> 
<img src="https://i.imgur.com/ffArESp.png)" height="80%" width="80%" 
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

  From the Azure Portal, restart Client-1
  
<p> 
<img src="https://i.imgur.com/1cIkxdC.png" height="80%" width="80%" 
</p>

Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
  
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

 Create a new OU named “_CLIENTS” and drag Client-1 into there

Step 6: Setup Remote Desktop for non-administrative users on Client-1
  
  </p>
  <img src="https://i.imgur.com/rM5W0lf.png" width="80%" 
</p>
<p>
  
Log into Client-1 as mydomain.com\jane_admin and open system properties

  Allow “domain users” access to remote desktop
  
</p>
  <img src="https://i.imgur.com/I2wwbgf.png" width="80%" 
</p>
  
  You can now log into Client-1 as a normal, non-administrative user now 

 
