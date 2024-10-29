<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




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
</p>
<p>
To get started with Network Security Groups and Inspecting Network Protocols, first create two virtual machines (VMs) on Azure: one Linux machine and one Windows 10 machine. Both VMs should have two CPUs and must be on the same virtual network (VNET). Once you have completed that, download Wireshark on the Windows machine. Here’s a link to the Wireshark download: Wireshark Download. After installation, open Wireshark and set a filter for ICMP traffic only. ICMP is a network layer protocol that relays messages related to network connection issues, and Ping is a common use of this protocol to test connectivity between hosts. By filtering Wireshark to capture only ICMP packets and pinging the private IP address of your Linux machine, you can visually observe the packets in Wireshark.
</p>
<br />
<p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We can inspect each individual packet and view the actual data being sent with each ping. The image below illustrates this process.
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the next portion of the lab, we will use the command ping -t to perpetually ping the Linux machine. This command will continuously send pings until we choose to stop it. While the Windows machine is pinging the Linux machine, we will switch to the Linux machine and block inbound ICMP traffic on its firewall. Once this is done, we will stop receiving echo replies from the Linux machine. To block ICMP, we will create a new Network Security Group (NSG) for the Linux machine, configuring it to block ICMP traffic. We can later allow the traffic by enabling ICMP on the Linux Network Security Groups page in Azure.
</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
Next, we will use our Windows machine to SSH into the Linux machine. SSH does not have a graphical user interface; instead, it provides users with access to the machine's command-line interface (CLI). We will set the Wireshark filter to capture SSH packets only. When we SSH into the Linux machine using the command prompt ssh labuser@10.0.0.5, we can see that Wireshark begins to immediately capture SSH packets.
</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will use Wireshark to filter for DHCP. DHCP, or Dynamic Host Configuration Protocol, operates on ports 67 and 68 and is used to assign IP addresses to machines. We will request a new IP address by using the command ipconfig /renew. Once we enter the command, Wireshark will capture the DHCP traffic.
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now it's time to filter DNS traffic. We will configure Wireshark to capture DNS traffic and initiate the process by typing the command nslookup www.google.com. This command essentially queries our DNS server to find out the IP address of Google.
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we will filter for RDP traffic. By entering the filter tcp.port==3389, we can see that traffic is continuously generated as we use the Remote Desktop Protocol to connect to our virtual machine.
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
