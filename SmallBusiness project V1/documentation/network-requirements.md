# Network Requirements Document
## Small Business Network Design Project

**Project Name:** SecureCorp Network Infrastructure  
**Author:** Moustafa Elnobi Mohamed  
**Date:** December 2025  
**Version:** 1.0

---

## 1. Executive Summary

This document outlines the network infrastructure requirements for SecureCorp, a small technology company with two primary departments: IT and General Employees. The network must provide secure, segmented access to resources while maintaining ease of management and scalability.

---

## 2. Business Requirements

### 2.1 Company Overview
- **Company Size:** 11 users (7 IT staff, 4 employees)
- **Departments:** IT Department, General Employees
- **Growth Projection:** 25% increase over next 2 years
- **Business Hours:** 24/7 IT operations, standard business hours for employees

### 2.2 Primary Objectives
1. Secure network segmentation between departments
2. Role-based access control (RBAC) for resources
3. Reliable printing services for both departments
4. Centralized server infrastructure
5. Guest WiFi for visitors (isolated from internal network)
6. Public-facing web server (DMZ)
7. Mobile device support (laptops, smartphones)
8. Automated IP management (DHCP)
9. Internal name resolution (DNS)
10. Email services for internal communication

---

## 3. User Requirements

### 3.1 IT Department (7 Users)

**Head Administrator (1 user):**
- **Devices:** 1 Laptop (for mobility)
- **Access Level:** Full access to all servers and resources
- **Requirements:**
  - Access IT Server (full control)
  - Access Employee Server (full control)
  - Print to all printers
  - Remote access capability
  - Administrative access to network devices
  - Internet access
  - Email access
  - DMZ server management

**Senior Administrator (1 user):**
- **Devices:** 1 Laptop (for mobility)
- **Access Level:** Full IT access, read-only Employee Server access
- **Requirements:**
  - Access IT Server (full control)
  - Access Employee Server (read-only)
  - Print to all printers
  - Remote access capability
  - Internet access
  - Email access
  - DMZ server monitoring

**Junior Administrators (4 users):**
- **Devices:** 4 Desktop PCs
- **Access Level:** IT Server limited access only
- **Requirements:**
  - Access IT Server (limited - assigned tasks only)
  - No access to Employee Server
  - Print to IT printers only
  - Internet access
  - Email access
  - No DMZ access

**Testing Device:**
- **Device:** 1 Smartphone
- **Access Level:** IT network access
- **Requirements:**
  - WiFi connectivity
  - Used for network testing
  - Access IT resources

---

### 3.2 Employee Department (4 Users)

**General Employees (4 users):**
- **Devices:** 4 Desktop PCs
- **Access Level:** Employee resources only
- **Requirements:**
  - Access Employee Server (full access)
  - No access to IT Server
  - Print to Employee printers only
  - Internet access
  - Email access
  - No administrative access

---

### 3.3 Guest Users (Variable)

**Visitors/Clients:**
- **Devices:** Personal laptops, phones, tablets
- **Access Level:** Internet only (isolated)
- **Requirements:**
  - WiFi connectivity
  - Internet access only
  - Completely isolated from internal network
  - No access to any internal resources

---

## 4. Network Requirements

### 4.1 Network Segmentation

**Required VLANs:**
1. **VLAN 10 - IT Department**
   - Purpose: IT staff workstations and mobile devices
   - Subnet: 192.168.10.0/24
   - Users: 7 devices

2. **VLAN 20 - Employees**
   - Purpose: General employee workstations
   - Subnet: 192.168.20.0/24
   - Users: 4 devices

3. **VLAN 30 - Servers**
   - Purpose: Internal servers (IT, Employee, DHCP, DNS, Email, Backup)
   - Subnet: 192.168.30.0/24
   - Devices: 6 servers

4. **VLAN 40 - Printers**
   - Purpose: Network printers (shared resource)
   - Subnet: 192.168.40.0/24
   - Devices: 4 printers

5. **VLAN 50 - Guest WiFi**
   - Purpose: Guest internet access (isolated)
   - Subnet: 192.168.50.0/24
   - Devices: Variable

6. **VLAN 60 - DMZ**
   - Purpose: Public-facing web server
   - Subnet: 192.168.60.0/24
   - Devices: 1 web server

7. **VLAN 99 - Management**
   - Purpose: Network device management
   - Subnet: 192.168.99.0/24
   - Devices: Network infrastructure

---

### 4.2 Server Requirements

**IT Server:**
- **Purpose:** IT department tools, monitoring, security applications
- **Access:** Head Admin (full), Senior Admin (full), Junior Admins (limited)
- **IP:** 192.168.30.10 (static)
- **Services:** Administrative tools, security monitoring, log management

**Employee Server:**
- **Purpose:** File shares, business applications, employee resources
- **Access:** Head Admin (full), Senior Admin (read-only), Employees (full)
- **IP:** 192.168.30.11 (static)
- **Services:** File sharing, application hosting, document storage

**DHCP Server:**
- **Purpose:** Automatic IP address assignment
- **Access:** IT administrators only
- **IP:** 192.168.30.20 (static)
- **Services:** DHCP pools for all VLANs

**DNS Server:**
- **Purpose:** Internal name resolution
- **Access:** IT administrators only
- **IP:** 192.168.30.21 (static)
- **Services:** Internal DNS zone, external forwarding

**Email Server:**
- **Purpose:** Internal email communication
- **Access:** All internal users
- **IP:** 192.168.30.22 (static)
- **Services:** SMTP, POP3/IMAP

**Backup Server:**
- **Purpose:** Automated data backups
- **Access:** IT administrators only
- **IP:** 192.168.30.23 (static)
- **Services:** Scheduled backups, disaster recovery

**Web Server (DMZ):**
- **Purpose:** Public-facing company website
- **Access:** Public internet, IT admins for management
- **IP:** 192.168.60.10 (static)
- **Services:** HTTP/HTTPS, company website

---

### 4.3 Printer Requirements

**IT Department Printers (2 units):**
- **Access:** IT department only
- **IPs:** 192.168.40.11, 192.168.40.12 (static)
- **Protocols:** IPP, LPD

**Employee Department Printers (2 units):**
- **Access:** Employees only
- **IPs:** 192.168.40.21, 192.168.40.22 (static)
- **Protocols:** IPP, LPD

---

### 4.4 Network Infrastructure

**Core Switch (Layer 3):**
- **Purpose:** Inter-VLAN routing, core connectivity
- **Features Required:**
  - Layer 3 switching (routing between VLANs)
  - VLAN support
  - SVI (Switched Virtual Interface) configuration
  - Minimum 24 ports
- **Management IP:** 192.168.99.1

**IT Department Switch:**
- **Purpose:** Access switch for IT department
- **Features Required:**
  - VLAN support
  - Port security
  - Minimum 12 ports
- **Management IP:** 192.168.99.2

**Employee Department Switch:**
- **Purpose:** Access switch for employee department
- **Features Required:**
  - VLAN support
  - Port security
  - Minimum 12 ports
- **Management IP:** 192.168.99.3

**Edge Router:**
- **Purpose:** Internet connectivity, firewall, ACLs
- **Features Required:**
  - NAT/PAT for internal to internet
  - ACL support for access control
  - Default route to ISP
  - Firewall capabilities
- **Management IP:** 192.168.99.254

**Wireless Access Point:**
- **Purpose:** WiFi for IT laptops, smartphone, guest access
- **Features Required:**
  - Multiple SSID support (IT WiFi, Guest WiFi)
  - VLAN tagging
  - WPA2/WPA3 security
- **Connected to:** Core Switch (trunk port)

---

## 5. Security Requirements

### 5.1 Access Control Lists (ACLs)

**ACL 1: IT Department to Servers**
- Permit: IT VLAN (192.168.10.0/24) to IT Server (192.168.30.10)
- Permit: Head Admin laptop (192.168.10.21) to Employee Server (192.168.30.11) - Full
- Permit: Senior Admin laptop (192.168.10.22) to Employee Server (192.168.30.11) - Read-only
- Deny: Junior Admins to Employee Server

**ACL 2: Employee Department to Servers**
- Permit: Employee VLAN (192.168.20.0/24) to Employee Server (192.168.30.11)
- Deny: Employee VLAN to IT Server

**ACL 3: IT Department to Printers**
- Permit: IT VLAN (192.168.10.0/24) to IT Printers (192.168.40.11-12)
- Deny: IT VLAN to Employee Printers (192.168.40.21-22)

**ACL 4: Employee Department to Printers**
- Permit: Employee VLAN (192.168.20.0/24) to Employee Printers (192.168.40.21-22)
- Deny: Employee VLAN to IT Printers (192.168.40.11-12)

**ACL 5: Guest Network Isolation**
- Permit: Guest VLAN (192.168.50.0/24) to Internet only
- Deny: Guest VLAN to all internal VLANs

**ACL 6: DMZ Protection**
- Permit: Internet to Web Server (192.168.60.10) on ports 80, 443
- Permit: IT admins to Web Server (management)
- Deny: DMZ to internal networks (except responses)

---

### 5.2 Port Security

**Access Switches:**
- Enable port security on all access ports
- Maximum 2 MAC addresses per port (for phones + PC)
- Violation mode: Shutdown
- Sticky MAC address learning

---

### 5.3 VLAN Security

- All unused switch ports: Shutdown and assign to unused VLAN (VLAN 999)
- Native VLAN changed from 1 to 99 (management VLAN)
- Disable DTP (Dynamic Trunking Protocol) on access ports

---

### 5.4 Device Security

**All Network Devices:**
- Enable secret password for privileged mode
- Console and VTY passwords required
- Password encryption enabled
- Login banner configured
- Disable unused services (HTTP server, CDP on edge ports)
- SSH enabled for remote access (Telnet disabled)

---

## 6. Service Requirements

### 6.1 DHCP Configuration

**DHCP Pools Required:**

**Pool 1: IT Department**
- Network: 192.168.10.0/24
- Range: 192.168.10.10 - 192.168.10.100
- Gateway: 192.168.10.1
- DNS: 192.168.30.21
- Lease: 8 hours

**Pool 2: Employees**
- Network: 192.168.20.0/24
- Range: 192.168.20.10 - 192.168.20.100
- Gateway: 192.168.20.1
- DNS: 192.168.30.21
- Lease: 8 hours

**Pool 3: Guest WiFi**
- Network: 192.168.50.0/24
- Range: 192.168.50.100 - 192.168.50.200
- Gateway: 192.168.50.1
- DNS: 8.8.8.8 (external)
- Lease: 2 hours

---

### 6.2 DNS Configuration

**Internal DNS Zones:**
- Domain: securecorp.local
- IT-Server: itserver.securecorp.local → 192.168.30.10
- Employee-Server: empserver.securecorp.local → 192.168.30.11
- Email-Server: mail.securecorp.local → 192.168.30.22

**External Forwarding:**
- Forward unresolved queries to 8.8.8.8 (Google DNS)

---

### 6.3 Routing Configuration

**Inter-VLAN Routing:**
- Configured on Core Switch (Layer 3)
- SVI (Switched Virtual Interface) for each VLAN
- Default route pointing to Edge Router

**Default Route:**
- Edge Router has default route to ISP (simulated internet)

---

## 7. Performance Requirements

### 7.1 Network Performance
- **Bandwidth:** 1 Gbps internal, 100 Mbps internet
- **Latency:** < 10ms internal, < 50ms internet
- **Uptime:** 99.5% availability

### 7.2 Server Performance
- **Response Time:** < 1 second for file access
- **Concurrent Users:** Support all users simultaneously
- **Backup Schedule:** Daily at 2:00 AM

---

## 8. Scalability Requirements

### 8.1 Growth Capacity
- Network designed to support 30% growth (15 users total)
- Additional VLAN capacity available
- Sufficient IP address space in each subnet

### 8.2 Future Expansion
- Room for additional servers
- Additional printer support
- Wireless access point expansion
- Additional VLANs as needed

---

## 9. Compliance & Standards

### 9.1 Industry Standards
- IEEE 802.1Q (VLAN tagging)
- IEEE 802.11 (WiFi standards)
- TCP/IP standards
- DHCP RFC 2131
- DNS RFC 1035

### 9.2 Best Practices
- Network segmentation
- Least privilege access
- Defense in depth
- Regular backups
- Change management

---

## 10. Documentation Requirements

### 10.1 Required Documentation
- Network topology diagrams (physical and logical)
- IP addressing scheme
- VLAN configuration details
- ACL configurations
- Device configurations (backed up)
- User access matrix
- Troubleshooting procedures
- Change log

### 10.2 Naming Conventions
- Devices: [Type]-[Location/Function]-[Number]
  - Example: SW-CORE-01, RT-EDGE-01, PC-IT-01
- VLANs: Descriptive names (IT-Department, Employees, Servers)
- Passwords: Minimum 12 characters, complexity required

---

## 11. Testing Requirements

### 11.1 Connectivity Tests
- Verify all devices receive DHCP addresses
- Test inter-VLAN routing
- Verify DNS resolution
- Test email connectivity
- Verify internet access

### 11.2 Security Tests
- Verify ACLs block unauthorized access
- Test port security violations
- Verify VLAN isolation
- Test guest network isolation
- Verify DMZ access restrictions

### 11.3 Performance Tests
- Bandwidth testing between VLANs
- Server response time testing
- Printer accessibility testing
- WiFi coverage and speed testing

---

## 12. Success Criteria

**Project is considered successful when:**

1. ✅ All 7 IT devices connected and accessible
2. ✅ All 4 employee devices connected and accessible
3. ✅ Head Admin can access both servers (full control)
4. ✅ Senior Admin can access IT Server (full) and Employee Server (read-only)
5. ✅ Junior Admins can access IT Server (limited) but NOT Employee Server
6. ✅ Employees can access Employee Server but NOT IT Server
7. ✅ IT department can print to IT printers only
8. ✅ Employees can print to Employee printers only
9. ✅ Guest WiFi provides internet access but isolated from internal network
10. ✅ All devices receive IP addresses automatically (DHCP)
11. ✅ DNS resolution works for internal resources
12. ✅ Email server accessible to all internal users
13. ✅ DMZ web server accessible from internet but isolated from internal network
14. ✅ All security policies enforced via ACLs
15. ✅ Complete documentation created and stored in GitHub

---

## 13. Project Timeline

**Estimated Duration:** 3 weeks

- **Week 1:** Planning, topology design, initial configuration
- **Week 2:** Advanced configuration (ACLs, services), testing
- **Week 3:** Documentation, final testing, GitHub portfolio creation

---

## 14. Deliverables

1. Fully functional Packet Tracer network file
2. Complete network documentation (GitHub repository)
3. Configuration files for all devices
4. Network diagrams (topology, logical, VLAN)
5. Testing documentation with screenshots
6. Access control matrix
7. IP addressing scheme
8. User guide for administrators

---

## 15. Approval

**Document Status:** Draft  
**Reviewed By:** Self-review  
**Approved By:** Portfolio Project  
**Date:** December 2025

---

**END OF REQUIREMENTS DOCUMENT**