<p align="center">
<img src="(https://i.imgur.com/Ua7udoS.png) alt="Traffic Examination"/>
</p>

<h1>Observing Network Traffic Using Wireshark through Virtual Machines</h1>
In this tutorial, we'll explore how to observe various types of network traffic to and from Azure Virtual Machines using Wireshark. We'll also experiment with Network Security Groups to understand their impact on network traffic.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)
- Windows Powershell
- Various Network Protocols (ICMP, SSH, DNCP, DNS, TCP Port ===)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- macOS M1
- Windows 10 Pro (Virtual Machine)
- Ubuntu Server 22.04 (Virtual Machine)

-----

<h1>Part 1: Set Up Resources at Microsoft Azure</h1>

Before we begin, let's set up our Azure environment:

- Create a Resource Group.
- Create a **Windows 10 Virtual Machine (VM1)** (with RDP Port) and assign it to the Resource Group you've created. Allow the VM1 to create a new Virtual Network (Vnet) and Subnet during setup.
- Create a VM2 **Ubuntu 22.04 VM** (with SSH Port) and assign it to the same Resource Group. Allow the VM2 to create another Vnet and Subnet during the setup.

</br>

-----

<h1>Part 2: Observing Traffic Types</h1>

Now that our Azure environment is set up, let's observe different types of network traffic using Wireshark:

<h2>2.1 Observe ICMP Traffic</h2>

ICMP (Internet Control Message Protocol) is used for diagnostic and control purposes. To observe ICMP traffic:

- Use **Microsoft RD Client** to connect to your **Windows 10 Virtual Machine (VM1)** Public IP Address.
- Install **Wireshark** within your **Windows 10 VM1**.
- Open **Wireshark** and apply a filter for **ICMP** traffic.
- Retrieve the **private IP address** of the **Ubuntu VM2** and ping it from within the **Windows 10 VM1** in the **Windows Powershell**.
    - Ex = ping 10.0.0.5
- Observe ping requests and replies in **Wireshark**.
- Ping a public website from the **Windows 10 VM1** and monitor the traffic in **Wireshark**.
    - Ex = ping www.google.com
- Start a continuous ping from your **Windows 10 V1M** to your **Ubuntu VM2**.
    - Ex = ping 10.0.0.5 or www.google.com -t
- Disable incoming (inbound) **ICMP** traffic in the **Network Security Group** of your **Ubuntu VM2** at **Microsoft Azure**. This allows you to block the connection through the virtual internet server since you've created a firewall in **Ubuntu VM2**
    - Ex = Deny **ICMP** with custom name: DENY_ICMP_PING_FROM_ANYWHERE
- Observe the **ICMP** traffic in **Wireshark** and the command line ping activity on the **Windows 10 VM1**.
- Re-enable **ICMP** traffic in the **Network Security Group** of your **Ubuntu VM2**. What this means is that you've disable the firewall and you allow the virtual interenet connection for two VMs to communicate.
    - Ex = Allow **ICMP** with the same custom name you've created previously.
- Observe the **ICMP** traffic in **Wireshark** and verify that the ping starts working.
- Stop the ping activity.
    - Use keys [Cntl^ + C]

<h2>2.2 Observe SSH Traffic</h2>

SSH (Secure Shell) is a protocol used for secure remote access. To observe SSH traffic:

- Filter for **SSH** traffic in **Wireshark**.
- **SSH** into your **Ubuntu VM2** private IP Address through your **Windows 10 VM1**.
- Observe **SSH** traffic in **Wireshark** while typing commands.
    - Ex = ssh (username)@(VM2 private address) - note: it'll ask for password, which will not show you what you type.
- Exit the **SSH** connection by typing 'exit' and pressing [Enter].

<h2>2.3 Observe DHCP Traffic</h2>

DHCP (Dynamic Host Configuration Protocol) is used for automatically assigning IP addresses. To observe DHCP traffic:

- Filter for **DHCP** traffic in **Wireshark**.
- From your **Windows 10 VM1**, attempt to renew your VM's IP address using the command [ipconfig /renew].
- Observe the **DHCP** traffic appearing in **Wireshark**.

<h2>2.4 Observe DNS Traffic</h2>

DNS (Domain Name System) is used for translating domain names to IP addresses. To observe DNS traffic:

- Filter for **DNS** traffic in **Wireshark**.
- From your **Windows 10 VM1's** command line, use [nslookup] to query the IP addresses of 'google.com' and 'disney.com'.
- Observe the **DNS** traffic in **Wireshark**.

<h2>2.5 Observe RDP Traffic</h2>

RDP (Remote Desktop Protocol) is used for remote desktop connections. To observe RDP traffic:

- Filter for **RDP** traffic in **Wireshark** [tcp.port == 3389].
- Observe the constant stream of traffic.

</br>

-----
<h1>Conclusion</h1>
In conclusion, we've gained hands-on experience in observing various network protocols using Wireshark. We've learned how to use Wireshark to monitor ICMP, SSH, DHCP, DNS, and RDP traffic, and how Network Security Groups can impact network traffic. Monitoring network traffic is a critical skill for IT professionals, and Wireshark is a valuable tool for gaining insights into network communication.
