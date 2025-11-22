## Secure FortiGate Firewall Deployment with Azure Bastion and Protected Windows Client Subnet

This project demonstrates how I deployed a FortiGate Next-Gen Firewall in Microsoft Azure, configured the network from the ground up, and integrated a Windows 11 client inside a protected subnet. 
The goal was to route all traffic from the Windows VM through the FortiGate, monitor activity through FortiView, and securely access internal resources using Azure Bastion.


## Project Overview

The lab simulates a small enterprise cloud environment built on Azure. It includes:

A FortiGate VM acting as the main security gateway

A protected subnet hosting a Windows 11 client

Azure Bastion to securely access the Windows VM without exposing RDP to the internet

User-Defined Routes (UDRs) that ensure all protected subnet traffic passes through the FortiGate

Firewall policies and static routes configured on FortiGate

Traffic monitoring using FortiView and Log & Report

This environment mirrors real-world cloud security architecture where firewalls, segmentation, and secure remote access are essential.

## What I Implemented

1. Deployed a FortiGate VM

I started by creating a FortiGate Next-Gen firewall in Azure using the marketplace image.

During deployment I configured:

-Admin username and password

-Networking (port1 as WAN, port2 as LAN)

-Required subnets and NSGs

-Public IP for management access

After deployment I accessed the FortiGate GUI through its public IP and verified that both interfaces were active.
<img width="1585" height="912" alt="fgt" src="https://github.com/user-attachments/assets/c15b020d-fd7e-487d-aca7-f33997e9fa13" />
<img width="1429" height="907" alt="pART 1" src="https://github.com/user-attachments/assets/53999adc-4f83-4a10-8596-9c570b32f1c1" />
<img width="934" height="915" alt="PART 2" src="https://github.com/user-attachments/assets/be311855-54a4-4bea-a1e2-31409ad0d1dc" />
<img width="1917" height="872" alt="FGT network configs" src="https://github.com/user-attachments/assets/372547eb-bc48-4216-80b0-0b00f87c5290" />
<img width="1918" height="915" alt="FGT public ip" src="https://github.com/user-attachments/assets/dae964ad-8621-4780-b453-7e5372dc62e8" />
<img width="1619" height="870" alt="deployment in progres" src="https://github.com/user-attachments/assets/903232af-6ba6-4da6-922f-20b747b160eb" />
<img width="1699" height="916" alt="FORTIGATE IS DEPLOYED" src="https://github.com/user-attachments/assets/0564467e-d7ca-4001-ab9a-cc8bde6ef486" />





