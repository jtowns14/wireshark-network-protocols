<p align="center">
<img src="https://i.imgur.com/Pe5GT2M.png" height="40%" width="60%"/>
</p>

<h1>Inspecting Network Protocols (ICMP, SSH, DHCP, DNS, and RDP) with Wireshark</h1>
The focus of this project was on inspecting network protocols such as ICMP, SSH, DHCP, DNS, and RDP using Wireshark. I utilized Microsoft Azure Virtual Machines and Network Security Groups to simulate different network scenarios and understand the impact on network traffic. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Network Security Groups)
- Remote Desktop
- PowerShell/Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create 2 virtual machines, one with Windows 10 and the other with Ubuntu
- Connect to the Windows 10 VM using Remote Desktop
- Install Wireshark within the Windows 10 VM
- Initiate a non-stop ping to the Ubuntu VM, add an inbound rule in the Ubuntu VM’s Network Security Group to deny ICMP traffic, and observe the effect in Wireshark
- Re-enable ICMP traffic in the Ubuntu VM’s Network Security Group, observe the effect in Wireshark, then end the ping activity
- SSH into the Ubuntu VM and observe the effect in Wireshark, then exit SSH
- In the Windows 10 VM, attempt to issue the VM a new IP address using ipconfig /renew and observe DHCP traffic in Wireshark
- Use the nslookup command and observe DNS traffic in Wireshark
- Observe ongoing RDP traffic in Wireshark
- Delete Resource Groups in Azure


<h2>Synopsis</h2>

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/96b47b3a-aed1-45f9-a23e-3119f5dece6e)


<p>

</p>
<p>
To begin this lab, I initiated the setup of two virtual machines: VM1 with Windows 10 and VM2 with Ubuntu. After their creation, I verified their presence in the same virtual network (vnet) and subnet using Network Watcher
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/d9d949b5-601e-42c3-a5a1-e38fd77e582a)

<p>


</p>
<p>
I copied and pasted the public IP address, located in the upper right corner, into Remote Desktop to establish a connection with VM1 and proceed with the lab tasks.
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/1e04eaab-96f2-49fd-ae52-8035c8584bec)

<p>

</p>
<p>
After accessing VM1, I utilized Microsoft Edge to download and install Wireshark. Upon opening Wireshark and Windows PowerShell, I applied a filter for ICMP traffic (displayed in the green line) and initiated a continuous ping to VM2 using its private IP address. Wireshark then captured each ping request from VM1 to VM2 and the corresponding reply from VM2 to VM1.
</p>
<br />

<p>
<img src="https://i.imgur.com/r2sTBjV.png" height="70%" width="70%"/>
</p>
<p>
After ensuring that I could ping VM2, I accessed VM2's Network Security Group and created a rule to block incoming ICMP traffic, thereby preventing VM2 from responding to pings.
</p>

<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/2b23ce46-1151-40ef-81ea-5618578bedc9)


<p>


</p>
<p>
With VM2 unable to receive ICMP traffic, the ping requests now timeout. In Wireshark's "Info" section, we can observe ongoing requests sent by VM1, but VM2 doesn't respond anymore, displaying a "no response found!" message.
</p>
<br />

<p>
<img src="https://i.imgur.com/jDPXYbg.png" height="15%" width="15%"/>
</p>
<p>
I revisited VM2's Network Security Group, accessed the previously created rule, and changed the "Action" to "Allow," restoring VM2's ability to receive incoming ICMP traffic.
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/561aa250-367c-4091-9737-e013ba8bf719)


<p>

</p>
<p>
After permitting ICMP traffic again, VM2 resumed sending replies as observed in both the PowerShell terminal and Wireshark. Having completed the ICMP traffic observation in Wireshark, I terminated the ping activity in PowerShell.
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/c696cfdc-82a3-4b9e-b1d9-0df9d66ef960)


<p>

</p>
<p>
I then filtered for SSH traffic in Wireshark and used the PowerShell terminal to “SSH into” VM2. Connecting to VM2 using SSH, along with typing and executing commends, generated SSH packets that could be observed in Wireshark. Using the “exit” command, I ended the SSH session and returned to the original terminal.
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/772c2131-bd01-4b8c-bd3d-a743a1c0ebdd)


<p>

</p>
<p>
To inspect DHCP traffic, I applied a filter for DHCP in Wireshark and executed the "ipconfig /renew" command on VM1 to try obtaining a new IP address. Despite the IP address remaining unchanged, Wireshark captured the request and acknowledgement, confirming the generation of DHCP traffic
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/d4da170f-1543-47e1-8234-e9932e9d8141)


<p>

</p>
<p>
After filtering Wireshark for DNS traffic, I utilized the "nslookup" command to fetch google.com's information. The terminal showed both IPv4 and IPv6 addresses, and Wireshark's "Info" section displayed A and AAAA records, matching the types of IP addresses observed.
</p>
<br />

![image](https://github.com/jtowns14/wireshark-network-protocols/assets/139197948/e46e24c1-6efd-4b11-a499-67e9fba8a9a2)


<p>

</p>
<p>
Finally, I used Wireshark to examine RDP traffic, which didn't require the PowerShell terminal since my physical computer's connection to the VM was generating RDP traffic continuously. Wireshark displayed real-time packets sent during the active RDP connection. While other protocols were filtered by name (e.g., "DNS"), for RDP, I opted to filter using its port number (tcp.port == 3389), both methods work effectively as long as the port is known!
</p>
<br />

<p>

</p>
<br />
