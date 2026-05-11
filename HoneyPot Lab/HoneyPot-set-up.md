## Part 2 — Honeypot Virtual Machine

1. Create a Windows 10 VM

In the Azure portal, search for Virtual Machines and click Create
Choose Windows 10 as the image
Pick an appropriate VM size (e.g. Standard_B2s)
Set a username and password, save these, you'll need them to RDP in
Leave all other defaults.

2. Create a Resource Group and Virtual Network

Create a new Resource Group, RG-SOC-LAB
I created a new virtual network for the resource group, Vent-soc-lab

3. Create a Virtual machine 
I created a vm named PAYROLL-DB-EAST-1 in the WEST US 2 region

4. Configure the Network Security Group (NSG)
After the VM is created:
Navigate to the VM → Networking → Network Security Group
Delete the rdp 300 inbound rule
Add a new inbound rule:

Source: Any
Source port ranges: *
Destination: Any
Destination port ranges: *
Protocol: Any
Action: Allow
Priority: 100
Name: DANGER_AllowAnyCustomInbound

This makes the VM fully exposed to the internet, intentional for the honeypot.

5. Disable the Windows Firewall on the VM

RDP into your VM using the public IP address
Open Run → type wf.msc → press Enter
Click Windows Defender Firewall Properties
On the Domain Profile, Private Profile, and Public Profile tabs, set the Firewall state to Off
Click OK

5. Confirm connectivity
From your local machine, ping the VM's public IP:
ping 48.202.57.123
You should get a response. The honeypot is now live and exposed to the internet.
