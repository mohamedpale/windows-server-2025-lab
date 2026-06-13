# Enterprise Infrastructure Sandbox: Windows Server 2025 Active Directory & Windows 11 Client Integration

## 📌 Project Overview
This project demonstrates the architectural design and deployment of an isolated enterprise perimeter network using Windows Server 2025 and a Windows 11 Pro client workstation node. Built entirely within a Type-2 hypervisor sandbox, this environment simulates a real-world corporate directory services ecosystem, centering on centralized identity management, secure DNS routing, and Group Policy workstation hardening.

## 🛠️ Technologies & Core Roles
* **Host Hypervisor:** VMware Workstation Pro
* **Server Platform:** Windows Server 2025 Datacenter (Primary Domain Controller)
* **Client Workstation:** Windows 11 Pro (Domain-Joined)
* **Network Infrastructure:** Active Directory Domain Services (AD DS), Standalone DNS Server
* **Policy Management:** Group Policy Management Console (GPMC), Administrative Template Enforcement

---

## 🖥️ Hypervisor & Virtual Machine Topology

To guarantee absolute isolation and prevent overlap with external network infrastructures, all nodes are bound to a custom, non-routable layer-2 hardware abstraction link.

### 📊 Provisioned Compute Resources

| VM Name / Role | OS Platform | vCPU Cores | RAM Allocation | Storage Allocation | Network Configuration |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WIN-DC01** (Domain Controller) | Windows Server 2025 | 2 Cores | 4 GB DDR4 | 60 GB NVMe | Static IP: `192.168.10.10` |
| **WIN-CL01** (Workstation Node) | Windows 11 Pro | 2 Cores | 4 GB DDR4 | 60 GB NVMe | Static IP: `192.168.10.20` |

### 🌐 Network Routing Layer
* **Network Segment:** VMware Isolated LAN Segment (`LabNet`)
* **Subnet Mask:** `255.255.255.0` (Class C /24 Subnet)
* **Primary Gateway Identifier:** `192.168.10.1`
* **Domain Name Resolution (DNS):** `192.168.10.10` (Iterated loopback mapping pointing directly to the AD integrated DNS zone)

---

## ⚙️ Implementation Phases

### Phase 1: Network & Active Directory Directory Foundation
1. **Static IPv4 Allocation:** Provisioned fixed network blocks on both host parameters. Configured loopback routing mechanisms on the server to handle internal DNS catalog synchronization queries.
2. **AD DS Promotion:** Automated forest creation for root directory target `homelab.local`, mapping database, transaction log files, and SYSVOL pointers to secure disk structures.
3. **Directory Topology:** Created a custom Organizational Unit (OU) container titled `Lab Users` to separate managed corporate workforce accounts from default global system containers.

### Phase 2: Security Engineering & Workstation Hardening via GPO
1. **GPO Engineering:** Built and deployed a dedicated Group Policy Object (GPO) titled `Desktop Restriction Policy` structurally linked directly to the `Lab Users` OU.
2. **Registry-Level Enforcement:** Navigated administrative template nodes to execute policy directive: *Prohibit access to Control Panel and PC settings*.
3. **Engine Refresh Optimization:** Used the administrative command-line interface to push immediate background policy synchronization (`gpupdate /force`).
4. <img width="3414" height="1314" alt="Screenshot 2026-06-13 010705" src="https://github.com/user-attachments/assets/db657eba-a403-4265-a95e-0448591ef069" />


### Phase 3: Client Provisioning & Active Directory Integration
1. **OOBE Local Bypass:** Bypassed cloud hardware enforcement mechanisms during the Windows 11 Out-of-Box Experience via command line execution (`OOBE\BYPASSNRO`) to provision local fallback profiles.
2. **Domain Integration Subroutine:** Aligned the client workstation's virtual network card to the `LabNet` segment, updated local DNS properties to point to the server, and executed a secure domain join to bind `WIN-CL01` to `homelab.local`.

---

## 🔍 Validation & Testing

* **Network Verification:** Verified localized network transit capabilities via command-line ICMP probing (`ping homelab.local`), yielding 0% packet loss and sub-1ms transit latency.
* **Authentication Handshake:** Logged out of local administrative states and successfully authenticated against the active directory database using the domain user structure (`HOMELAB\ahadi`).
* **Policy Compliance Proof:** Attempted to launch the Windows 11 Settings Engine and Control Panel under the managed user context. The system kernel actively intercepted and dropped the process call, proving 100% Group Policy compliance and security rule replication.
<img width="1685" height="976" alt="Screenshot 2026-06-13 132227" src="https://github.com/user-attachments/assets/a017bbac-20ca-48d0-84dc-3e59495d9d38" />
---

# Project 3: Secure Corporate File Server Deployment (NAS Simulation)

## 📌 Project Overview
This project details the creation and deployment of a centralized, secure network file storage pool (Network Attached Storage simulation) managed directly by the Windows Server 2025 domain controller. The objective was to configure custom network shares and partition folder directory clearances using strict Access Control Lists (ACLs) to protect corporate financial assets while maintaining open collaboration folders for staff.

## ⚙️ Implementation & Permission Scoping

1. **Directory Tree Architecture:** Created a centralized root pool `C:\CorpShares` on the server storage volume housing two specific departmental sub-folders: `Accounting` and `Public`.
2. **Network Share Provisioning:** Provisioned advanced network sharing flags across the environment mapping the absolute Universal Naming Convention (UNC) paths (`\\192.168.10.10\Accounting` & `\\192.168.10.10\Public`).
3. **Role-Based Access Control (RBAC):**
   * **Public Share:** Configured network share permissions to grant `Everyone` change and read privileges to allow open document collaboration.
   * **Accounting Share:** Stripped default administrative inherited inheritance pathways. Configured explicit Share and NTFS permission boundaries to explicitly block standard user parameters, restricting data traffic solely to authorized infrastructure administrators.

## 🔍 Validation & Testing Proof
<img width="1345" height="1052" alt="Screenshot 2026-06-13 163604" src="https://github.com/user-attachments/assets/91c62ceb-d622-44a5-b9ef-401017ce728f" />
<img width="1335" height="981" alt="Screenshot 2026-06-13 164402" src="https://github.com/user-attachments/assets/9014f924-31fa-4368-b100-8db7186eb1bf" />

* Successfully executed a remote network path routing loop from the Windows 11 client machine (`\\192.168.10.10`).
* Verified folder structural boundaries: The client successfully interacted with the `Public` directory, while attempts to access the protected `Accounting` zone were actively dropped at the network kernel level with a **"Permission Denied / Access Is Denied"** security block.
* <img width="1405" height="1001" alt="Screenshot 2026-06-13 164301" src="https://github.com/user-attachments/assets/824e235b-712c-4089-a57b-cd19c44f37c3" />
<img width="1387" height="994" alt="Screenshot 2026-06-13 165126" src="https://github.com/user-attachments/assets/cb283ebe-c706-4b62-88e0-020d16e6e95f" />
