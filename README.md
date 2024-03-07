![Setting Up DHCP on Windows Server 2022](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/0172c924-7185-4008-825a-ff01e0aed0b8)


<h1>Setting Up DHCP on Windows Server 2022</h1>
This tutorial outlines the implementation of a Dynamic Host Configuration Protocol (DHCP) server within a Hyper-V Virtual Machine. DHCP is the protocol that leases IPs from a scope of addresses configured in the system. I go over the procedure, starting from deployment to validation. I also review DHCP's DORA (Discover, Offer, Request, Acknowledgement) process in obtaining an address from the server to gain a better perspective of how it functions behind the scenes. DHCP can be an afterthought for us when we connect our devices to the internet. We rarely think of how this system is done and the expediency it offers to our lives. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Hyper-V (Virtual Machines/Compute)
- Server Manager


<h2>Operating Systems Used </h2>

- Windows 11 Home (23H2)[HOST]
- Windows Server 2022
- Windows 11 Pro 


<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<h3>Infrastructure Summary</h3>

For this project, I used Windows Server 2022 and Windows 11 Pro on a virtual machine in Hyper-V. These virtual machines are on a private switch and, therefore isolated from the internet. Windows Server 2022 plays the DHCP server role, while Windows 11 Pro acts as the client receiving an automatic IP address assignment from the server.  The network ID I configured for this network is 192.168.0 /24 using only IPv4 addressing. The scope of the configuration will range from 192.168.0.50-192.168.0.100. 

![Setting Up DHCP on Windows Server 2022-infrastructure](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d6c8d872-a3e0-4818-805c-0c01ea273975)

<h3>Setup DHCP Role on Server Manager</h3>

Before setting up the role, I verified my server had a human-readable computer name. I also verified it was in a workgroup environment for file sharing and network connectivity.  Static IP assignments for DHCP servers are essential for consistent, reliable communication, and in this case, I set it up with an IP address of 192.168.0.10 by the name of DNS-IRONMAN. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/2e90b574-e196-4ac5-a453-783e47b7b22e)


The first step is to click on the "Dashboard" tab in the Server Manager console. I clicked on the "Add roles and features" to create a DHCP role. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/cda71bc2-23c5-4cb0-9394-acbcc19f28ed)

This brings up the Wizard that details the prerequisites I explained earlier. In the next window, I selected a "role-based or feature-based installation".

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/358135f5-77f3-4eba-91da-362b7f0cd30e)


![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/165a0aca-5585-4e97-bf24-e4cb985220e3)

In the next window, I made sure the computer name and the static IP address are accurate.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/aa5e0b30-1682-41c9-8b70-62d4189d399a)

After pressing next, I selected DHCP server and added the management features.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/56c88e74-9be6-45f6-aa65-1fc9806a467a)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/0345db2d-5c4d-4584-86e9-59a7999d48e8)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/2bb1df80-4f36-45b5-949e-fdccda67932c)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/aafb855a-1ec0-4be2-9b05-ba0985a6fc45)

In this window, it asks if you would like the system to automatically restart if it needs to update. I didn't check the box because production systems need to stay on for peformance and reliabilty. Then I clicked "Install" to confirm.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/69a1f074-4745-4eda-8494-dcf17d281862)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/999c4e9c-6dfd-450a-8329-c901cf6f3b09)



