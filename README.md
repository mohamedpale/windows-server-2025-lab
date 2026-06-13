# Windows Server 2025 Homelab: Active Directory & GPO Infrastructure

## 📌 Project Overview
### 🎛️ Host System Capabilities
* **Hypervisor Platform:** VMware Workstation Pro
* **Virtual Network Mapping:** Custom Host-Only Virtual Switch Subnet (`VMnet10`)
* **DHCP Configuration:** Disabled (All addressing manually assigned to prevent allocation conflicts)

### 📊 Provisioned Compute Resources

| VM Name / Role | OS Platform | vCPU Cores | RAM Allocation | Storage Allocation | Network Layer |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WIN-DC01** (Primary Domain Controller) | Windows Server 2025 | 2 Cores | 4 GB DDR4 | 60 GB NVMe (Thin) | Static IP: `192.168.10.10` |
| **WIN-CL01** (Workstation Client Node) | Windows 10 Pro / 11 Pro | 2 Cores | 4 GB DDR4 | 40 GB NVMe (Thin) | Static IP: `192.168.10.20` |

### 🌐 Network Routing Layer
* **Subnet Mask:** `255.255.255.0` (/24 Class C Network)
* **Primary Gateway Identifier:** `192.168.10.1`
* **Primary Domain Resolution (DNS):** `192.168.10.10` (Points directly to loopback Active Directory zone mapping)
This project demonstrates the deployment of a foundational corporate IT infrastructure using Windows Server 2025 within a virtualized sandbox environment (VMware Workstation). The goal was to build a secure, scalable directory domain from scratch to simulate real-world enterprise system administration.

## 🛠️ Technologies & Skills Used
* **Operating System:** Windows Server 2025
* **Hypervisor:** VMware Workstation
* **Core Roles:** Active Directory Domain Services (AD DS), DNS Server
* **Administration:** Group Policy Management, Active Directory Users and Computers (ADUC), PowerShell/CLI

## ⚙️ Implementation Steps

### Phase 1: Network & Identity Foundation
1. **Static IP Configuration:** Configured a dedicated static IP subnet (`192.168.10.10/24`) pointing loopback DNS to ensure persistent domain mapping.
2. **AD DS Promotion:** Installed Active Directory binaries and promoted the server to a root domain controller for the forest `homelab.local`.
3. **Directory Structure:** Designed an Organizational Unit (OU) structure (`Lab Users`) to separate administrative accounts from standard testing profiles.

### Phase 2: Security & Group Policy Enforcement
1. **GPO Creation:** Engineered a custom Group Policy Object (GPO) titled `Desktop Restriction Policy` linked directly to the `Lab Users` OU.
2. **Workstation Hardening:** Configured administrative registry templates to completely prohibit user access to the Control Panel and PC Settings apps (`gpedit` enforcement).
3. <img width="3414" height="1314" alt="Screenshot 2026-06-13 010705" src="https://github.com/user-attachments/assets/c796af4c-ee49-4623-a06d-18e266eba49d" />

4. **Policy Synchronization:** Forced manual background engine refreshes using the command-line interface (`gpupdate /force`) to guarantee instant replication.

## 🔍 Validation & Testing
* Verified Active Directory health and catalog storage targets inside `C:\Windows\NTDS`.
* Successfully synchronized and verified live User and Computer Policies via administrative command prompt validation.
