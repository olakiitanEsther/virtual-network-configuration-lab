# Building an Isolated Virtual Private Network (VirtualBox Lab)

## Project Overview
This project demonstrates the manual architecture, deployment, and validation of an isolated Virtual Private Network using Oracle VirtualBox. To accommodate strict local host hardware constraints (8 GB RAM), a cross-platform environment was engineered combining a graphical **Windows 10 Guest VM** with a lightweight, text-only **Ubuntu Server 20.04 CLI Guest VM**. Both nodes were successfully networked to validate local connectivity while maintaining complete host OS stability.

## Network Architecture & Design Constraints
* **Host Environment:** Windows 11 (8.00 GB RAM / Intel CPU Core / 258 GB Storage)
* **Virtualization Architecture:** Oracle VirtualBox Hypervisor
* **Network Topology:** Isolated Internal Network Loop (`Task 1` Broadcast Domain)
* **Routing Strategy:** No external gateway router; manual static IP assignment spanning a clean Class C address space (`192.168.1.0/24`).

### Node Infrastructure Specifications

| Network Node Element | VM 1: Windows Client | VM 2: Ubuntu Linux Server |
| :--- | :--- | :--- |
| **Operating System** | Windows 10 Pro (x64) | Ubuntu Server 20.04.6 LTS (CLI Only) |
| **Assigned RAM Memory**| 2048 MB (2.0 GB) | 1536 MB (1.5 GB) |
| **Compute Core Alloc.**| 1 vCPU Core | 1 vCPU Core |
| **Target Storage Allocation**| 50.00 GB (Dynamic VDI) | 15.28 GB (Dynamic VDI) |

---

## Static Network Configuration Matrix

| Attribute Parameter | VM 1: Windows Client Configuration | VM 2: Ubuntu Server Configuration |
| :--- | :--- | :--- |
| **Hostname / Device Name** | DESKTOP-A7B7VIB | ubuntu-vm |
| **Assigned IP Address** | 192.168.1.10 | 192.168.1.20 |
| **Subnet Mask Boundary** | 255.255.255.0 | 255.255.255.0 (`/24`) |
| **Default Exit Gateway** | None (Isolated Topology) | None (Isolated Topology) |
| **Hardware MAC Address** | 08-00-27-F2-DA-2C | 08:00:27:a1:9d:48 |

---

## Technical Challenges & Engineered Resolutions

### 1. Host Hardware Constraints (The 8 GB RAM Bottleneck)
* **Challenge:** Running a standard host OS alongside a heavy Windows 10 VM and a standard Ubuntu Desktop graphical VM pushed the laptop’s 8 GB physical RAM pool to 100% utilization. This resulted in system-wide freezing, locked input devices (mouse/keyboard lag), and out-of-memory alerts.
* **Resolution:** Swapped out the heavy Ubuntu Desktop graphical image file for a headless, lightweight **Ubuntu Server (CLI-only)** image file. Capped the Windows VM allocation at 2 GB and the Ubuntu Server VM at 1.5 GB. This minimized the hypervisor's footprint to 3.5 GB total, keeping the host laptop stable and responsive.

### 2. Graphical Kernel & Wizard Freeze (`vmwgfx` / Black Screen Crash)
* **Challenge:** Initial attempts to boot the standard desktop live installer with low resources caused driver mismatches (`vmwgfx seems to be running on an unsupported hypervisor`), permanently freezing the environment at a blank terminal screen before the installation menu could render.
* **Resolution:** Re-architected the virtual hardware container parameters within VirtualBox by standardizing the Graphics Controller configuration down to an accelerated storage driver template. Shifting completely to the text-based Ubuntu Server installation environment successfully bypassed the heavy X-server GUI stack initialization routines entirely.

### 3. Asymmetric ICMP Silent Packet Drops (The Firewall Block)
* **Challenge:** After successfully provisioning the manual network, the Windows VM could ping the Ubuntu Server node seamlessly, but the Ubuntu Server returned `Destination Host Unreachable` / timed-out loops when targeting the Windows client. 
* **Resolution:** Diagnosed the issue as an active network security profile block. Opened the classic Windows Control Panel via the command line layer (`ncpa.cpl`) and systematically deactivated **Windows Defender Firewall** profile layers across the private domain vector. This immediately opened up the communication loop, allowing bidirectional ICMP echo traffic to pass through.

### 4. Diagnostic Syntax Typo Errors
* **Challenge:** Early manual pings from the Linux CLI console repeatedly outputted immediate `Network is unreachable` protocol faults due to slight input tracking mistakes in the Class C subnet octets (typing `198.168.1.10` instead of the manually configured `192.168.1.10`).
* **Resolution:** Practiced direct terminal interface tracking and error logging checks to verify matching destination addresses before initiating structural socket connections.

---

## Deployment & Verification Workflows (Visual Evidence)

### Phase 1: Local Interface Configuration Verification
To initialize verification, system interfaces were updated to clear default DHCP states. Local system architecture layouts were validated using native shell tools (`ipconfig /all` on Windows and `ip addr` on Linux).

#### Windows Client Interface Baseline Metrics:
![Windows Interface Configuration Verification](Showing%20the%20successful%20IP%20addresses%20(ipconfig)%20WINDOW.jpeg)

#### Ubuntu Server Interface Baseline Metric
![Ubuntu Server Interface Configuration Verification](Showing%20the%20successful%20IP%20addresses%20(%20ip%20addr)%20UBUNTU.jpeg)




---

### Phase 2: Active Inter-Node Connectivity Testing (Successful Ping)
Bidirectional packet delivery loops were initiated across the virtual broadcast medium. Once firewall layers were cleared, execution commands targeting `ping 192.168.1.10` from Ubuntu and `ping 192.168.1.20` from Windows returned stable response metrics displaying **0% packet loss values**.

#### Bidirectional ICMP Connectivity Verification:
![Successful Network Connectivity Ping Loop - Ubuntu](UBUNTU-Showing%20the%20successful%20ping%20loop%20response.jpeg)

![Successful Network Connectivity Ping Loop - Windows](WINDOW%20-Showing%20the%20successful%20ping%20loop%20response.jpeg)


---

### Phase 3: Failure Boundary Validation & Subnet Analysis
To evaluate architectural failure boundaries, the Windows client address configuration was intentionally shifted out of bounds to an alternative logical range (`10.0.0.10`). Re-executing connectivity validation metrics immediately returned a shell termination string showing `PING: transmit failed. General failure` with **100% loss metrics**. This systematically proved that without an active structural layer-3 router device present within the virtual hypervisor engine, components residing outside the identical logical network mask boundaries are completely incapable of locating target routing paths.

#### Network Disconnection Boundary Proof:
<img width="1170" height="653" alt="Showing the failed ping window 100% loss)" src="https://github.com/user-attachments/assets/83d1e71f-2b97-44e8-b9d1-7516e522e78e" />





---

## Final Project Documentation Matrix
The structural network settings grid table was recorded inside Excel to formalize validation data profiles.

#### Final Data Record & Troubleshooting Analysis Grid Report:
[📊 Click Here to View/Download Completed Excel Documentation Matrix (Practical_Task_1_Network.xlsx)](Practical_Task_1_Network.xlsx)

---

## Key Skills Demonstrated
* Hypervisor Hardware Optimization & Memory Resource Scaling
* Cross-Platform Linux System Administration (Ubuntu Server CLI Deployment)
* Static IPv4 Network Addressing Schemes & Subnetting Logic
* Operating System Firewall Profiling & Interface Debugging
* Basic Networking Diagnostics (`ping`, `ipconfig`, `ip addr`)

---

## Author

**OLAKIITAN ESTHER**  
*Cybersecurity INTERN*  

https://www.linkedin.com/in/olagunju-olakiitan/

