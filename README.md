# Active-Directory-Simulation
# Lab Overview:
Install Windows Server 2019 inside VirtualBox, Create a Domain Controller with RAS,DHCP and NAT, automate users using PowerShell Script, and Install Windows 10 as a client on virtual network

# Software & Devices Used:
- VirtualBox Version 7.2.6  
- VirtualBox Guest Additions Version 7.4.1 (iso)  
- Windows Server 2019 (64-bit iso)  
- Windows 10 eval (64-bit iso)  
- Microsoft PowerShell  

# Procedure
# Part 1 (Initial Installations)
- Install VirtualBox  
- Install Server 2019 on VirtualBox  
  - Storage - 20GB  
  - Memory  -  4GB  
  - Cores   -  2 cores  
  - Network Adapters (2)  
- Install "Guess Editions"  
#
# Part 2 (IP Addressing)
- Open Server 2019 Virtual machine
- Select "Network and Internet Settings"
- Select "Change Adapter Options"
- Identify and rename the Gateway adapter (Should be an actual IP address i.e. 10.0.0.4)
  - Adapter name - _Internet_
- Identify and rename the Internal adapter (Should be a APIPA address)
  - Adapter name - _Virtual_
  - Assign IP address - 172.16.0.1
  - Assign Subnet mask - 255.255.255.0
  - Assign DNS server - 127.0.0.1 (It will be the DNS server so used the loopback address)
- Change the PC name to "_Domain_" (requires restart)
#
# Part 3 (Adding ADDS role, Creating the Domain, and Adding Dedicated Admin Account)
- In Server Manager, add "Active Directory Domain Services" role
- Promote the server to a domain controller
  - On the Server Manager dashboard, This can be done by clicking the yellow task icon to complete the post-deployment configuration.
  - Add new forest
  - Set Root Domain Name: _mydomain.com_
  - Set Directory Services Restore Password: _Password1_
  - Install and Restart
- Create dedicated Admin Account
  - Open "Active Directory Users and Computers"
  - Add a Organization Unit to the mydomain.com folder.
  - Name the OU: Admins
  - Create a new user inside Admin Group folder
    - Name - _Rudy Turnquest_
    - user login - _A_rturnquest_
    - password - _Password1_
- Add the Account to "Domain Admins" to make Domain Account
  - Go to "Properties" and select "Member Of"
  - Add and create "_Domain Admins_" group
  - Select and add user to group
  - Apply, exit and logout
- Login as Admin User
  - At the Login Screen, select other user
  - login as Domain admin Account
#
# Part 4 (Install NAT and RAS)
- In Server Manager, add "Remote Access" role
  - Install "Routing" option
- In Server Manager, Open the Tools menu and select "Routing and Remote Access"
- In Routing and Access Window, right-click the domain controller and select "Configure and enable Routing and Remote Access"
- Select "NAT"
- Point to public interface and select the gateway adapter (Name should be "_Internet_")
- Finish
#
# Part 5 (Install DHCP)
- In Server Manager, add "DHCP" role
- In Server Manager, Open the Tools menu and select "DHCP"
- In DHCP window, right-click the IPV4 icon under the domain menu and select "New Scope..."
- Define Scoop
  - Name - 172.16.0.100-200
  - Start range - 172.16.0.100
  - Stop range  - 172.16.0.200
  - Subnet mask - 255.255.255.0
  - (No lease Duration or IP exclusions)
- Select 'Yes' when ask to configure DHCP options
  - Router (Default Gateway) - 172.16.0.1
  - Select 'Add' to add IP address
  - Select 'Yes' when ask to activate the scoop
- Finish
- Right-click the domain icon and select "Authorize"
- Right-click the domain icon again and select "Refresh"
- _A Green check mark should appear to indicate that DHCP is up_
#
# Part 6 (Install Windows 10)
- Storage - 20GB
- Memory  -  4GB
- Cores   -  2 cores
- **In Virtual Box menu, Change the network adapter from "NAT" to "Internal Network"**
- Install "Windows 10 Pro" from options menu
#
# Part 7 (Rename and Add Client to Domain)
- In Windows 10, right-click start menu and select "system"
- Select "Rename this PC (advanced)" (Scroll down)
- Select "Change..."
  - Name - Client1
  - Domain - mydomain.com
  - Select "OK"
- Use the admin account when asked about permissions for the domain
- Select "OK" and restart
#
# Part 8 (Confirm Internet connection)
- In Windows 10, open the command line  
- use command _ipconfig_ to confirm default gateway is correct  
- use command _ping www.google.com_ to confirm internet connection
#
# Part 9 (Generate users using Powershell)
- From Domain Controller, Access the Internet and download link to compressed file
- Extract contents to Desktop
- From Start menu, Open 'Windows Powerhell ISE' as an administrator (right-click Powershell)
- Open the Powershell script "CREATE_USERS.ps1"
  - _Because the script is not digitally signed, it cannot run (Security feature)_
- Enter the prompt "Set-ExecutionPolicy unrestricted" and select 'Yes to All'
  - _Allows for all scripts to be run_
- Change directory in PowerShell to where the script is stored
  - Enter cd C:\users\A_rturnquest\desktop\AD_PS-master
- Click 'Run' in Powershell window
  - _1000 users will be generated_
- Open "Active Directory Users and Computers"
- Select "Users" folder and confirm names have been added.

