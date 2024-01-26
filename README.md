<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark and experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines) 
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04


<h2>Actions and Observations</h2>

<p>
 First, you will need to create two VMs on Azure. One machine will be a Linux machine, and the other will be a Windows 10 machine. Both will have two CPUs and must be on the same VNET. Once that is done, go to the Windows machine and download Wireshark. I will attach a link to the Wireshark download. https://www.wireshark.org/download.html Once installed, open Wireshark and filter for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol to test connectivity between hosts. When we filter Wireshark only to capture ICMP packets and ping our Linux machine's private IP address, we can see the packets on Wireshark.
</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/2d2bd01c-5e12-42ca-b7b0-e4381024b75b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
We can inspect each packet and see the actual data being sent in each ping. The picture below demonstrates just that.
</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/40eecdc3-ce1f-4a25-b3ad-e4956c03e187" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
In the next portion of the lab, we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it; while the Windows machine is pinging the Linux machine, we will go to the Linux machine and block inbound ICMP traffic on its firewall. Once we do that, we will stop receiving echo replies from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on Azure's Linux Network Security Groups page.
</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/8995c480-93f0-4f88-88c5-d089b9196131" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/09d8d425-034c-4e34-b012-b5602a4b6b22" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Next, we will use our Windows machine to SSH the Linux machine. SSH has no GUI. It just gives the user access to the machine's CLI. We will set the wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5," we can see that Wireshark starts to capture SSH packets immediately.
</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/a4d8fa58-7f70-4155-b874-14866f7c48e1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Now, we will use Wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol, and it works on ports 67/68. It is used to assign IP addresses to machines. We will request a new IP address with the command "ipconfig /renew." Once we enter the command, Wireshark will capture DHCP traffic.

</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/ea4d4740-cfc5-4624-820f-3a33f293c985" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com." this command essentially asks our DNS server what Google's IP address is.
</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/13a3effb-1bc8-4667-b4bc-01f3f770cf6f" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Lastly, we will filter for RDP traffic. When we enter tcp.port==3389, traffic is spammed non-stop because we are using Remote Desktop Protocol to connect to our Virtual Machine.
</p>
<br />
<p>
<img src="https://github.com/lenac0des/azure-network-protocols/assets/71302899/873f9fa3-fbb2-4566-a002-902e8f72167c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

