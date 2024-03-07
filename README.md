![Slide1](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/ba3ee2bb-396d-4c68-98c6-17629582fe8a)


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

- Setup DHCP Role on Server Manager
- Post DHCP Configuration
- Setting a DHCP Scope
- Client DHCP Set Up
- DORA Process
- Client DHCP Verification
- DHCP Server Verification

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

In the next window, I made sure the computer name and the static IP address were accurate.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/aa5e0b30-1682-41c9-8b70-62d4189d399a)

After pressing next, I selected the DHCP server and added the management features.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/56c88e74-9be6-45f6-aa65-1fc9806a467a)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/0345db2d-5c4d-4584-86e9-59a7999d48e8)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/2bb1df80-4f36-45b5-949e-fdccda67932c)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/aafb855a-1ec0-4be2-9b05-ba0985a6fc45)

In this window, it asks if you would like the system to automatically restart if it needs to update. I didn't check the box because production systems need to stay on for performance and reliability. Then I clicked "Install" to confirm.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/69a1f074-4745-4eda-8494-dcf17d281862)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/999c4e9c-6dfd-450a-8329-c901cf6f3b09)

<h3>Post DHCP Configuration</h3>

Once the role finished downloading it appeared as a tab on the left panel of the Server Manager console. I clicked on it and it prompted me to finish the configuration. This includes the creation of security groups for the DHCP administration. After hitting "Commit", it should be ready for IPv4 configuration. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/dc1bb062-e39f-4493-954d-18ddba0238a1)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/a9b0ffc4-f9ef-4628-98c1-50c8e7e3ded1)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/0a35d911-aacb-4794-ba9d-f3b272786063)

<h3>Setting a DHCP Scope</h3>

A scope is the pool of IP addresses utilized by the DHCP server to assign its network devices. Depending on the network, one might want to use a wide range of IP addresses to support an enterprise, or the network might be small and only need a small range. Networks can employ a system where the first 10 addresses can be reserved for vital production devices such as the server, default gateway, switches, access points, etc. This method can streamline any upgrades, changes, or troubleshooting to those devices in the future. In this project, I decided to use a limited pool of IPs (192.168.0.25-192.168.0.50). I right-clicked on the Server DNS-IRONMAN, and chose the "DHCP Manager”. This opened a snap-in on the MMC (Microsoft Management Console).

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/cd5fbe76-a18f-46cd-b196-58573f0fb132)

I clicked on DNS-IRONMAN in the left panel to open the IPv4 options. I then right-clicked the IPv4 icon and selected the "New Scope" option that brought up the Wizard to start the process. After pressing on the "Next" button, it asked to name the scope to make it easier to identify and an option to add a description. This is useful in large enterprise environments. I named mine "ProjectLAN". 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/b6e45e31-b2d7-4c94-b77c-11ee0edb7f94)

It then asks to set the beginning and ending of the IP pool. In the same window, it asks to set the length of the network ID and subnet mask to help distinguish it from the host ID.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/77d50987-a1a4-422c-91d5-08c245eb660f)


In the next window, it gave me the opportunity to set any exclusions I might want to set. Exclusions are useful when the entire range of the host IDs are in the pool, but there are certain IPs you might want to exclude so the DCHP server doesn't lease them out. For example, you wouldn't want it to hand out your server's IP or default gateway's IP address. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/4123170b-7a69-4caa-9ea7-dc159bab78f9)

In the next window, I got to set up the duration of the leases on the IP addresses assigned. The default length is 8 hours, but if you were in a library environment, you would want to shorten the timeframe. You wouldn't want to run out of IP addresses quickly by leasing IPs longer than needed. This project doesn't require a specific duration, so I left it at its default. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d943f9d4-3019-4239-a14c-093a0101aab4)

After clicking "Next", it asked to set any additional DHCP configuration options such as setting IP addresses for routers, default gateways, DNS servers, or WINS. After choosing to add them, I can set my default gateway and DNS servers. This project is a private network with no access to the Internet, however, I just wanted to exhibit the configuration steps for demonstration purposes. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/474c270a-c71e-4c79-8171-2be812cbe226)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/cb24c9ea-25b7-476f-b41d-706151d33b80)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/bd941722-770c-4c14-bbf7-3e5134170a51)

WINS (Windows Internet Name Service) is the older brother of DNS and was used in older systems. In this case, I didn't need to configure this step, so I clicked on "Next" and chose to activate the scope by pressing "Next" and "Finish". 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/f8040780-4cd9-4deb-b893-68d03098e987)

The MMC gives me a view of the contents, including the address pool, current leases, reservations, scope options, and policies of the ProjectLAN scope I created. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/c316b4b4-9a8d-435c-9bb6-e3c4d21038df)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/945690de-c5c9-4c16-8012-4f9d69befe38)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d15e2708-d26c-4738-b271-1aea1f9203c1)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/8ee108c9-dbba-4fdd-922d-439881497985)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/18700216-3794-4f7b-afd2-7975ba5cb097)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/5ede302f-94da-4367-abbf-084f43aa045b)

<h3>Client DHCP Set Up</h3>

I want to set up my client PC to be able to receive an automatic IP address anytime it boots up. I ran a Windows 11 Pro and the first thing I did was click on the Windows logo in the taskbar, then went to the "Settings" option with the gear icon. I navigated to "Network and Internet" in the left panel and found "Advanced network settings". I went over to the "Ethernet" tab and under "More adapter options", I clicked on "Edit". This brought up a window that gave the option to change IPv4 settings and within that option, I chose the two options to "Obtain an IP address automatically" and "Obtain DNS server address automatically" and I applied the changes by pressing "OK". 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/51769f69-5a20-439c-ae51-2328818e0bb9)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/77ee19e5-84e1-456c-8ee9-11ad85dec622)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d9eb01f5-e279-4bfd-8ef6-f175bbac887d)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/3a81f2b4-98e2-4543-b05b-49bf5db31e18)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/c9c853e4-4626-46ed-88df-5cacc8a81827)

<h3>DORA Process</h3>

The process that happens in the background is between the client and the DHCP server. The client PC will send out a broadcast message to the DHCP server and this is called the "Discover" phase. Once the message to the server is reached, the DHCP server will send an "Offer" of an IP address to the client so it can be used in the network. The client will then officially "Request" this IP address to the server and finally the server will then send back an "Acknowledgment" message back to the client. 

![Slide3](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/936d3dbe-c7c6-4c4e-b999-68a361486951)

![Slide4](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/9bcfb222-0df6-4ee1-9eb9-3020a171abbd)

![Slide5](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/dcee9e30-20c8-45ac-a7f5-b763173ff500)


![Slide6](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/3523de54-93d8-4d0f-8ca2-1802d1e3a8ce)

<h3>Client DHCP Verification</h3>

I wanted to verify that the DHCP was working properly so I opened the command line. I typed in ipconfig and as expected it displayed the first IP address in the DHCP address pool along with the DNS servers and default gateway I configured in the manager.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/bb6c5872-bd31-4fd1-a422-391e6f82d448)

<h3>DHCP Server Verification</h3>

I wanted to validate the settings in my DHCP server, so I opened the DHCP snap-in and under my leases, I was able to verify that the address 192.168.0.25 was leased to my Windows 11 Pro PC for 8 days. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/9235aba0-d28c-4bc1-9d00-77e61c906115)

If I wanted to keep this IP address in reservation for future use indefinitely, I can make use of the feature in the same window by right-clicking the address and selecting "Add to Reservation". This will create an entry in my Reservation tab. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/c71ab941-f647-4340-bc28-b5a494ddf781)


<h3>Conclusion</h3>

DHCP is a system that can save users in a network a lot of time by automatically assigning IP addresses to traverse other subnets or within its network. It uses the DORA process to assign from a pool of addresses and administrators can add in reservations, exclusions, length of leases, etc. DHCP is just another tool in our arsenal for productivity.  
