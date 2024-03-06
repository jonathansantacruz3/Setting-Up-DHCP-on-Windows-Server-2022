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
