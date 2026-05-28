# Home Lab Running Active Directory & PowerShell User Automation

## Project Architecture & Components

* **Hypervisor:** Oracle VirtualBox
* **Domain Controller (DC):** Windows Server 2019 (Standard with Desktop Experience)
* **Client Machine:** Windows 10 Pro
* **Automation:** PowerShell Script for bulk Active Directory Provisioning

---

<img width="784" height="258" alt="AD_PNG drawio" src="https://github.com/user-attachments/assets/58492eaf-9b76-4514-be68-492478e0bb64" />

---

## Network Configuration

| Network Adapter | Configuration Type | IP Addressing | Purpose |
| :--- | :--- | :--- | :--- |
| **Adapter 1 (DC)** | NAT | DHCP (Automatic from Local Network) | Direct Outside Internet Connection |
| **Adapter 2 (DC)** | Internal VM Network | `172.16.0.1` (Subnet: `255.255.255.0`) | Private Virtual Gateway / DNS server |
| **Client 1** | Internal Network | DHCP Leased (`172.16.0.100` - `172.16.0.200`) | Enterprise Workstation |

### Network Services Implemented:
* **Active Directory Domain Services (AD DS):** Deployed a new forest (`mydomain.com`).
* **Routing and Remote Access (RAS / NAT):** Configured NAT to route client traffic from internal network through the DC's public NIC.
* **DHCP Server:** Set up dynamic scope range (`172.16.0.100` to `172.16.0.200`) ensuring to pass `172.16.0.1` as both the Router (Default Gateway) and Primary DNS Server.

---

## Automated User Provisioning (PowerShell)

Rather than manually generating AD entries for 1k+ users, leveraged GenAI to develop a custom PowerShell script to be executed to bulk create 1k randomized user profiles under a dedicated organizational unit (OU).

### Key Script Actions:
1. Generates an Organizational Unit (`_users`) configured with `ProtectedFromAccidentalDeletion = $false`.
2. References a `names.txt` file containing a target string of randomized user names.
3. Splits names iteratively into lowercase structural strings to programmatically map `First Initial + Last Name` formats (e.g., `Brian Hernandez` > `bhernandez`).
4. Generates an Active Directory Account object assigning secure password parameters (`Password1`), auto-enabling profiles, and bypassing initial standard expirations.

<img width="777" height="732" alt="script_ad" src="https://github.com/user-attachments/assets/fc386e71-9b82-47d1-a8e2-76fa98469281" />
<img width="1242" height="817" alt="scriptcomplete" src="https://github.com/user-attachments/assets/31216cfd-d168-42ab-849b-2540922a81f2" />

---

## 🚀 Deployment & Verification Steps

1. **Virtual Machines Setup:** Installed Windows Server 2019 and Windows 10 Pro on VirtualBox, modifying CPU processors and injecting Bi-directional clipboard integration via VirtualBox Guest Additions.
2. **Promote Domain Controller:** Configured AD DS, named the instance `DC`, configured dedicated admin structures, and stood up the DNS mapping footprint.
3. **Configure NAT & DHCP:** Authorized the local server routing components and activated the DHCP client pool leases.
4. **Execute Automation:** Ran the user provisioning loop script through the PowerShell Integrated Scripting Environment (ISE) as an administrator.
5. **Domain Join:** Altered the Windows 10 desktop computer name to `Client1`, targeted the local directory network gateway, joined the workstation to `mydomain.com` FQDM, and verified global sign-on profiles via newly script-generated credentials.

<img width="782" height="836" alt="final1" src="https://github.com/user-attachments/assets/131b70e5-822f-4e42-80ec-182a60fd0da2" />
<img width="931" height="721" alt="dns_ad" src="https://github.com/user-attachments/assets/c9b05724-37df-4766-9dc6-2e11ee7fd5be" />

---

<h2>🤳 Connect with me on LinkedIn:</h2>

[<img align="left" alt="BrianHernandez | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]

[linkedin]: https://www.linkedin.com/in/brian-hernandez04/

  
