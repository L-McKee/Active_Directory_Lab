1) Install Microsoft Windows 10 and Server 2016 (or whatever version you want to use) within Microsoft Azure cloud software.

2) When installing the server, MAKE SURE TO SELECT A SIZE OPTION THAT SUPPORTS 2 NICs (I used D2 v5). This one got me for a bit...

3) Create a secondary Network Interface Card (NIC) on the Server 2016 machine.
      Shut down the server VM
      Go to your server VM, down to Networking
      Click "Attach Network Interface", name it whatever you want
      Restart the server VM
      
4) In the server VM, go to:
      Network Settings -> Change Adapter Settings
      Find the internal NIC and rename it to something like "Internal"
      Label the other NIC something like "Internet"
      Restart
      
5) Go back to network settings to set IP address for internal NIC
      Network Settings -> Change Adapter Options
      Double-Click Internet Protocol Version 4
      Select "Use the following IP address"
      IP: 172.16.0.1
      Subnet: 255.255.255.0
      Prefered DNS Server: 127.0.0.1 (Loopback IP)
      
6) Install Active Directory Domain Services:
      In the Server Manager main page, click "Add Roles and Features"
      Select "Role-based or Feature-based installation"
      Select the server you wish to install Active Directory Domain Services (I only had 1 option)
      Check "Active Directory Domain Services" click next
      Keep pre-set "Features"... next
      AD DS... next
      Install
      
7) Post-Deployment Configuration
      Click "Promote this computer to Domain Server
      Cick "Add new forest"
      Name it whatever you want (I chose "mydomain.com")
      Create a password, you won't use it anyway
      Do NOT check anything under DNS Options... next
      Additional Options... next
      Paths... next
      Review Options... next
      Install
      
8) Active Directory Users and Computers
      Go to Start menu, click Windows Administrative Tools
      Click Active Directory Users and Computers
      In the LEFT pane, select "yourdomainname.com"
      Right click, select New -> Organizational Unit
      Name it "Admin"
      Right click your new Admin folder, select New -> User
      Create a new user and password (I chose "a-FirstinitialLastname" as it is best practices in most enterprises)
      For the sake of keeping is simple, uncheck "User must change password at next logon"
      Check "Password Never Expires"
      Next, finish
      
9) Make this user a Domain Admin...
      In the RIGHT pane, right click your new user, select "Properties"
      Select the "Member Of" tab
      Click "Add"
      type "domain admins"
      Ok, Apply, Ok
      Sign out of the current account to access the new Domain Admnin account we made
      
10) Installing Remote Access Server / Network Address Translation (RAS/NAT)
      On the Server Manager Dashboard, select "Add roles and features"
      Next, Next... select the same server you chose for previous step
      Under Server roles, check Remote Access
      For Role services, check Routing
      Install, exit window
      On the Server Manager Dashboard, go to Tools -> Routing and Remote Access
      Right click on your computers name (mine was DC) and select "Configure and Enable Routing and Remote Access"
      Check "Network address translation"
      Check "Use this public interface to connect to the internet"
      Select the interface labeled "Internet"
      Finish and let the system intialize (this took F-O-R-E-V-E-R)
      
11) Set up DHCP on the Domain Controller
      Back to the Server Manager Dashboard -> Add roles and features
      Again, select the same server you chose from the previous 2 steps
      Under Select Server roles, select "DHCP Server", add features
      Next, Next, Install
      In the Server Manager Dashboard, go to Tools -> DHCP
      Click on the DC you created, right click "IPV4" and select "New Scope"
      
