# Home Lab Running Active Directory & PowerShell User Automation

<!-- This repository outlines the configuration of a basic windows networking environment running Active Directory within a private virtual ecosystem. The lab emulates a mini corporate network, routing client traffic safely to the internet through a designated domain controller while demonstrating automated enterprise provisioning. -->

## 🛠️ Project Architecture & Components

* **Hypervisor:** Oracle VirtualBox
* **Domain Controller (DC):** Windows Server 2019 (Standard with Desktop Experience)
* **Client Machine:** Windows 10 Pro
* **Automation:** PowerShell Script for bulk Active Directory Provisioning

---
<!--
## 🌐 Network Configuration

The environment splits traffic across two network adapters on the Domain Controller to safely isolate the internal directory infrastructure while providing web access to clients. 

| Network Adapter | Configuration Type | IP Addressing | Purpose |
| :--- | :--- | :--- | :--- |
| **Adapter 1 (DC)** | NAT | DHCP (Automatic from Local Network) | Direct outside Internet connection |
| **Adapter 2 (DC)** | Internal Network | `172.16.0.1` (Subnet: `255.255.255.0`) | Private Virtual Gateway / DNS server |
| **Client 1** | Internal Network | DHCP Leased (`172.16.0.100` - `172.16.0.200`) | Enterprise workstation traffic emulation |

### Network Services Implemented:
* **Active Directory Domain Services (AD DS):** Deployed a new forest (`mydomain.com`).
* **Routing and Remote Access (RAS / NAT):** Configured network address translation to route client traffic out from the internal network safely through the DC's public interface.
* **DHCP Server:** Set up a dynamic scope range (`172.16.0.100` to `172.16.0.200`) explicitly passing `172.16.0.1` as both the Router (Default Gateway) and primary DNS server.

---

## ⚡ Automated User Provisioning (PowerShell)

Instead of manually generating directory entries, a custom PowerShell script was executed within an unrestricted execution policy environment to bulk-create 1,000 randomized user profiles under a dedicated organizational unit (`_users`).

### Key Script Actions:
1. Generates an Organizational Unit (`_users`) configured with `ProtectedFromAccidentalDeletion = $false`.
2. References a `names.txt` file containing a target string of randomized names.
3. Splits names iteratively into lowercase structural strings to programmatically map `First Initial + Last Name` formats (e.g., `jmatakor`).
4. Generates an Active Directory Account object assigning secure password parameters (`Password1`), auto-enabling profiles, and bypassing initial standard expirations.

---

## 🚀 Deployment & Verification Steps

1. **Virtual Machines Setup:** Installed Windows Server 2019 and Windows 10 Pro on VirtualBox, modifying processors and injecting Bi-directional clipboard integration via VirtualBox Guest Additions.
2. **Promote Domain Controller:** Configured AD DS, named the instance `DC`, configured dedicated admin structures, and stood up the DNS mapping footprint.
3. **Configure NAT & DHCP:** Authorized the local server routing components and activated the dynamic client pool leases.
4. **Execute Automation:** Ran the user provisioning loop script through the PowerShell Integrated Scripting Environment (ISE) as an administrator.
5. **Domain Join:** Altered the Windows 10 desktop computer name to `Client1`, targeted the local directory network gateway, joined the workstation to `mydomain.com`, and verified global sign-on profiles via newly script-generated credentials.

---
*Project inspired by the hands-on instructional guide by Josh Madakor. Watch the full step-by-step tutorial on [YouTube](https://youtu.be/MHsI8hJmggI).*
