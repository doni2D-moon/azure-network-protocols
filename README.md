<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

![image](https://github.com/user-attachments/assets/f1bb01bc-3094-404a-93c5-adfb6638a192)

<p>
In this step, will be installing the Windows 10 virtual machine on Microsoft Azure (blue border). After creating the Resource Group (called Network Activities for this example), type "virtual machine" in the search bar and select virtual machines. On the next screen, it will show "Create." Click Create --> Azure virtual machine to create the machine. It will then lead to the window shown above. 1) Give the virtual machine a name --> 2) Select a "region" --> 3) Select the "image" (Windows 10 Pro for this example) --> 4) Select the "size" --> 5) Provide a "username" and "password" found under Administrator account and 6) Check the box under Licensing. Then click next to Disks then to Networking to select the Vnet.
</p>
<br />

![image](https://github.com/user-attachments/assets/bc86e70b-0455-4df6-9f9e-8729b42825e8)

<p>
In this step (continuation from the previous step), will select the Vnet (that needs to be the same as the Vnet for the Linux/ Ubuntu VM. Under Networking, 1) make sure the "Virtual Network" is selected and can be used for the Linux machine. In this example, the Vnet was changed to "vm-net." Then, 2) Choose "Review + create." It will then validate.
</p>
<br />

![image](https://github.com/user-attachments/assets/73cc821a-a2f6-402d-9767-5c47e5618fb4)

<p>
After it says "Validation passed," click "Create" on the bottom to create the VM. Will know it finished creating when it says Your deployment is complete.
</p>
<br />

![image](https://github.com/user-attachments/assets/d073b801-ad1a-4509-9151-5b7c11337375)

<p>
When creating the Linux VM (orange border), make sure to select "Password" to input information for the "username" and "password" that will be used to log into the VM from Powershell. Under the Networking for Linux, also make sure the Virtual network is the same as the Vnet for the Windows VM. In this example, the Virtual network is "vm-net" (the same as Windows' vnet).
</p>
<br />

![image](https://github.com/user-attachments/assets/ca7da47d-97ea-43af-b4c8-7befdb90455f)

<p>
In this step, will be logging into the Windows VM. After bring up Remote Desktop, click on the Windows VM found under Virtual machines in Azure and search for the "Public IP address" (found on the right). Type or copy the Public IP address in the Remote Desktop window and click "Connect." It will show a window where you have to log in with the information used when creating the Windows VM. Click Yes after inputting the log in information to log into the VM.
</p>
<br />

![image](https://github.com/user-attachments/assets/af1719b1-2466-4efc-89ec-77da800106d0)

<p>
Once logged in, load up a browser (in this example, Microsoft Edge) and type in www.wireshark.org to go to a page to download Wireshark (that will be used to observe traffic between the VMs). In the above image, click "Windows x64 Installer." From there, it will download and show a file. Open the file and proceed with the steps to install Wireshark.
</p>
<br />

![image](https://github.com/user-attachments/assets/ac79728c-7a3b-484c-aba9-eb5aa9d89ee3)

<p>
After installing Wireshark, search for it next to Start and load it. Once it loads, make sure "Ethernet" is highlighted and click on the blue fin on the top left. Once make it to the page above, type icmp (shown in the green bar) to filter for ICMP traffic. From the search bar next to Start, load Powershell. In Powershell, type the following command (ping Linux Private IP address) to ping into the Linux VM. (To get the Linux VM's Private IP address, in Azure, go to virtual machines --> linux vm --> scroll down and look on the right side to find the Private IP address). Pinging the address shows "Reply" from the Linux machine in Powershell and "Reply" and "Request" in Wireshark between the two machines (shown in pink). 
</p>
<br />

![image](https://github.com/user-attachments/assets/19036af8-a26f-455d-8946-b214973a991f)

<p>
In this step, will be creating a firewall for the Linux VM to block traffic from the Windows VM. Back i Azure, click on the Linux VM found on the page virtual machines. From here (on the side), click on Networking --> 1) Network settings --> 2) the link in the box named "Network Interface/ IP Configuration." It will lead to a new page.
</p>
<br />

![image](https://github.com/user-attachments/assets/25b8e1c3-d249-4f97-9b37-f2fb87a69228)

<p>
After the previous step, it will lead to this page. Go to Settings on the left side --> 1) "Inbound security rules" and Add (on the top, next to Hide default rules) to see a window pop up on the right side. In the Add inbound security rule window, input the following: Destination Port Ranges - *, Protocol - ICMPv4, Action - Deny, and Priority - 290 (above 300 ssh). All others should already be Source - Any, Source port ranges - *, Destination - Any, Service - Custom, and Name - (can be any as long as remember it). Then 2) Click "Add." It will then show the rule above the ssh rule.
</p>
<br />

![image](https://github.com/user-attachments/assets/7e26c3d3-d2ce-44e1-b816-d33cfc77ade6)

<p>
Back in the Windows VM, if give a perpetual ping of ping (Private IP address of Linux) -t in Powershell, it will show "Request timed out" revealing that the firewall of Linux blocked traffic from Windows VM. In Wireshark, will see multiple requests to the Linux VM (in pink) with no reply.
</p>
<br />

![image](https://github.com/user-attachments/assets/4c063567-6d8d-4aef-848e-c4759410e444)

<p>
To allow traffic between the VMs again, go back in Azure to the Linux VM inbound security rules. The rule with the number 290 at the very top can be changed or deleted (which can be done by clicking the trash can on the far right on that line).
</p>
<br />

![image](https://github.com/user-attachments/assets/56173e82-9577-463e-90c5-b53d6e42a38c)

<p>
Back in the Windows VM, deleting the inbound security rule will produce replies from the Linux VM in Powershell and in Wireshark. To stop the ping, press "Ctrl+C."
</p>
<br />

![image](https://github.com/user-attachments/assets/1a89b72d-fc51-442d-b721-02cbd3392d06)

<p>
In this step, will be filtering for SSH traffic. Before start filtering for SSH, click the green fin on the top in Wireshark to Continue without saving to clear the window. 1) Then type "ssh" or "tcp.port ==22" to filter only for SSH (in the green bar). In Powershell, connect to Linux by 2) typing ssh username@Private IP address of Linux (ex. labuser@10.0.0.5) --> type "yes" --> type the password (it will not show what will be typing). Once done, it will show name for Linux in green (ex. labuser@linux-vm). 3)Typing commands will show ssh traffic in Linux (that can still be shown in Windows even when changed to the Windows VM). All of what was input in Powershell from ssh in Linux to connecting to the Linux VM can be seen in Wireshark (in blue).
</p>
<br />

![image](https://github.com/user-attachments/assets/f17d435f-4068-4fda-9cea-b54fabc18e6f)

<p>
In this step, will be filtering with DHCP (udp.port 67 and 68) but to get a new IP address. 1)Type in "dhcp" or "udp.port==67 or udp.port==68" in the green bar. Then click the green fin to clear if by clicking Continue without saving. From here, go to Powershell and type "ipconfig/ renew" to get a new IP address. If only getting Request and Acknowledge, then can create a file with "ipconfig /release, ipconfig /renew" saved in c:\programdata to force it to give a new IP address. In Powershell, 2)type in cd c:\programdata --> ls --> 3) .\file name (ex dhcp.bat) to run the file and get the IP address released and renewed. It will reboot and show "Discover," Offer, Request, and ACK in Wireshark (in blue).
</p>
<br />

![image](https://github.com/user-attachments/assets/d3701348-3209-44cd-b591-09ff2fdb40ed)

<p>
In this step, will be filtering for DNS (udp and TCP port 53). Type "DNS" or "udp.port==53 or tcp.port==53" in the green bar. This is where sites we know are translated into IP addresses. Clear the area with Continue without saving (green fin). In Powershell, ping a website (ex. www.google.com or disney.com) and see the IP addresses associated with the sites in Powershell and in Wireshark.
</p>
<br />

![image](https://github.com/user-attachments/assets/01429eea-5552-47d8-99c3-a06d9bf6a8c4)

<p>
In this final step, will be filtering RDP (tcp 3389) traffic in Wireshark. Type in "tcp.port==3389" in the green bar and click the green fin to Continue without saving. Then below observe the continous traffic in Wireshark. The above image shows some of the traffic happening in Wireshark.
</p>
<br />
