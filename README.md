# ad-lab
Wanting to expand my knowledge of Windows Server and Active Directory environments, I followed a lab that creates two virtual machines with VirtualBox (Windows Server 2019 and a Windows 10 machine). I will create a new Active Directory forest using mydomain.com for the domain, and bulk create 1000 users with a PowerShell script. I also added a DHCP scope with the DHCP tools of Server Manager to assign IP addresses to our client machines in our internal network.

# Software and Utilities Used
* Oracle VirtualBox
* PowerShell
# Operating Systems Used
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
