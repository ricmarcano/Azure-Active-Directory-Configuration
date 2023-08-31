<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Azure Active Directory: Configuration</h1>
This lab builds upon the previous one in which I installed Active Directory and established a domain controller. Now, I'll proceed with configuring Active Directory, enabling a client to join the domain, and creating user accounts.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Configuration Steps</h2>

Now that Active Directory is set up on the domain controller VM, let's create new groups and users. Open the Active Directory Users and Computers console. Right-click on the domain you've made (like testdomain.com in my case) to make new Organizational Units (OUs). I've made two OUs named _EMPLOYEES and _ADMINS. This naming helps with a future PowerShell script. Inside the _ADMINS OU, I made a new User named Rick James. Rick will get special admin rights through a Security Group. To do this, right-click the user > Properties > Member Of > Add the right security group. For example, I added Rick to the Domain Admins group. Moving forward, I'll use Rick's account for any more changes. I'll log off as "ricardood" and log in as rick_admin.

![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/de1a8a1f-a463-4620-b0df-436dbbe48125)
![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/9a003546-0630-431c-9157-9085c8ea096b)
![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/705e02ba-352d-4bff-b6eb-f807ba7f33b7)

Before Client-1 can join the domain, the DNS settings have to be set up correctly. The DNS server should point to the private IP address of the domain controller. In the Azure portal, go to the Networking tab > select Network Interface > DNS Servers > Custom. Input the domain controller's private IP address in the DNS servers section > save the changes. Afterward, restart the client VM to ensure the DNS changes are applied.

![Screenshot 2023-08-31 at 12 48 45 PM](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/b4c03960-d2d6-42a0-b07a-032f71bc3c91)

Let's have the client VM join the domain now. In the client VM's System menu, choose "Rename this PC (advanced)" > "Change." Enter the domain and the required credentials to let the client join the domain. I'm using Rick James' login for this lab. Remember, the login details need to be entered within the domain context (testdomain.com\rick_admin). After this, the client should be successfully added to the domain. On the domain controller, you should now see the client listed under Computers in the Active Directory Users and Computers panel.

![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/e703de5c-5dd8-42d2-8ecc-969a82b85b23)
![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/e95a9934-b541-4bf9-b716-70adcdd55f00)

Before domain users can utilize the client computer, Remote Desktop needs to be enabled for non-admin users. While logged in as the administrator (in my case, Rick), open System Properties > Access Remote Desktop > choose users permitted to access this PC remotely > grant Remote Desktop access to Domain Users. Now, non-admin users can log in to Client-1 remotely. Usually, a Group Policy can achieve this across multiple systems, but for this lab, we're not using a Group Policy to make this change.

![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/03dfc1ca-a17e-4d82-bde8-ccfd9b714519)

Users can be created by manual means or through a script. In this lab, a PowerShell script will be used, which is available here. On the domain controller, open PowerShell ISE with administrative privileges (ensuring you're logged in as an admin on the domain controller). Generate a new file, insert the script into the ISE console, execute the script, and observe the accounts coming into existence.

![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/fb0edba4-4a45-4cfe-8cad-3e840820f107)
![image](https://github.com/ricmarcano/Azure-Active-Directory-Configuration/assets/141169092/63cae4a7-931a-4eca-a91c-d6a73cf95ae7)
