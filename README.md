<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Using Wireshark within Microsoft Azure Virtual Machines</h1>
<p>This tutorial walks you through setting up Windows and Ubuntu VMs in Azure and using Wireshark to observe various types of network traffic. You’ll learn how to create and configure virtual machines, monitor ICMP, SSH, DHCP, DNS, and RDP traffic, and understand the behavior of each traffic type.</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)
- Windows Powershell
- Various Network Protocols (ICMP, SSH, DHCP, DNS, TCP Port ===)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
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
  
-----

## Part 2: Observe Network Traffic

### ICMP Traffic

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
   - Go to the Network Security Group (NSG) in the network settings for your Ubuntu VM
   - Add inbound traffic rule of ICMP and choose Deny
   - Change number of 310 to 290
   - Check Wireshark and command line or Powershell in Windows 10 VM for traffic changes.
   - Re-enable ICMP traffic in the NSG and verify that ping starts working again.
   - Stop the ping activity. (e.g. of your keyboard keys - CMD + C)

### SSH Traffic

1. **Filter for SSH Traffic**:
   - In Wireshark, set the filter to show only SSH traffic.

2. **SSH into Ubuntu VM**:
   - From Windows 10 VM, connect to the Ubuntu VM via SSH using its private IP. (e.g. - `ssh username@Private IP Address`)
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

### DNS Traffic

1. **Filter for DNS Traffic**:
   - In Wireshark, filter to show only DNS traffic.

2. **Use nslookup**:
   - From the Windows 10 VM command line or Powershell, use `nslookup` to find IP addresses for `google.com` and `disney.com`.
   - Observe the DNS traffic in Wireshark.
   - Use this link to learn more about the commands and use it for learning purpose: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc725991(v=ws.11)

### RDP Traffic

1. **Filter for RDP Traffic**:
   - In Wireshark, set the filter to show RDP traffic (`tcp.port == 3389`).

2. **Observe RDP Traffic**:
   - Note the constant stream of traffic. This happens because RDP continuously transmits a live stream between computers.
  
-----

## Conclusion

Now that you've learned how to use Wireshark to monitor various types of network traffic. 

You have gained practical experience in observing ICMP, SSH, DHCP, DNS, and RDP traffic, which are an important skills for managing, troubleshooting, and undestanding the network environments.
