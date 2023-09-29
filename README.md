<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Observing Network Traffic</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark and experiment with Network Security Groups. Some notes and screenshots are provided for highlights of some or all explanations.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- MacBook Air
- RD Client (Remote Desktop App)
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro
- Ubuntu Server 20.04

-----

<h1>Part 1: Set Up Resources</h1>

- Create a Resource Group.
- Create a Windows 10 Virtual Machine (VM) and assign it to the previously created Resource Group.
- Allow the VM to create a new Virtual Network (Vnet) and Subnet during setup.
- Create a Linux (Ubuntu) VM and assign it to the same Resource Group and Vnet.
- Observe the Virtual Network within Network Watcher.

</br>

-----

<h1>Part 2: Observing Traffic Types</h1>

<h2>2.1 Observe ICMP Traffic</h2>

- Use RD Client to connect to your Windows 10 Virtual Machine.
- Install Wireshark within your Windows 10 VM.
- Open Wireshark and apply a filter for ICMP traffic.
- Retrieve the private IP address of the Ubuntu VM and ping it from within the Windows 10 VM.
- Observe ping requests and replies in Wireshark.
- Ping a public website (e.g., www.google.com) from the Windows 10 VM and monitor the traffic in Wireshark.
- Start a continuous ping from your Windows 10 VM to your Ubuntu VM.
- Disable incoming (inbound) ICMP traffic in the Network Security Group of your Ubuntu VM.
- Observe the ICMP traffic in Wireshark and the command line Ping activity on the Windows 10 VM.
- Re-enable ICMP traffic in the Network Security Group of your Ubuntu VM.
- Observe the ICMP traffic in Wireshark and verify that the ping starts working.
- Stop the ping activity.

<h2>2.2 Observe SSH Traffic</h2>

- Filter for SSH traffic in Wireshark.
- SSH into your Ubuntu Virtual Machine from your Windows 10 VM using its private IP address.
- Observe SSH traffic in Wireshark while typing commands.
- Exit the SSH connection by typing 'exit' and pressing [Enter].


<h2>2.3 Observe DHCP Traffic</h2>

- Filter for DHCP traffic in Wireshark.
- From your Windows 10 VM, attempt to renew your VM's IP address using the command 'ipconfig /renew'.
- Observe the DHCP traffic appearing in Wireshark.

<h2>2.4 Observe DNS Traffic</h2>

- Filter for DNS traffic in Wireshark.
- From your Windows 10 VM's command line, use 'nslookup' to query the IP addresses of 'google.com' and 'disney.com'.
- Observe the DNS traffic in Wireshark.

<h2>2.5 Observe RDP Traffic</h2>

- Filter for RDP traffic in Wireshark (tcp.port == 3389).
- Observe the constant stream of traffic. Discuss why RDP traffic is continuous compared to other activities.
- Answer: RDP provides a continuous live stream from one computer to another, resulting in constant traffic transmission.

</br>

-----
<h1>Conclusion</h1>
In conclusion, we've gained a deeper understanding of various network protocols, security measures, and the influence of geographic locations on online experiences. This journey highlights the significance of vigilance in monitoring and managing network traffic, recognizing its pivotal role in today's digital world.
