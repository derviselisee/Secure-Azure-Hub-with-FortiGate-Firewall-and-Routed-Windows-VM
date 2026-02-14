## Secure FortiGate Firewall Deployment with Azure Bastion and Protected Windows Client Subnet

This project demonstrates how I deployed a FortiGate Next-Gen Firewall in Microsoft Azure, configured the network from the ground up, and integrated a Windows 11 client inside a protected subnet. 
The goal was to route all traffic from the Windows VM through the FortiGate, monitor activity through FortiView, and securely access internal resources using Azure Bastion.

<img width="1915" height="913" alt="Azure Portal" src="https://github.com/user-attachments/assets/164ca837-3a68-4e61-95b4-106a4e07d55f" />


## Project Overview

The lab simulates a small enterprise cloud environment built on Azure. It includes:

-A FortiGate VM acting as the main security gateway

-A protected subnet hosting a Windows 11 client

-Azure Bastion to securely access the Windows VM without exposing RDP to the internet

-User-Defined Routes (UDRs) that ensure all protected subnet traffic passes through the FortiGate

-Firewall policies and static routes configured on FortiGate

-Traffic monitoring using FortiView and Log & Report

This environment mirrors real-world cloud security architecture where firewalls, segmentation, and secure remote access are essential.

## What I Implemented

## 1. Deployed a FortiGate VM

I began the project by deploying a FortiGate Next-Generation firewall directly from the Azure Marketplace. 
During setup I created the admin credentials, selected the correct subnets, and mapped port1 as the WAN interface and port2 as the LAN interface. 
I also assigned a public IP to allow initial management access. Once the deployment completed, I connected to the FortiGate GUI through its public IP and verified from the dashboard and
interface page that both network interfaces were active and correctly attached to their respective Azure subnets.

Key configuration steps during FortiGate deployment:

• Created the admin username and password
• Mapped port1 as WAN and port2 as LAN
• Selected the required subnets and applied NSG rules
• Assigned a public IP for secure management access

<img width="1585" height="912" alt="fgt" src="https://github.com/user-attachments/assets/c15b020d-fd7e-487d-aca7-f33997e9fa13" />
<img width="1429" height="907" alt="pART 1" src="https://github.com/user-attachments/assets/53999adc-4f83-4a10-8596-9c570b32f1c1" />
<img width="934" height="915" alt="PART 2" src="https://github.com/user-attachments/assets/be311855-54a4-4bea-a1e2-31409ad0d1dc" />
<img width="1917" height="872" alt="FGT network configs" src="https://github.com/user-attachments/assets/372547eb-bc48-4216-80b0-0b00f87c5290" />
<img width="1918" height="915" alt="FGT public ip" src="https://github.com/user-attachments/assets/dae964ad-8621-4780-b453-7e5372dc62e8" />
<img width="1619" height="870" alt="deployment in progres" src="https://github.com/user-attachments/assets/903232af-6ba6-4da6-922f-20b747b160eb" />
<img width="1699" height="916" alt="FORTIGATE IS DEPLOYED" src="https://github.com/user-attachments/assets/0564467e-d7ca-4001-ab9a-cc8bde6ef486" />

After the deployment finished, I signed in to the FortiGate GUI using its public IP address to confirm that the firewall was running correctly.
I reviewed the dashboard and verified system status, licensing, and overall health. 
I also checked the network interfaces and confirmed that both port1 and port2 were active, reachable, and mapped to the correct Azure subnets.

<img width="1918" height="913" alt="dashboard" src="https://github.com/user-attachments/assets/1f83f071-5000-4732-9ba8-98d88500482e" />
<img width="1918" height="915" alt="Fortigate interfaces" src="https://github.com/user-attachments/assets/c948e3d5-f7bf-456c-a41c-5dd40d3cd945" />


## 2. Created the Protected Subnet and Windows 11 VM

I deployed a Windows 11 Pro virtual machine and placed it inside a dedicated protected subnet using the range 172.16.1.0/24.
This subnet cannot reach the internet directly because all outbound traffic is intentionally forced through the FortiGate firewall, which acts as the security gateway for the environment. 
To make this work, I associated a custom route table with the protected subnet and configured it to send all outbound traffic to the FortiGate internal interface at 172.16.0.68.
This setup ensures that every packet leaving the Windows VM passes through the firewall where it can be inspected, logged, and controlled.

Why the protected subnet uses a custom route table:

• It forces the Windows VM to route all traffic through the FortiGate.
• It prevents the VM from accessing the internet directly through Azure.
• It allows the firewall to apply NAT, policies, and full traffic inspection.

<img width="1405" height="909" alt="Windows vm creation" src="https://github.com/user-attachments/assets/4d76431d-f522-4ba9-97b3-30898dc7576d" />
<img width="1467" height="906" alt="Windows client network config" src="https://github.com/user-attachments/assets/726522aa-bc17-4df8-a27b-0e3bc5d32e76" />
<img width="1409" height="876" alt="Windows VM created" src="https://github.com/user-attachments/assets/9b63c53f-4e94-4120-a900-cd9e1c45ef8e" />
<img width="1914" height="875" alt="Windows client is deployed" src="https://github.com/user-attachments/assets/bceb7b02-27a4-4d93-9941-f8ff4086f89c" />


## 3. Configured Azure Bastion

Azure Bastion played an important role in this project because it allowed me to access my Windows VM securely without exposing a public RDP port. 
Instead of opening port 3389 to the internet, which is one of the most common attack vectors against cloud environments, Bastion provides a fully browser-based remote session that stays inside Azure’s internal network.
This keeps the VM isolated, reduces the attack surface, and follows best practices for secure cloud administration.

Key advantages of using Azure Bastion:

• Browser-based RDP access directly from the Azure portal
• No public IP or open RDP port on the VM
• Built-in secure jump-host functionality for private subnets

This is an important security practice because exposing RDP publicly is a major attack vector.

<img width="1419" height="770" alt="I am deploying Bastion to access my windows securely" src="https://github.com/user-attachments/assets/8f403ed6-bb21-4946-9e82-e12d65361d66" />
<img width="2878" height="1446" alt="AZURE BASTION" src="https://github.com/user-attachments/assets/3af5d459-2858-44a2-be4c-3ba4435c3a15" />
<img width="1347" height="916" alt="Bastion successfully created" src="https://github.com/user-attachments/assets/8b8333d6-a06a-4cb3-aef3-c8d616bf31f5" />
<img width="1918" height="913" alt="I am accessing my windows through Bastion" src="https://github.com/user-attachments/assets/876dfca9-c358-4ee5-a1c2-64f600bee68a" />
<img width="2880" height="1464" alt="win client in Bastion" src="https://github.com/user-attachments/assets/52993530-4dff-49d7-89e8-5b426e8c05c2" />



## 4. Implemented User-Defined Routes (UDR)

I created a custom route table and applied it to the protected subnet:

0.0.0.0/0 → Next hop: Virtual Appliance → 172.16.0.68 (FortiGate port2)

Bastion Subnet (172.16.0.128/26) → Next hop: 172.16.0.1 (Azure default gateway)

Why this matters:

Forces Windows VM outbound traffic through FortiGate

Allows Bastion traffic to function correctly

Enables FortiGate to perform NAT and security inspection

<img width="1915" height="875" alt="configure route so that windows can use fortigate as gateway" src="https://github.com/user-attachments/assets/3d814eef-d4a4-4dd1-9760-271abd27a657" />

## 5. Configured FortiGate Firewall Policy and Static Routes

On the FortiGate I created:

-Firewall Policy

Incoming: port2 (LAN)

Outgoing: port1 (WAN)

Action: Accept

NAT: Enabled
This allowed the Windows client to access the internet.
<img width="1910" height="914" alt="Firewall Policies" src="https://github.com/user-attachments/assets/42be2d07-8ddf-4f2f-b710-06d8e9c437c9" />

-Static Route for Bastion

To ensure Bastion could reach the Windows VM through FortiGate, I added:
Destination: 172.16.0.128/26
Gateway: 172.16.0.1
Interface: port1
Comment: Bastion
<img width="1915" height="915" alt="Fortigate static routes" src="https://github.com/user-attachments/assets/6b829fb4-eec0-4090-b385-e6066f4ef2dc" />

## Testing and Validation

After completing the configuration:

✔ Windows VM Successfully Pinged FortiGate LAN

The Windows client at 172.16.1.4 could ping:

FortiGate port2: 172.16.0.68

FortiGate WAN: 172.16.0.4

✔ Windows VM Browsed the Internet

I opened a browser inside the Windows VM and confirmed that outbound traffic was NATed through the FortiGate.

✔ Verified Traffic in FortiView

Inside FortiGate GUI:

FortiView showed the Windows client traffic

I could see applications, destinations, and bandwidth

Logs confirmed NAT and security inspection were working properly

<img width="1910" height="912" alt="Windows client can communicate with my fortigate" src="https://github.com/user-attachments/assets/07b4978d-8945-41d6-a11d-17a8b7596822" />
<img width="1918" height="912" alt="we can see that oiuyr client use fortigate as gateway" src="https://github.com/user-attachments/assets/e56b74fb-a816-4147-9b2b-dcc2c6e95b15" />
<img width="1916" height="911" alt="Win 11 internet browsing" src="https://github.com/user-attachments/assets/45ff7f31-075f-47e1-9a48-20ffee05aa60" />
<img width="1917" height="911" alt="FortiView sources" src="https://github.com/user-attachments/assets/54e1c004-3962-4e40-a442-4bfd22baaf2d" />
<img width="1919" height="914" alt="Traffic Logs" src="https://github.com/user-attachments/assets/b59068b5-dd41-49e3-b353-0bacd73683e1" />


## SUMMARY 
This project walks through deploying a fully functional FortiGate firewall in Azure and attaching a Windows 11 client behind it. 
I created the FortiGate VM, verified the interfaces, and then deployed a Windows machine in a protected subnet to force all its traffic through the firewall. 
After configuring Azure Bastion for secure access, adding the required route table, and creating the correct firewall policies and static routes, the Windows client was able to reach the internet through FortiGate.
All traffic appeared in FortiView and the logs, confirming that the setup worked exactly like a real enterprise cloud security environment.

## Skills Gained

-Azure networking and subnet segmentation

-Deployment and configuration of FortiGate in cloud

-Creating secure management access with Azure Bastion

-Implementing User-Defined Routes (UDR)

-Firewall policy design and NAT

-Traffic inspection and logging in FortiGate

-Cloud security architecture fundamentals


## Final Result

By the end of this project I created a functional cloud security environment where:

The Windows VM securely connects through Azure Bastion

All outbound traffic goes through the FortiGate firewall

Traffic is logged and inspected

FortiGate enforces security policies and NAT

The architecture mirrors a real enterprise cloud edge setup


## What I Learned Today

Working on this project taught me a lot about how Azure networking, routing, and firewall integration actually work in real cloud environments.
I learned how to deploy a FortiGate VM, map its interfaces to different Azure subnets, and use Azure Bastion for secure remote access without exposing any public RDP ports.
I also gained a deeper understanding of user-defined routes and how Azure forces traffic through a virtual appliance even when the VM still shows the Azure gateway. At the same time, 
I learned how to troubleshoot routing loops, fix Bastion return paths, and verify traffic flow through FortiView and firewall logs.
Overall, today really helped me understand the building blocks of cloud security architecture and how on-prem firewall concepts translate into Azur














