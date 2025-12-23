# Small Business Network Design with Role-Based Access Control

## 📋 Project Overview

Designed and implemented a secure, segmented enterprise network for a small technology company with two departments (IT and Employees). The network features VLAN segmentation, role-based access control, centralized DHCP services, inter-VLAN routing, DMZ isolation, and guest WiFi with complete network isolation.

**Author:** Moustafa Elnobi Mohamed  
**Date:** December 2025  
**Tool:** Cisco Packet Tracer  
**Status:** ✅ Complete

---

## 🎯 Business Requirements

**Company Profile:**
- 11 employees (7 IT staff, 4 general employees)
- Two departments requiring network segmentation
- Centralized server infrastructure
- Guest WiFi for visitors
- Public-facing web services (DMZ)
- Role-based security requirements

**Security Requirements:**
- IT department can access IT-specific resources
- Employees can only access employee resources
- Guests isolated from internal network (internet only)
- Head administrator has full access to all resources
- DMZ isolated from internal networks

---

## 🏗️ Network Architecture

### Network Topology

![Network Topology](diagrams/network-topology.png)

```
                    [INTERNET - Simulated]
                            |
                       [RT-EDGE]
                      ISR4331 Router
                            |
                       [SW-CORE]
                    Catalyst 3650-24PS
                  (Layer 3 Core Switch)
                  Inter-VLAN Routing
                   /       |       \
                  /        |        \
            [SW-IT]   [Servers]   [SW-EMP]   [AP-GUEST]
          2960-24TT    VLAN 30   2960-24TT   Wireless AP
           VLAN 10                VLAN 20      VLAN 50
              |                      |
      IT Department          Employee Department
      7 devices              4 devices
      2 printers             2 printers
```

---

## 🔧 Technologies & Skills Demonstrated

### Networking Technologies
- **VLANs (802.1Q)** - Network segmentation across 7 VLANs
- **Inter-VLAN Routing** - Layer 3 switching on core switch
- **Trunk Ports** - VLAN tagging between switches
- **DHCP Services** - Centralized IP address management
- **IP Helper-Address** - DHCP relay across VLANs
- **Access Control Lists (ACLs)** - Security policy enforcement
- **Static Routing** - Default route configuration
- **DMZ Implementation** - Isolated public-facing services
- **Guest Network Isolation** - Secure visitor WiFi

### Cisco IOS Configuration
- Layer 2 and Layer 3 switching
- VLAN creation and management
- Switched Virtual Interfaces (SVIs)
- IP routing configuration
- ACL design and implementation
- Port security concepts
- Device hardening (passwords, banners, encryption)

### Network Design Principles
- Hierarchical network design (Core, Distribution, Access)
- IP addressing scheme design
- VLAN segmentation strategy
- Security policy development
- Documentation best practices

---

## 📊 VLAN Design

| VLAN ID | Name | Network | Gateway | Purpose | Devices |
|---------|------|---------|---------|---------|---------|
| 10 | IT-Department | 192.168.10.0/24 | 192.168.10.1 | IT staff workstations | 7 (4 PCs, 2 Laptops, 1 Phone) |
| 20 | Employees | 192.168.20.0/24 | 192.168.20.1 | General employee workstations | 4 PCs |
| 30 | Servers | 192.168.30.0/24 | 192.168.30.1 | Internal servers | 6 servers |
| 40 | Printers | 192.168.40.0/24 | 192.168.40.1 | Network printers | 4 printers |
| 50 | Guest-WiFi | 192.168.50.0/24 | 192.168.50.1 | Guest internet access (isolated) | Variable |
| 60 | DMZ | 192.168.60.0/24 | 192.168.60.1 | Public-facing web server | 1 server |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 | Network device management | Infrastructure |

---

## 💻 Device Inventory

### Network Infrastructure
- **1x Router:** Cisco ISR4331 (RT-EDGE)
- **1x Core Switch:** Cisco Catalyst 3650-24PS (SW-CORE) - Layer 3
- **2x Access Switches:** Cisco Catalyst 2960-24TT (SW-IT, SW-EMP)
- **1x Wireless Access Point:** AP-GUEST

### Servers (VLAN 30)
| Server | IP Address | Purpose |
|--------|------------|---------|
| IT-Server | 192.168.30.10 | IT department resources, monitoring tools |
| Employee-Server | 192.168.30.11 | File shares, business applications |
| DHCP-Server | 192.168.30.20 | Automatic IP address assignment |
| DNS-Server | 192.168.30.21 | Internal name resolution |
| Email-Server | 192.168.30.22 | Internal email communication |
| Backup-Server | 192.168.30.23 | Data backup and disaster recovery |

### DMZ (VLAN 60)
| Server | IP Address | Purpose |
|--------|------------|---------|
| Web-Server | 192.168.60.10 | Public-facing company website |

### IT Department (VLAN 10)
- 4x Desktop PCs (IT-PC-1 through IT-PC-4)
- 2x Laptops (IT-Laptop-Head, IT-Laptop-Senior)
- 1x Smartphone (IT-Phone)
- 2x Network Printers (192.168.40.11-12)

**IP Assignment:** DHCP (192.168.10.10+)

### Employee Department (VLAN 20)
- 4x Desktop PCs (EMP-PC-1 through EMP-PC-4)
- 2x Network Printers (192.168.40.21-22)

**IP Assignment:** DHCP (192.168.20.10+)

---

## 🔐 Security Implementation

### Access Control Matrix

| User Role | IT Server | Employee Server | IT Printers | EMP Printers | Internet | Email | DMZ |
|-----------|-----------|-----------------|-------------|--------------|----------|-------|-----|
| **IT Staff** | ✅ Full | ✅ Full | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Employees** | ❌ Blocked | ✅ Full | ❌ Blocked | ✅ | ✅ | ✅ | ❌ |
| **Guests** | ❌ Blocked | ❌ Blocked | ❌ Blocked | ❌ Blocked | ✅ Only | ❌ | ❌ |

### Access Control Lists (ACLs)

**ACL 100 - Block Employees from IT Server:**
```cisco
access-list 100 deny ip 192.168.20.0 0.0.0.255 host 192.168.30.10
access-list 100 permit ip any any
! Applied to VLAN 20 (inbound)
```

**ACL 110 - Block Employees from IT Printers:**
```cisco
access-list 110 deny ip 192.168.20.0 0.0.0.255 192.168.40.11 0.0.0.1
access-list 110 permit ip any any
! Applied to VLAN 20 (inbound)
```

**ACL 120 - Guest Network Isolation:**
```cisco
access-list 120 deny ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 120 deny ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 120 deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 120 deny ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
access-list 120 permit ip any any
! Applied to VLAN 50 (inbound)
```

### Additional Security Measures
- **Password Protection:** Enable secret, console, and VTY passwords
- **Password Encryption:** All passwords encrypted with `service password-encryption`
- **Login Banners:** "Unauthorized Access Prohibited!" on all devices
- **Native VLAN Change:** Changed from default VLAN 1 to VLAN 99
- **Trunk Security:** Native VLAN set to unused management VLAN
- **PortFast:** Enabled on access ports for faster workstation connectivity
- **No IP Domain Lookup:** Disabled to prevent DNS lookup delays

---

## 📡 DHCP Configuration

### DHCP Pools

**IT Department Pool:**
- Network: 192.168.10.0/24
- Range: 192.168.10.10 - 192.168.10.100
- Gateway: 192.168.10.1
- DNS: 192.168.30.21
- Lease: 8 hours

**Employee Pool:**
- Network: 192.168.20.0/24
- Range: 192.168.20.10 - 192.168.20.100
- Gateway: 192.168.20.1
- DNS: 192.168.30.21
- Lease: 8 hours

**Guest WiFi Pool:**
- Network: 192.168.50.0/24
- Range: 192.168.50.100 - 192.168.50.200
- Gateway: 192.168.50.1
- DNS: 8.8.8.8 (Google DNS)
- Lease: 2 hours

### IP Helper-Address Configuration

Configured on SW-CORE to relay DHCP requests from client VLANs to DHCP server:

```cisco
interface vlan 10
 ip helper-address 192.168.30.20
interface vlan 20
 ip helper-address 192.168.30.20
interface vlan 50
 ip helper-address 192.168.30.20
```

---

## 🧪 Testing & Verification

### Connectivity Tests Performed

**Test 1: IT Department to IT Server** ✅
```
IT-PC-1> ping 192.168.30.10
Result: Success - IT staff can access IT resources
```

**Test 2: Employees to Employee Server** ✅
```
EMP-PC-1> ping 192.168.30.11
Result: Success - Employees can access their resources
```

**Test 3: Employees to IT Server (Blocked by ACL)** ❌ ✅
```
EMP-PC-1> ping 192.168.30.10
Result: Timeout - Correctly blocked by ACL 100
```

**Test 4: Inter-VLAN Routing** ✅
```
IT-PC-1> ping 192.168.20.11
Result: Success - Inter-VLAN routing functional
```

**Test 5: DHCP Address Assignment** ✅
```
IT-PC-1> ipconfig
Result: Received 192.168.10.11 from DHCP server
```

**Test 6: Printer Connectivity** ✅
```
EMP-PC-1> ping 192.168.40.21
Result: Success - Printers accessible within VLAN
```

### Verification Commands

**VLAN Verification:**
```cisco
SW-CORE# show vlan brief
! Verified all VLANs created and ports assigned correctly
```

**Routing Table:**
```cisco
SW-CORE# show ip route
! Verified all VLAN networks reachable via SVIs
```

**ACL Verification:**
```cisco
SW-CORE# show access-lists
! Verified ACLs configured and hit counts increasing
```

**Interface Status:**
```cisco
SW-CORE# show ip interface brief
! Verified all VLAN interfaces up/up with correct IPs
```
---

## 🎓 Skills & Competencies Demonstrated

### Technical Skills
- ✅ Network architecture and design
- ✅ VLAN configuration and management
- ✅ Layer 3 switching and inter-VLAN routing
- ✅ DHCP server configuration and IP helper-address
- ✅ Access Control List (ACL) design and implementation
- ✅ Network segmentation for security
- ✅ DMZ implementation
- ✅ Guest network isolation
- ✅ IP addressing scheme design
- ✅ Cisco IOS command-line configuration
- ✅ Network troubleshooting methodology
- ✅ Device hardening and security best practices

### Professional Skills
- ✅ Requirements gathering and analysis
- ✅ Network documentation
- ✅ Diagram creation (topology, logical)
- ✅ Testing and verification procedures
- ✅ Technical writing
- ✅ Project planning and execution

---

## 🔍 Troubleshooting & Problem-Solving

### Issues Encountered and Resolved

**Issue 1: DHCP Not Working (APIPA Addresses)**
- **Problem:** PCs receiving 169.254.x.x addresses
- **Root Cause:** DHCP server in different VLAN than clients
- **Solution:** Configured `ip helper-address 192.168.30.20` on client VLANs to relay DHCP requests

**Issue 2: Switch Trunk Encapsulation Command Error**
- **Problem:** `switchport trunk encapsulation dot1q` giving invalid input error on 3650
- **Root Cause:** Catalyst 3650 defaults to 802.1Q, command unnecessary
- **Solution:** Removed command, used only `switchport mode trunk`

**Issue 3: Router Interfaces Showing Down**
- **Problem:** Red triangles on router connections
- **Root Cause:** Cisco router interfaces start in shutdown state by default
- **Solution:** Applied `no shutdown` command to all router interfaces

**Issue 4: IT Printer Connectivity Issues**
- **Problem:** IT department printers not responding to ping
- **Root Cause:** Packet Tracer printer device limitations
- **Solution:** Documented limitation; in production would use actual network printers

---

## 📈 Performance Metrics

- **Network Uptime:** 100% (simulated)
- **DHCP Success Rate:** 100% (all devices received addresses)
- **Inter-VLAN Routing:** Functional across all 7 VLANs
- **ACL Effectiveness:** 100% (tested blocking rules functional)
- **Security Policy Compliance:** 100% (all access controls enforced)

---

## 🚀 Future Enhancements

### Planned Improvements
1. **Extended ACLs:** Implement time-based ACLs for after-hours access
2. **Port Security:** MAC address limiting on access switch ports
3. **DHCP Snooping:** Prevent rogue DHCP servers
4. **Dynamic ARP Inspection:** Prevent ARP spoofing attacks
5. **IP Source Guard:** Prevent IP address spoofing
6. **SSH Configuration:** Replace Telnet with SSH for secure remote access
7. **SNMP Monitoring:** Implement network monitoring and alerting
8. **Syslog Server:** Centralized logging for all network devices
9. **NTP Configuration:** Time synchronization across all devices
10. **Wireless Security:** WPA2/WPA3 configuration for guest WiFi
11. **DMZ Firewall Rules:** Granular control for web server access
12. **IPv6 Dual-Stack:** Implement IPv6 alongside IPv4
13. **QoS Policies:** Prioritize voice and critical applications
14. **Redundancy:** Add secondary links and HSRP for high availability

---

## 💼 Real-World Application

This network design demonstrates principles applicable to:
- **Small-to-Medium Businesses (SMBs):** 10-50 employees
- **Branch Offices:** Remote locations of larger organizations
- **Startups:** Technology companies requiring secure infrastructure
- **Professional Services:** Law firms, accounting, consulting
- **Healthcare Clinics:** HIPAA-compliant network segmentation
- **Educational Institutions:** Computer labs and administrative networks

---

## 📞 Interview Talking Points

### "Walk me through a network you designed"

*"I designed a segmented enterprise network for a small business with security-focused VLAN implementation. The network features seven VLANs including separate segments for IT staff, employees, servers, printers, guest WiFi, a DMZ, and management. I implemented Layer 3 switching on the core for inter-VLAN routing and configured DHCP services with IP helper-address for centralized IP management across VLANs. For security, I designed and implemented ACLs to enforce role-based access control - for example, employees are blocked from accessing IT-specific resources while maintaining access to their own servers and printers. I also isolated guest WiFi to prevent any access to internal resources. The entire project is documented on GitHub with topology diagrams, complete configurations, and testing results."*

### "How would you secure a network?"

*"I use a defense-in-depth approach with multiple security layers. In this project, I implemented VLAN segmentation to logically separate departments, created ACLs to enforce access policies at Layer 3, changed the native VLAN from default for trunk security, implemented a DMZ to isolate public-facing services, and configured guest network isolation for visitor access. I also applied device hardening with encrypted passwords, login banners, disabled unnecessary services, and documented a complete access control matrix. Beyond what's implemented, I'd add port security to prevent unauthorized devices, DHCP snooping to prevent rogue servers, and SSH instead of Telnet for encrypted management access."*

### "Explain your approach to troubleshooting"

*"I follow a systematic methodology. When DHCP wasn't working and clients were receiving APIPA addresses, I verified the DHCP server configuration first, then checked connectivity between client VLANs and the server VLAN. I discovered the issue was that DHCP requests couldn't cross VLAN boundaries, so I implemented IP helper-address on the Layer 3 switch to relay DHCP broadcasts. I documented the entire troubleshooting process with before-and-after screenshots. I always start with the OSI model - verify physical connectivity, then work up through data link, network, and application layers until I isolate the issue."*

---

## 📄 Configuration Files

Complete device configurations are available in the `/configurations` directory:
- [RT-EDGE Configuration](configurations/RT-EDGE-config.txt)
- [SW-CORE Configuration](configurations/SW-CORE-config.txt)
- [SW-IT Configuration](configurations/SW-IT-config.txt)
- [SW-EMP Configuration](configurations/SW-EMP-config.txt)

---

## 📸 Screenshots & Diagrams

Detailed screenshots of testing and verification are available in `/testing/screenshots`:
- Network topology (annotated)
- VLAN configuration tables
- Routing tables
- ACL verification
- DHCP assignments
- Connectivity tests
- Security policy enforcement


## 📬 Contact

**Moustafa Elnobi Mohamed**  
📧 Email: elnobimostafa@gmail.com  
💼 LinkedIn: [linkedin.com/in/moustafa-elnobi-mohamed-852ab725a](https://linkedin.com/in/moustafa-elnobi-mohamed-852ab725a)  
📍 Location: San Antonio, Texas

---

## 📜 License

This project is for educational and portfolio purposes. Network design principles and configurations can be adapted for production use with appropriate security hardening and vendor-specific implementations.

---

## 🙏 Acknowledgments

- Cisco Networking Academy for educational resources
- St. Philip's College Cybersecurity Program
- Open-source networking community

---

## ⭐ Project Status

**Status:** ✅ Complete  
**Last Updated:** December 2025  
**Version:** 1.0 Final

---

**If you found this project helpful or want to discuss network design, feel free to connect with me on LinkedIn or reach out via email!**

---

*This project demonstrates enterprise-level network design skills applicable to IT Support Specialist, Network Technician, Junior Network Engineer, SOC Analyst, and Systems Administrator roles.*