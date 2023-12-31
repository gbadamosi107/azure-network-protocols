<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1: Create Resources in Azure: Windows 10 VM and Linux (Ubuntu) Server VM
- Step 2: Observe ICMP Traffic  
- Step 3: Create a Network Security group on Linux to block ICMPs from Windows VM 
- Step 4: Observe SSH Traffic
- Step 5: Observe DHCP Traffic
- Step 6: Observe DNS Traffic
- Step 7: Observe RDP Traffic


<h2>Actions and Observations</h2>

<h3>Step 1: Create Resources in Azure</h3><br>

<p>
<img src="https://i.imgur.com/892yARI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p><br>
<p>
In Azure, create two VMs, one will be Linux (Ubuntu) machine and the other will be Windows 10 machine. Place both VMs on the same VNET. 
</p>
<br />

<h3>Step 2: Observe ICMP Traffic</h3><br>

<p>
<img src="https://i.imgur.com/XeD4ExD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p><br>
<p>

<p>
<img src="https://i.imgur.com/76a5lVr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p><br>
<p>  
  
Use Remote Desktop to connect to Windows 10 VM1. In Windows 10 VM1, install and open Wireshark, which is a protocol analyzer that allows you to view all packets coming through the network (download Wireshark from Google.com). Then filter Wireshark to capture only ICMP traffic by typing "ICMP" in the search bar. ICMP is a network layer protocol that relays messages concerning network connection issues.  Next, open Powershell on Windows VM1 and ping Linux VM2's private IP address. Ping is a command tool that uses ICMP to check if another computer is reachable on the network. Once Linux VM2 is pinged, we will inspect the actual data being transmitted within the ICMP packets. Lastly, we will perpetually ping Linux VM2 from Windows VM1, using the "ping -t" command in Powershell. We can stop the perpetual pings by typing "CTRL C" in PowerShell.
</p>
<br />


<h3>Step 3: Create a Network Security group on Linux VM to block ICMPs from Windows VM</h3><br>
<p>
<img src="https://i.imgur.com/hdttlbo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p><br>

<p>
<img src="https://i.imgur.com/ZLNGbUY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p><br>

<p>
We will configure the firewall on Linux VM2 in Azure to deny inbound ICMP traffic from Windows VM1. Once this is done, Windows VM1 will stop receiving echo replies from Linux VM2. We will observe the result in Wireshark. To block ICMP traffic on Linux VM2, navigate to Network Security Group (NSG) in Azure. Select VM2 and edit the inbound security rule to deny ICMP. After observing the effects, we can re-enable ICMP traffic on Linux VM2 by going back to NSG in Azure and changing the inbound security rule to "allow." 
</p>
<br />


<h3>Step 4: Observe SSH Traffic</h3><br>
<p>
<img src="https://i.imgur.com/KMUXUhF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will access Linux VM2 from Windows VM1 via SSH. SSH is used when remotely connecting from one computer to another and accesing the command line. We will also set  Windows VM 1 Wireshark filter to capture only SSH packets by typing "ssh labuser@10.0.0.5" (Linux VM2's private IP address) in the PowerShell command line. When this is done, we will observe  Wireshark capturing SSH packets.
</p>
<br />

<h3>Step 5: Observe DHCP Traffic</h3><br>
<p>
<img src="https://i.imgur.com/9pals8I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Windows VM1, we will use Wireshark to filter for the Dynamic Host Configuration Protocol (DHCP), which operates on UDP ports 67 and 68, and is used to assign an IP address to the host machine when it first connects to the network. We will request a new IP address with the command, "ipconfig /renew." 
</p>
<br />d

<h3>Step 6: Observe DNS Traffic</h3><br>
<p>
<img src="https://i.imgur.com/GpMKstc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will analyze Domain Name Server (DNS) traffic by filtering it on Wireshark. We will then initiate DNS traffic for Google by typing the command, "nslookup www.google.com." DNS uses nslookup to provide the IP address for a host when the human-readable domain name is entered. DNS uses both UDP and TCP port 53.
</p>
<br />

<h3>Step 7: Observe RDP Traffic</h3><br>
<p>
<img src="https://i.imgur.com/UJPaE1j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will filter for Remote Desktop Protocol (RDP) traffic, which operates on TCP port 3389. RDP is used to remotely connect from one computer to another to gain remote desktop graphic user interface (GUI). We will filter for RDP using the port number, by typing "tcp.port==3389" in Wireshark.
</p>
<br />
