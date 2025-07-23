<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Using Wireshark within Microsoft Azure Virtual Machines</h1>
<p>In this tutorial, we will learn how to observe live network traffic in the cloud using Azure Virtual Machines and Wireshark. 
   
   This guide will walk you through setting up Windows and Ubuntu VMs in Azure and using Wireshark to observe various types of network traffic. You’ll learn how to create and configure virtual machines, monitor ICMP, SSH, DHCP, DNS, and RDP traffic, and understand the behavior of each traffic type.</p>

<h2>What You’ll Learn:</h2>

- Basics of cloud computing with Azure
- How to capture network traffic using Wireshark
- Understand protocols like ICMP, DNS, SSH, DHCP, RDP
- How to interpret real-time network behavior and apply security rules

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)
- Windows Powershell
- Various Network Protocols (ICMP, SSH, DHCP, DNS, TCP Port ===)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own, it'll work on remote desktop app)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of these OS)***
- Ubuntu Server 22.04 (Virtual Machine)

-----

## Part 1: Create and Configure Resources

1. **Create a Resource Group & Virtual Network**:
   - Go to your Azure portal and create a new Resource Group.
   - Create a Virtual Network (Vnet)

2. **Create a Windows 10 Virtual Machine (VM)**:
   - Select the Resource Group you created.
   - Create a Windows 10 VM.
   - Choose the VNet you have created.

3. **Create a Linux (Ubuntu) VM**:
   - Choose the same Resource Group and VNet as the Windows 10 VM.
  
***📖 Note: A Resource Group organizes related Azure services, and a VNet allows your VMs to communicate like they’re on the same local network. You’ll use one VM (Windows) to generate and monitor traffic, and the other (Linux) to respond to it.***
  
-----

## Part 2: Observe Network Traffic

***Wireshark is a free and powerful tool used to analyze packets and protocols on a network. It helps you see what’s happening behind the scenes.***

### ICMP Traffic

***📖 Note: ICMP is the protocol used when you “ping” another device.***

1. **Connect to Windows 10 VM**:
   - Use Remote Desktop to access your Windows 10 VM.

2. **Install and Use Wireshark**:
   - Install Wireshark on the Windows 10 VM at your Azure.
   - Open Wireshark and set the filter to show only ICMP traffic.

3. **Ping Ubuntu VM**:
   - Find the private IP address of your Ubuntu VM from your Azure. (e.g. - `10.0.0.5`)
   - Open Powershell or Command Line on the Windows 10 VM.
   - Ping this Private IP address from the Windows 10 VM and watch the traffic in Wireshark. (e.g. - `ping 10.0.0.5`)

4. **Ping a Public Website**:
   - In the Windows 10 VM, use the command line or PowerShell to ping a public website (e.g., `www.google.com`).
   - Observe the traffic in Wireshark.

5. **Continuous Ping**:
   - Start a continuous ping from Windows 10 VM to Ubuntu VM. (e.g. - `ping 10.0.0.5 -t`)

6. **Manage Network Security**:
   - Go to the Network Security Group (NSG) in the network settings for your Ubuntu VM --- ***((You’ll see how NSG (Network Security Group) rules in Azure control traffic.))***
   - Add inbound traffic rule of ICMP and choose Deny
   - Change number of 310 to 290 --- ***((⛔ Now try pinging again from Windows VM — it should fail THEN Go back and re-enable ICMP (change action to Allow)))***
   - Check Wireshark and command line or Powershell in Windows 10 VM for traffic changes.
   - Re-enable ICMP traffic in the NSG and verify that ping starts working again.
   - Stop the ping activity. (e.g. of your keyboard keys - CMD + C)

***📖 Note: Observe in Wireshark how traffic stops and resumes depending on NSG rules.***

### SSH Traffic

1. **Filter for SSH Traffic**:
   - In Wireshark, set the filter to show only SSH traffic. (e.g. `tcp. port == 22`)

2. **SSH into Ubuntu VM**:
   - From Windows 10 VM, use the Windows Terminal or Powershell and connect to the Ubuntu VM via SSH using its private IP. (e.g. - `ssh username@Private IP Address`) 
   - Type commands in the SSH session and observe the traffic in Wireshark. Use this link for commands cheat sheet (for a study purpose) https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/

3. **Exit SSH Session**:
   - Type `exit` and press `enter` to close the SSH connection.

### DHCP Traffic

1. **Filter for DHCP Traffic**:
   - In Wireshark, filter to show only DHCP traffic.

2. **Renew IP Address**:
   - On Windows 10 VM, run `ipconfig /renew` to request a new IP address.
   - Observe the DHCP traffic in Wireshark.
   - Use this link for a cheat sheet (with explanation) commands https://www.ninjaone.com/blog/ipconfig-commands/
  
***📖 Note: The purpose of this DHCP is that it assigns IP addresses automatically, perferably forcing a new request if done correctly. You'll notice that DHCP Discover, Offer, Request, and Acknowledge as known messages. For more information about the ipconfig commands blog post above***

### DNS Traffic

1. **Filter for DNS Traffic**:
   - In Wireshark, filter to show only DNS traffic.

2. **Use nslookup**:
   - From the Windows 10 VM command line or Powershell, use `nslookup` to find IP addresses for `google.com` and `disney.com`.
   - Observe the DNS traffic in Wireshark.
   - Use this link to learn more about the commands and use it for learning purpose: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc725991(v=ws.11)
  
***📖 Note: DNS translates domain names (like google.com) into IP addresses.***

### RDP Traffic

1. **Filter for RDP Traffic**:
   - In Wireshark, set the filter to show RDP traffic (`tcp.port == 3389`).

2. **Observe RDP Traffic**:
   - Note the constant stream of traffic. This happens because RDP continuously transmits a live stream between computers.
  
***📖 Note: RDP is what you’re using to control the Windows VM.***
  
-----

## Conclusion

Now that you've learned how to use Wireshark to monitor various types of network traffic. 

You also should keep in mind that Azure is an example of Infrastructure as a Service (IaaS): You rent virtual machines and networks, including the benefits of cloud computing like scalability (which you can add/remove resources as needed), cost-efficiency (which you pay only for what you use), and accessibility (which you can access from anywhere). 

You even gained practical experience in observing ICMP, SSH, DHCP, DNS, and RDP traffic, which are an important skills for managing, troubleshooting, and undestanding the network environments.
