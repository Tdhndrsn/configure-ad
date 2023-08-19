<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This a video demonstration installing and configuring Active Directory within Microsoft Azure Portal.<br />


<h2>Video Demonstration</h2>

https://youtu.be/GFxeRzyF4VE



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services


<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>Configuration Steps</h2>

  

  <h2>Setup Resources in Azure</h2>
1.	Create the Domain Controller VM (Windows Server 2022) 
2.	Set Domain Controller’s NIC Private IP address to be static 
•	On Azure portal, Click on Domain Controller, Click networking, Click Network card, click IPconfiguration, Set IP to Static, Click Save
3.	Create the Client VM (Windows 10)
4.	Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
•	Type in windows search bar: wf.msc 
•	Click inbound rules
•	Filter protocol to ICMPv4
•	Right click on Core Networking Diagnostics click “enable rule” (there should be 2 rules for Core Networking Diagnostics)




 

  <h2>Install Active Directory</h2>
  1.	Login to Domain Controller and install Active Directory
•	Click Add roles and features to install Active Directory Domain Services
•	Click yellow triangle upper right hand corner, click promote this server to a domain controller
•	Click “Add a new forest”
•	Enter ROOT domain name
•	Enter Password
•	Click continue then Install
•	Domain Controller should restart if not restart in Azure portal and then log back into Domain Controller with my domain credentials

<h2>Create an Admin and Normal User Account in Active Directory</h2>
1.	Create a new Organizational Unit right click on domain and select organizational unit. Create Organizational Unit for EMPLOYEES and ADMINS.
2.	Create a new employee in ADMINS – right click on admins, click user, type admin first and last name, type username and password, click ok.
3.	Add new employee to “Domain Admins” Security Group – right click employee name under ADMINS, click properties, click member of, click add, type domain, click check names, click domain admin, click ok, apply, ok. 




<h2>Join Client (Windows 10 VM) to the Domain Controller (Windows Server 2022)</h2>
1.	Go to Azure portal, set client’s (Windows 10) DNS settings to the DC’s private IP address. First, get DC’s private IP address: Go to DC – copy private IP address – go to client’s virtual machine, select networking, click the network card (NIC), click DNS Servers, select Custom, type DC’s private IP address, click save. Restart client’s virtual machine.
2.	Login to Client’s (Windows 10) VM (Remote Desktop) – use original login credentials and join Client’s VM to the domain (Windows Server 2022)
•	Right click windows icon, click system, click Rename this PC (advanced), click change, click domain and type the domain name with the admin’s name created in Active Directory. Ex: mydomain.com\jane_admin. Click ok. Enter “password” then click ok. (Computer will restart)
3.	Login into the Domain Controller (Windows Server 2022) (Remote Desktop) and verify Client (Windows 10) shows up in Active Directory Users and Computers inside the “Computers” container on the root of the domain.
4.	Right click on root domain, click Organizational Unit, type CLIENTS and drag Client in “Computers” container to Organizational Unit CLIENTS.



<h2>Setup Remote Desktop for non-administrative users on Client (Windows 10 VM)</h2>
1.	Log into Client (Windows 10 VM) via Remote Desktop as the administrator you recreated in Active Directory. Ex: username: mydomain.com\jane_admin and password
2.	Open system properties – right click windows icon, click system, click “REMOTE DESKTOP”, click “select users that can remotely access this PC”, click ADD.
3.	Allow “domain users” access to remote desktop, Click ok
4.	Now you can log into Client (Windows 10 VM) using original credentials, as a non-administrative user.



<br />
