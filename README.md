<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines

In this tutorial, we explore various network traffic to and from Azure Virtual Machines using Wireshark. We'll also experiment with Network Security Groups. 

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

## Operating Systems Used

- Windows 10 (21H2)
- Ubuntu Server 20.04

## High-Level Steps

1. Create folders: "read-access", "write-access", "no-access", "accounting"
2. Set permissions for the "Domain-Users" group
3. Attempt to access file share as a normal User
4. Create an "accountants" folder for Security Group and configure permissions

## Actions and Observations

### Step 1: Creating Sample File Shares with Various Permissions

![image](https://github.com/CopaceticWill/azure-network-protocols/assets/137100082/d02e909f-6177-43b3-b681-f39d888dc912)

Here, we:

- Connect/log into DC-1 as your domain admin account (mydomain.com\user_admin)
- Connect/log into Client-1 as a normal user (mydomain\<someuser>)
- On DC-1, on the C:\ drive, create 4 folders: “read-access”, “write-access”, “no-access”, “accounting”
- Set the following permissions (share the folder) for the “Domain Users” group:
  - Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
  - Folder: “write-access”,  Group: “Domain Users”, Permissions: “Read/Write”
  - Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”

### Step 2: Creating an “ACCOUNTANTS” Security Group, Assign Permissions, and Test Access

![image](https://github.com/CopaceticWill/azure-network-protocols/assets/137100082/8a72cd75-1923-419a-8776-90525193f5ab)

Here, we:

- Return to DC-1, in Active Directory, and create a security group called “ACCOUNTANTS”
- On the “accounting” folder created earlier, set the following permissions:
  - Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”
- On Client-1, as <someuser>, try to access the accountant's folder. It fails.
- Log out of Client-1 as <someuser>
- On DC-1, make <someuser> a member of the “ACCOUNTANTS”  Security Group
- Sign back into Client-1 as <someuser> and try to access the “accounting” share in \\DC-1\ as <someuser>. Access is now granted.

