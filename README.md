<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Using Wireshark with Microsoft Azure Virtual Machines</h1>
<p>This tutorial walks you through setting up Windows and Ubuntu VMs in Azure and using Wireshark to observe various types of network traffic. You’ll learn how to create and configure virtual machines, monitor ICMP, SSH, DHCP, DNS, and RDP traffic, and understand the behavior of each traffic type.</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)
- Windows Powershell
- Various Network Protocols (ICMP, SSH, DNCP, DNS, TCP Port ===)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of these OS)***
- Ubuntu Server 22.04 (Virtual Machine)

-----

## Part 1: Create and Configure Resources

1. **Create a Resource Group**:
   - Go to your Azure portal and create a new Resource Group.

2. **Create a Windows 10 Virtual Machine (VM)**:
   - Select the Resource Group you created.
   - Allow the creation of a new Virtual Network (VNet) and Subnet during VM setup.

3. **Create a Linux (Ubuntu) VM**:
   - Choose the same Resource Group and VNet as the Windows 10 VM.

4. **Check Your Virtual Network**:
   - Use Network Watcher to observe your Virtual Network.
  
-----

## Part 2: Observe Network Traffic

### ICMP Traffic

1. **Connect to Windows 10 VM**:
   - Use Remote Desktop to access your Windows 10 VM.

2. **Install and Use Wireshark**:
   - Install Wireshark on the Windows 10 VM.
   - Open Wireshark and set the filter to show only ICMP traffic.

3. **Ping Ubuntu VM**:
   - Find the private IP address of your Ubuntu VM.
   - Ping this IP address from the Windows 10 VM and watch the traffic in Wireshark.

4. **Ping a Public Website**:
   - In the Windows 10 VM, use the command line or PowerShell to ping a public website (e.g., www.google.com).
   - Observe the traffic in Wireshark.

5. **Continuous Ping**:
   - Start a continuous ping from Windows 10 VM to Ubuntu VM.

6. **Manage Network Security**:
   - Go to the Network Security Group (NSG) for your Ubuntu VM and disable inbound ICMP traffic.
   - Check Wireshark and command line in Windows 10 VM for traffic changes.
   - Re-enable ICMP traffic in the NSG and verify that ping starts working again.
   - Stop the ping activity.

### SSH Traffic

1. **Filter for SSH Traffic**:
   - In Wireshark, set the filter to show only SSH traffic.

2. **SSH into Ubuntu VM**:
   - From Windows 10 VM, connect to the Ubuntu VM via SSH using its private IP.
   - Type commands in the SSH session and observe the traffic in Wireshark.

3. **Exit SSH Session**:
   - Type `exit` and press Enter to close the SSH connection.

### DHCP Traffic

1. **Filter for DHCP Traffic**:
   - In Wireshark, filter to show only DHCP traffic.

2. **Renew IP Address**:
   - On Windows 10 VM, run `ipconfig /renew` to request a new IP address.
   - Observe the DHCP traffic in Wireshark.

### DNS Traffic

1. **Filter for DNS Traffic**:
   - In Wireshark, filter to show only DNS traffic.

2. **Use nslookup**:
   - From the Windows 10 VM command line, use `nslookup` to find IP addresses for `google.com` and `disney.com`.
   - Observe the DNS traffic in Wireshark.

### RDP Traffic

1. **Filter for RDP Traffic**:
   - In Wireshark, set the filter to show RDP traffic (tcp.port == 3389).

2. **Observe RDP Traffic**:
   - Note the constant stream of traffic. This happens because RDP continuously transmits a live stream between computers.
  
-----

## Conclusion

You've now set up a Windows 10 and Ubuntu VM in Azure, and learned how to use Wireshark to monitor various types of network traffic. By following this tutorial, you gained practical experience in observing ICMP, SSH, DHCP, DNS, and RDP traffic, which are essential skills for managing and troubleshooting network environments.

Understanding these traffic types helps in diagnosing network issues, securing communications, and ensuring efficient network performance.
