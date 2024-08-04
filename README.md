<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create 2 Virtual Machines with one running Windows 10 and the second running Linux
- Download Wireshark
- Observe Various Protocol Traffic:
  - ICMP
  - SSH
  - DHCP
  - DNS
  - RDP

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/user-attachments/assets/c8c3186c-be22-4fde-959f-fa0ec48fd42a"/>
</p>
<p>
First we will create two virtual machines, with one running Windows 10 and the other running Linux.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d5753164-c21f-4636-a722-db8a9bb47826"/>
</p>
<p>
Open the virtual machine that is running Windows 10. Download and install Wireshark.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/8244b399-883d-40ee-807f-9c4f0382952e"/> <img src="https://github.com/user-attachments/assets/7caaf989-e5fb-4a21-9a38-834800ddbdda"/>
</p>
<p>
Once Wireshark has been downloaded, we will open it, select "Ethernet", and start capturing packets. We can then filter for ICMP traffic only.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/0b9fe7cf-019a-40f5-bf26-9ee9b2ffaef4"/> <img src="https://github.com/user-attachments/assets/54417319-6a7e-4778-b82d-45ca282d4e88"/>
</p>
<p>
In the Azure Portal, we will need to find and make note of the Private IP address for 'VM2'. Back in VM1, open Powershell and ping the private IP address of VM2 (ex: ping 10.0.0.5). We can observe in Wireshark how VM1 sends ping requests to VM2 and received replies quickly after using ICMP protocol.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/edfeff80-2c18-4fc7-89f7-d2efa2485e24"/> <img src="https://github.com/user-attachments/assets/642f5ff4-58a3-4c2f-b1ae-46d73f89f8a2"/>
</p>
<p>
We can then observe what happens when ICMP traffic is blocked. In Azure, open Network Security Groups and select VM2. Settings>Inbound security rules and add a new rule that denies all ICMP traffic. Back in VM1, using the command 'ping 10.0.0.5 -t' we can note that the request continues to be timed out. This is due to all ICMP traffic being blocked for VM2.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/b57694eb-6b4c-4dd2-97f8-05581a45bbd2"/> <img src="https://github.com/user-attachments/assets/53cb0497-8192-449a-abd1-629563b3baaf"/>
</p>
<p>
We can now observe some SSH traffic. By first filtering SSH traffic in Wireshark, we can then open Powershell and "SSH into" VM2 via the private IP address. Utilizing your username and password, we will first type 'ssh username@IP' (ex: ssh labuser@10.0.0.5). You will be prompted to continue to which you will type 'yes' and the password you created VM2 with. We can further observe SSH traffic by typing some Linux commands, observing the traffic in Wireshark. Once completed, we can close the connection by typing 'exit'.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/9b94198c-981e-48d8-9f9d-e6f08522dfe4"/>
</p>
<p>
To observe DHCP traffic, we can begin by filtering DHCP traffic in Wireshark. In Powershell, we can attempt to issue a new IP address using 'ipconfig /renew' where we can then observe the DHCP traffic.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/ead3c132-e8ce-44f4-bd8b-7a476ee73ab8"/>
</p>
<p>
To observe DNS traffic, we can start with filtering DNS traffic in Wireshark. In Powershell, we can then use nslookup what google.com IP address is and observe the traffic in Wireshark.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/4aa3340a-f9f4-4d64-8eb2-f92c8fb77708"/>
</p>
<p>
Lastly, we will observe RDP traffic. Beginning with filtering RDP traffic in Wireshark using tcp.port == 3389, we can then observe the non-stop traffic due to the use of Remote Desktop to live stream one computer to another.
</p>
<br />
