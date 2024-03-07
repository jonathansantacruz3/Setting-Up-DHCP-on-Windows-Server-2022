![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/3ed624f4-9242-49d9-844d-aeb0400093cc)![Setting Up DHCP on Windows Server 2022](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/0172c924-7185-4008-825a-ff01e0aed0b8)


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


<h3>Post DHCP Configuration</h3>

Once the role finished downloading it appeared as a tab on the left panel of the Server Manager console. I clicked on it and it prompted me to finish the configuration. This includes the creation of security groups for the DHCP administration. After hitting "Commit", it should be ready for IPv4 configuration. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/dc1bb062-e39f-4493-954d-18ddba0238a1)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/a9b0ffc4-f9ef-4628-98c1-50c8e7e3ded1)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/0a35d911-aacb-4794-ba9d-f3b272786063)

<h3>Setting a DHCP Scope</h3>

A scope is the pool of IP addresses utilized by the DHCP server to assign its network devices. Depending on the network, one might want to use the a wide range of IP addresses to support an enterprise or the network might be small and only need a small range. Networks can employ a system where the first 10 addresses can be reserved for vital production devices such as the server, default gateway, switches, access points, etc. This method can streamline any upgrades, changes or troubleshooting to those devices in the future. In this project I decided to use a limited pool of IPs (192.168.0.25-192.168.0.50). I clicked on the DHCP tab on the left panel and right-clicked on the Server DNS-IRONMAN and chose the "DHCP Manager".This opened up a snap-in on the MMC (Microsoft Management Console).

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/cd5fbe76-a18f-46cd-b196-58573f0fb132)

I clicked on DNS-IRONMAN in the left panel to open up the IPv4 options. I then right-clicked the IPv4 icon and selected the "New Scope" option that brought up the Wizard to start the process. After pressing on the "Next" button, it asked to name the scope to make it easier to identify and an option to add a description. This is useful in large enterprise environments. I named mine "ProjectLAN". 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/b6e45e31-b2d7-4c94-b77c-11ee0edb7f94)

It then asks to set the beginning and ending of the IP pool. In the same window, it asks to set the length of the network ID and subnet mask to help distinguish it from the host ID.

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/77d50987-a1a4-422c-91d5-08c245eb660f)


In the next window, it gave me the opportunity to set any exclusions I might want to set. Exclusions are useful when the entire range of the host IDs are in the pool, but there are certain IPs you might want to exclude so the DCHP server doesn't lease them out. For example, you wouldn't want it to hand out your server's IP or default gateway's IP address. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/4123170b-7a69-4caa-9ea7-dc159bab78f9)

In the next window, I got to set up the duration of the leases on the IP addresses assigned. The default length is 8 hours, but if you were in a library environment, you would want to shorten the timeframe. You wouldn't want to run out of IP addresses quick by leasing IPs longer than needed. This project doesn't require a spcific duration so I left it at its default. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d943f9d4-3019-4239-a14c-093a0101aab4)

After clicking "Next", it asked to set any additional DHCP configuration options such as setting IP addresses for routers, default gateways, DNS servers, or WINS. After choosing to add them, I can set my default gateway and DNS servers. This project is a private network with no access to the internet, however, I just wanted to exhibit the configuration steps for demonstration purposes. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/474c270a-c71e-4c79-8171-2be812cbe226)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/cb24c9ea-25b7-476f-b41d-706151d33b80)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/bd941722-770c-4c14-bbf7-3e5134170a51)

The WINS (Windows Internet Name Service) is the older brother of DNS and was used in older systems. In this case, I didn't need to configure this step so I clicked on "Next" and chose to activate the scope by pressing "Next" and "Finish". 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/f8040780-4cd9-4deb-b893-68d03098e987)

The MMC gives me a view of the contents, including the address pool, current leases, reservations, scope options, and policies of the ProjectLAN scope I created. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/c316b4b4-9a8d-435c-9bb6-e3c4d21038df)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/945690de-c5c9-4c16-8012-4f9d69befe38)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d15e2708-d26c-4738-b271-1aea1f9203c1)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/8ee108c9-dbba-4fdd-922d-439881497985)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/18700216-3794-4f7b-afd2-7975ba5cb097)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/5ede302f-94da-4367-abbf-084f43aa045b)

<h3>Client DHCP Set Up</h3>

I want to set up my client PC to be able to receive an automatic IP address anytime it boots up. I am running a Windows 11 Pro and the first thing I did was click on the Windows logo in the task bar, then went to the "Settings" option with the gear icon. I navigated to "Network and internet" in the left panel and found "Advanced network settings". I went over to the "Ethernet" tab and under "More adapter options", I clicked on "Edit". This brought up a window that gives the option to change IPv4 settings and within that option I chose the two options to "Obtain an IP address automatically" and "Obtain DNS server address automatically" After applying the changes by pressing "OK" I opened up my command line. 

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/51769f69-5a20-439c-ae51-2328818e0bb9)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/77ee19e5-84e1-456c-8ee9-11ad85dec622)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/d9eb01f5-e279-4bfd-8ef6-f175bbac887d)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/3a81f2b4-98e2-4543-b05b-49bf5db31e18)

![image](https://github.com/jonathansantacruz3/Setting-Up-DHCP-on-Windows-Server-2022/assets/151465848/c9c853e4-4626-46ed-88df-5cacc8a81827)


