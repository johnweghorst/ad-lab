# ad-lab
Wanting to expand my knowledge of Windows Server and Active Directory environments, I followed a lab that creates two virtual machines with VirtualBox (Windows Server 2019 and a Windows 10 machine). I will create a new Active Directory forest using mydomain.com for the domain, and bulk create 1000 users with a PowerShell script. I also added a DHCP scope with the DHCP tools of Server Manager to assign IP addresses to our client machines in our internal network.

## Skills Demonstrated
- Server Manager
- Active Directory Domain Services 
- DNS and DHCP configuration
- Internal VM networking (VirtualBox)
- NAT and Routing with Routing and Remote Access Service
- PowerShell automation for bulk user creation
- Domain joining and client authentication testing

# Software and Utilities Used
* Oracle VirtualBox
* PowerShell
# Operating Systems Used and Tools Used
* Windows Server 2019
* Windows 10
* PowerShell (script from: https://github.com/joshmadakor1/AD_PS/archive/refs/heads/master.zip)
# Walkthrough:
* We choose the Windows Server 2019 iso and load it into VirtualBox.
<img width="975" height="481" alt="image" src="https://github.com/user-attachments/assets/355f8306-9e74-4fad-a016-e2739c34221b" />

* Right click on the created Windows Server VM and choose Settings. Under “Expert”, head to Network and create two adapters. One will be ‘NAT’, which is already there for us, and another for the internal network. This allows internet connectivity and connectivity between devices in the domain.
Under ‘General”, go to Features and turn on bidirectional to allow copy/paste from the host machine to the virtual machine.
<img width="886" height="402" alt="image" src="https://github.com/user-attachments/assets/a1cc3084-ba62-46b7-bee7-7abbf0d570c9" />
<img width="555" height="470" alt="image" src="https://github.com/user-attachments/assets/515dd414-70d5-47b6-bec1-c7f1e1448a0b" />

* Click the Storage option next. Click Add Optical Drive options and choose VBoxGuestAdditions.iso. This is located in the install directory of VirtualBox. This is what will actually allow us to copy and paste between the host and VM.
<img width="964" height="545" alt="image" src="https://github.com/user-attachments/assets/9309008d-4071-4342-8860-dba103ca04a2" />

* Start the VM and proceed with the install. Choose Desktop Experience for the GUI. Choose ‘Custom’ under the Windows setup. Click Next on any prompts.
<img width="975" height="714" alt="image" src="https://github.com/user-attachments/assets/fa944576-d3e1-47d0-b52c-f0a570812378" />

* Once it installs, create a password and we’re in.
* Since CTRL+ALT+DEL will toggle your computer’s Task Manager, you have to use VirtualBox’s built-in option. Select Input > Keyboard > CAD at the top on the menu of VirtualBox. Alternatively, by default the “Host” key is the rightmost CTRL on your keyboard. Pressing this + delete can also be used here.
<img width="974" height="408" alt="image" src="https://github.com/user-attachments/assets/97067861-1cb4-4022-99a3-094b4b603939" />

* Once everything gets loaded, head to the Network and Network and Sharing options settings and then click on Change Adapter Settings. To tell which is connected to the internet and which is the internal network, right click on an adapter and choose Status and then Details in the window. The one connected to the internet will display a normal IP address and the internal network will show and APIPA address (169.254…) because we haven’t configured a connection for it yet. Rename accordingly one as the internet and one as the internal network. 

<img width="941" height="506" alt="image" src="https://github.com/user-attachments/assets/a59c51a1-ada1-4645-8f51-0709baaf4389" />

<img width="945" height="475" alt="image" src="https://github.com/user-attachments/assets/5022ee8f-1b6b-4ab4-9a70-50cb1011881c" />

* Right click on the internal adapter again and select Properties, then TCP/IPv4 and click Properties. We will be assigning a static IP address to this adapter for our internal network. We can choose any of the RFC 1918 designated address ranges, and I will be choosing 172.16.0.0/12 here. I will set the IP address to 172.16.0.1 with a subnet mask of 255.255.255.0, just for the ease of working with /24. Leave the default gateway blank and put 127.0.0.1 as the DNS server because our server will act as its own DNS.

<img width="628" height="719" alt="image" src="https://github.com/user-attachments/assets/21e358fe-2347-4bd4-b3e8-2756d0952ba3" />

* Next, we’ll install Active Directory Domain Services. Open up Server Manager, then click Add Roles and Features. Next, Next, Next, then Active Directory Domain Services (I have already installed it). Restart if needed.

<img width="975" height="685" alt="image" src="https://github.com/user-attachments/assets/5e09104b-0b7b-4a5d-b5ae-a7eca7068cae" />

<img width="975" height="685" alt="image" src="https://github.com/user-attachments/assets/547ed835-97f7-49a1-81be-e38813d5103a" />

* After install, we have to set our server as a Domain Controller. Click the notification flag at the top of the main screen of Server Manager and click “Promote this server to a domain controller. Choose Add New Forest, then type what ever you want to name your domain. I have just chosen ‘mydomain.com’. Continue with Next and enter a password, do not delegate it as a DNS server and proceed with install. I left everything else default.

<img width="975" height="714" alt="image" src="https://github.com/user-attachments/assets/673dc12e-d10c-408b-9ef8-4edf46ca8e30" />

* This will take a while. Go have a coffee or take a short walk to stretch your legs. It will automatically restart and we’ll have to log in. Notice that we can now log in to our domain we created on the login screen.
* Now we will go about creating our first OU (Organization unit) and object (the user) inside Active Directory using Active Directory Users and Computers. Click on Tools at the top and the option.

<img width="975" height="601" alt="image" src="https://github.com/user-attachments/assets/a0e7e44d-3b19-4b51-a996-303de6b6a13a" />

* We’re going to make a new admin account. Right-click on “mydomain.com” on the left and choose New > Organization Unit. Name it Admins or something similar. Likewise, right click on the new OU and choose New > User. Name it something and choose what ever password options you want.

<img width="692" height="588" alt="image" src="https://github.com/user-attachments/assets/3109cd6d-9e23-4b24-81a6-37eb70afeb88" />
<img width="813" height="495" alt="image" src="https://github.com/user-attachments/assets/c28a94f0-7b9a-4282-a816-16a19a944058" />

* We will add this user to the Admin group. Right click on it and select Properties, then select the Member Of tab and add the user to Domain Admins. Now we can test the account we just made by logging out of the default Admin account and into it. 
* Next, we’ll set up Remote Access so our client machine on our internal network can access the internet through our DC acting as a default gateway. Add Roles and Features > Remote Access. Choose both of the first options.

<img width="888" height="645" alt="image" src="https://github.com/user-attachments/assets/7e17b2d6-97ac-42ef-a4c4-24deb69f29ac" />

* Next, next, next, install. It’s successful. Now, with the role installed, we need to actually configure Routing and Remote Access.
* Now we’ll need to configure the Remote Access services and NAT so our Windows 10 client machine can access the internet through the DC. Click Tools > Routing and Remote Access on Server Manager. Right-click the DC (local) and choose Configure.

<img width="975" height="625" alt="image" src="https://github.com/user-attachments/assets/f671a947-c9be-4f2e-86c7-8efd4b06cda7" />

* Choose the NAT radio button and click Next. Select our internet facing connection (the non-internal adapter), Next and proceed through the steps.

<img width="789" height="675" alt="image" src="https://github.com/user-attachments/assets/befc8aa8-0f45-4678-9655-0217c82296d2" />

* Now that Remote Access is installed and configured, we can install DHCP so our Windows 10 client machine can be assigned an IP address automatically from our pool. We will install the DHCP role which will allow our Windows 10 pc to obtain an IP address automatically from the DC.
* Click on Add Features and Roles, and install DHCP server.
* After it is installed, we will configure a DHCP scope for our addresses. A scope is a range of addresses that a DHCP server will assign to members of specific networks. In our example, our DHCP server will hand out addresses in the 172.16.0.100-200 range, with a subnet mask of 255.255.255.0. I will set the lease time to 1 day. What the lease is depends on your use application for the DHCP scope. A local restaurant Wi-Fi should have a shorter duration lease because people are coming and going, compared to an office setting where the same people will be every day. Click on Tools > DHCP, then right-click on IPv4 and select New Scope.

<img width="817" height="663" alt="image" src="https://github.com/user-attachments/assets/57ab2c88-5a5e-4437-a8ca-843279969ce7" />
<img width="811" height="666" alt="image" src="https://github.com/user-attachments/assets/fe521238-a9a6-418e-b91e-8e58a7e7c96e" />

* No exclusion options unless you want them. DHCP exclusions are IP addresses that will not be given away by DHCP servers to clients. These are useful if you have things that need a static IP such as a web server.
* Enter 172.16.0.1 as the router for this DHCP scope. Click Next until it ends and it’s configured.
* The next step requires using a PowerShell script we will download from https://github.com/joshmadakor1/AD_PS/archive/refs/heads/master.zip. It will bulk create 1000 AD users for us. To get the script onto the Virtual Machine, we’ll have to get it from the internet and we have to disable some security features on the DC so as not to get spammed with popups when browsing the internet. These settings should not be disabled in a production environment as it is a security risk, but for this VM project it eases things up by removing the warnings that get spammed when trying to access the internet through a browser. In Server Manager, click 1. Configure this local server next to the Quick Start box, then click ON switch next to IE Enhanced Security Configuration and change it to OFF. (I’ve already changed it to off)

<img width="975" height="655" alt="image" src="https://github.com/user-attachments/assets/dc42184c-6ed6-4e22-a06d-b8b2dd4bfdeb" />

* The next step is running the PowerShell script. Search for Windows PowerShell ISE in the start menu, right-click it and ‘Run as administrator’. Open the file folder and navigate to where you extracted the script. To run the script, we have to authorize PowerShell to run it otherwise it will have a security error about helping you not run scripts you don’t trust. Search for ‘PowerShell ISE’ from the Start menu, right-click, and Run as administrator.

<img width="623" height="408" alt="image" src="https://github.com/user-attachments/assets/449832bb-b38e-4898-b9bc-73b4a5424238" />

* Once inside, open up the 1_CREATE_USERS.ps1 file. Then, in the bottom blue area, type “Set-ExecutionPolicy Unrestricted" without quotations and then select Yes to All to be able to run all scripts. 

* Next, click the green “play” button at the top to run the script.<img width="152" height="147" alt="image" src="https://github.com/user-attachments/assets/e8582f1b-89cb-4aed-a6ae-6a988aa35584" />

* You may need to add the names.txt to your System32 folder.

<img width="394" height="634" alt="image" src="https://github.com/user-attachments/assets/15f63325-89f4-49b2-a1f3-ca34fb2089cf" />

* The script runs.
* Now, it’s time to make a W10 client machine.
* Do so in VirtualBox.
* Make network adapter 1 for internal network. It is set this way because we want our client machine to be in the internal network of the DC. This means our host will get its internal IP from the DHCP scope we set up earlier, and it will also connect to the internet using the DC’s NAT and routing capabilities. We don’t want our client machine having internet access through VirtualBox’s NAT adapter. Enable bidirectional copy and paste and VboxGuessAdditions too if you want.
* During installation choose I don’t have a product key and Windows 10 Pro. Select Custom installation and proceed through. Select Offline account when prompted to sign in and "Limited experience" when prompted to connect your Microsoft account. * * Pick stuff and finish the set up, it doesn’t matter. Enter any username and password that you want.
* Once we’re in, let’s check our network settings. Open up a command prompt, and type ipconfig /all.

<img width="625" height="320" alt="image" src="https://github.com/user-attachments/assets/7d18aacb-c1b1-428d-bffa-e9c0aa3ff146" />

* We are on the internal network and have been supposed with an IP address in our DHCP scope we configured earlier.
* Now, we need to add this PC to the domain. Search up “View your PC name” under the Start Menu and open it.  Scroll down to “Rename this PC (advanced)”. Click Change.

<img width="633" height="114" alt="image" src="https://github.com/user-attachments/assets/d6e682b3-3130-40cb-af3f-73d786f781bb" />

* Rename the computer what ever you want, I’m going to use CLIENT1. Then also add it to your domain, in my case mydomain.com. Enter one of your admin credentials and we join.

<img width="972" height="828" alt="image" src="https://github.com/user-attachments/assets/7119f00d-7bd1-4e7d-85bd-b3a08a474db6" />

* While the W10 machine restarts, hit up AD on the Server to see that our PC was created as an object in Active Directory under 'Computers'. 

<img width="681" height="453" alt="image" src="https://github.com/user-attachments/assets/a3bfbde5-f955-40eb-bd7d-7dfa56d7cfda" />
<img width="975" height="362" alt="image" src="https://github.com/user-attachments/assets/74882c59-6f75-40f5-9fda-3ca9254b3ed5" />

* We can also see the DHCP lease if we look up the DHCP tools in Server Manager. 
* Tada! A simple Active Directory lab utilizing a PowerShell script to simulate real users and setting up a DHCP scope. We can also log in to one of the user accounts on the CLIENT1 PC. The password for all accounts we created is Password1. Give it a try.

* <img width="355" height="147" alt="image" src="https://github.com/user-attachments/assets/eb641006-4d48-4bca-b585-5182400834a6" />

* This lab I have learned my way around Windows Server and Server Manager a bit more and practiced with tools I’ve rarely used before. I got more familiar around Active Directory and hope to do more labs in the future involving GPOs and Groups.
* Thanks for following along.










