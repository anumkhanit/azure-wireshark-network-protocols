<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>🔎 Analyzing Network Traffic in Azure with Wireshark</h1>
<p>Learn how to monitor and understand real-time network traffic in a cloud environment using Azure virtual machines and Wireshark.</p>

<h1>🧠 Overview</h1>
   
   <p>In this lab, you’ll create two Azure virtual machines—one Windows 10 and one Ubuntu Linux—within the same virtual network. You’ll use Wireshark on the Windows VM to analyze protocols such as ICMP, SSH, DNS, DHCP, and RDP. Along the way, you’ll also adjust Network Security Group (NSG) rules to observe how traffic is filtered in Azure.

This guide is beginner-friendly and great for students exploring network engineering, cloud computing, or cybersecurity fundamentals.
</p>

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

<h1>☁️ Part 1: Set Up Azure Resources</h1>

<h2>📦 Step 1: Create a Resource Group & Virtual Network</h2>

1.	Log into Azure Portal
2.	Create a Resource Group
    - Example: `NetworkLabRG`
3.	Create a Virtual Network (VNet)
    - Name: `NetworkLabVNet`
	 - Address space: `10.0.0.0/16` (default)

<h2>🖥️ Step 2: Create Virtual Machines</h2>

- Windows 10 VM
    - Use the resource group and VNet created above
	 - Allow `RDP (port 3389)` during setup
	 - Choose a small size (e.g. B2s)
	 - Note the private IP address

- Ubuntu Linux VM
   - Same Resource Group and VNet as Windows VM
	- Allow `SSH (port 22)`
	- Choose username + password
	- Note the private IP address

***📖 Note: A Resource Group organizes related Azure services, and a VNet allows your VMs to communicate like they’re on the same local network. You’ll use one VM (Windows) to generate and monitor traffic, and the other (Linux) to respond to it.***
  
-----

<h1>🔍 Part 2: Monitor Network Traffic with Wireshark</h1>

***Wireshark is a free and powerful tool used to analyze packets and protocols on a network. It helps you see what’s happening behind the scenes.***

- 💻 Install Wireshark

	- On the Windows 10 VM, connect via Remote Desktop
	- Download Wireshark: `https://www.wireshark.org`
	- Launch Wireshark and start capturing on the Ethernet adapter

-----

### ICMP Traffic (Ping)

***📖 Note: ICMP is the protocol used when you “ping” another device.***

- Filter ICMP in Wireshark
   - Open Wireshark and set the filter to show only `icmp` traffic on the search bar.

- Ping Ubuntu VM
   - Find the private IP address of your Ubuntu VM from your Azure. (e.g. - `10.0.0.5`)
   - Open Powershell or Command Line on the Windows 10 VM.
   - Ping this Private IP address from the Windows 10 VM and watch the traffic in Wireshark. (e.g. - `ping 10.0.0.5`)

- Ping a Public Website
   - In the Windows 10 VM, use the command line or PowerShell to ping a public website (e.g., `www.google.com`).
   - Observe the traffic in Wireshark.

- Continuous Ping
   - Start a continuous ping from Windows 10 VM to Ubuntu VM. (e.g. - `ping 10.0.0.5 -t`)
 
- 🛑 Stop the ping with `Ctrl + C`.
  
-----

## 🔐 Manage ICMP with NSG

- Go to the Ubuntu VM > Networking > NSG

- Add a new Inbound Rule:
  - Protocol: `ICMP`
  - Action:`Deny`
  - Priority: `290`
 
- Return to the Windows VM and try pinging again. It should fail.

- Change the NSG rule to Allow ICMP and try again. It should succeed.

- 🛑 Stop the ping with `Ctrl + C`.

***📖 Note: Observe in Wireshark how traffic stops and resumes depending on NSG rules.***

-----

### 🧰 SSH Traffic

- Filter for SSH in Wireshark

   - In Wireshark, set the filter to show only SSH traffic. (e.g. `tcp. port == 22`) on the search bar.

- SSH into Ubuntu VM

   - From Windows 10 VM, use the Windows Terminal or Powershell and connect to the Ubuntu VM via SSH using its private IP. (e.g. - `ssh username@Private IP Address`) 
   - Type commands in the SSH session and observe the traffic in Wireshark. Use this link for commands cheat sheet (for a study purpose) https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/

- Exit SSH Session

   - Type `exit` and press `enter` to close the SSH connection.

-----

### 📶 DHCP Traffic

- Filter for DHCP Traffic

   - In Wireshark, filter to show only DHCP traffic on the search bar

- Renew IP Address

   - On Windows 10 VM, run `ipconfig /renew` to request a new IP address.
   - Observe the DHCP traffic in Wireshark.
   - Use this link for a cheat sheet (with explanation) commands https://www.ninjaone.com/blog/ipconfig-commands/
  
***📖 Note: The purpose of this DHCP is that it assigns IP addresses automatically, perferably forcing a new request if done correctly. You'll notice that DHCP Discover, Offer, Request, and Acknowledge as known messages. For more information about the ipconfig commands blog post above***

-----

### 🌍 DNS Traffic

- Filter for DNS Traffic

   - In Wireshark, filter to show only DNS traffic `dns` on the search bar.

- Use nslookup

   - From the Windows 10 VM command line or Powershell, use `nslookup` to find IP addresses for `google.com` and `disney.com`.
   - Observe the DNS traffic in Wireshark.
   - Use this link to learn more about the commands and use it for learning purpose: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc725991(v=ws.11)
  
***📖 Note: DNS translates domain names (like google.com) into IP addresses.***

-----

### 🖥️ RDP Traffic

***📖 Note: Since you are already connected via Windows Remote Desktop, you’ll observe continuous traffic on `port 3389`.***

- Filter for RDP Traffic

   - In Wireshark, set the filter to show RDP traffic (`tcp.port == 3389`) on the search bar.

- Observe RDP Traffic

   - Note the constant stream of traffic. This happens because RDP continuously transmits a live stream between computers.
  
-----

<h1>📘 What Is Cloud Computing?</h1>

<p>Cloud computing allows users to access computing resources (like servers, storage, and apps) over the internet rather than managing physical infrastructure.</p>

<p>Azure is a leading cloud platform that supports:</p>

- IaaS (Infrastructure as a Service) → VMs, Networks, NSGs
- Scalability & Flexibility → Use what you need, when you need it
- Security & Monitoring → Protect and observe your data flows

-----

## 🧠 Summary of Wireshark Filters

- Protocol | Use | Wireshark Filter

  - ICMP | Ping test / connectivity | `icmp`
  - SSH | Secure shell login | `tcp.port == 22`
  - DHCP | IP address leasing | `bootp`
  - DNS | Domain name resolution | `dns`
  - RDP | Remote Desktop traffic | `tcp.port == 3389`

- ✅ Wrap Up

  - ✅ You set up a cloud lab using Azure
  - ✅ Monitored real traffic with Wireshark
  - ✅ Learned how protocols behave in real-time
  - ✅ Explored firewall rules using Network Security Groups (NSGs)

- 🧹 When You’re Finished

  - To avoid charges:

    - Return to Azure Portal
	 - Stop or delete both VMs and the Resource Group

-----

### 🧠 Want to Learn More?

- [Azure Networking Documentation](https://learn.microsoft.com/en-us/azure/networking/)
  
- [Wireshark Documentation](https://www.wireshark.org/docs/wsug_html_chunked/)

- [Microsoft Learn - DNS Basics](https://learn.microsoft.com/en-us/windows-server/networking/dns/dns-overview)
