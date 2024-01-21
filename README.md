<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Installation steps for Active directory</h2>

Before using the VMs, it is important to set the domain controller VM's IP address as static. By default, the VMs will not be able to communicate with each other if both have dynamic IPs despite being on the same vnet. If we do not make the necessary change, the client will not be able to join the domain that will be created later. On the Azure portal, click on the Networking tab on the domain controller VM. Click on the Network Interface and open the IP configurations tab. Toggle the Assignment switch to be Static and save your changes. We are making sure the domain controller has a static IP and it will be used as a reference when we make configurations.


![image](https://github.com/jacobsoto/install-Active-Directory/assets/156248197/a9743c25-bb17-4fd3-be73-8b0e0ceef4fe)

![image](https://github.com/jacobsoto/install-Active-Directory/assets/156248197/75cf6b4d-a764-4e67-8cfc-369274adf834)

![image](https://github.com/jacobsoto/install-Active-Directory/assets/156248197/5de37ba4-959f-460b-985e-5fe5866da77f)

![image](https://github.com/jacobsoto/install-Active-Directory/assets/156248197/8acc8bb5-26b8-43c6-98e6-1389071c95e4)

After setting the static IP, it is time to log in to the client VM and see if there is connectivity to the domain controller. Using ping -t (domain controller private ip address), will show that the connection is being timed out. On the domain controller VM, we need to enable ICMPv4 on the local Windows Firewall. Within the search bar, type wf.msc to open Windows Defender Firewall. Click on Inbound Rules and enable the Core Networking Diagnostics - ICMP Echo Request rules. Returning to the client VM will show that the ping is now resolving without errors



![image](https://github.com/jacobsoto/install-Active-Directory/assets/156248197/daa05844-f29d-412c-81dc-f6d551316bc2)


It is now time to install Active Directory on the domain controller VM. With Server Manager open, click on Add Roles and Features and click Next. Confirm the private IP address of the domain controller VM. In the Server Roles tab, click on Active Directory Domain Services. Click Add Features, click Next, then Install. Next we have to promote the server into a domain controller. In Server Manager, there is a warning sign in the top right corner under a flag. Click on that flag and click Promote this server to a domain controller. Click on Add a new forest and specify a domain name. In my case, I will use ernestotest.com. Specify a password for the domain and click on Next on each screen and Install.



