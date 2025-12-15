\# Lab 01 - Configure NAT for IPv4



\## 🎯 Lab Objectives



By completing this lab, I demonstrated the ability to:

\- ✅ Configure Static NAT (one-to-one translation)

\- ✅ Configure Dynamic NAT with PAT/Overload (many-to-one translation)

\- ✅ Define inside and outside NAT interfaces

\- ✅ Create and apply access control lists for NAT

\- ✅ Verify NAT translations and statistics

\- ✅ Troubleshoot NAT connectivity issues



\*\*Difficulty:\*\* ⭐⭐⭐ Intermediate

\*\*Time Required:\*\* 2-3 hours

\*\*CCNA Topics Covered:\*\* NAT/PAT configuration, public/private addressing, ACLs



---



\## 🖼️ Network Topology



!\[Lab Topology](topology.png)



\### Device List:

\- \*\*R1\*\* - Edge Router (ISR4331) - Performs NAT translation

\- \*\*R2\*\* - ISP Router (ISR4331) - Simulates Internet connection

\- \*\*S2\*\* - Inside Switch (2960-24TT) - Connects internal network

\- \*\*S1\*\* - Outside Switch (2960-24TT) - Connects to ISP

\- \*\*PC-B\*\* - Inside Host - Uses private IP addressing

\- \*\*PC-A\*\* - Outside Host - Simulates Internet server



---



\## 📋 IP Addressing Scheme



\### Inside Network (Private - RFC 1918)

| Device | Interface | IP Address | Subnet Mask | Description |

|--------|-----------|------------|-------------|-------------|

| R1 | G0/0/1 (Inside) | 192.168.1.1 | 255.255.255.0 | Default gateway for inside network |

| PC-B | NIC | 192.168.1.2 | 255.255.255.0 | Inside host (private IP) |



\### Outside Network (Public)

| Device | Interface | IP Address | Subnet Mask | Description |

|--------|-----------|------------|-------------|-------------|

| R1 | G0/0/0 (Outside) | 209.165.200.230 | 255.255.255.248 | NAT outside interface |

| R2 | G0/0/0 | 209.165.200.225 | 255.255.255.248 | ISP router (simulated Internet) |

| PC-A | NIC | 209.165.200.226 | 255.255.255.248 | Outside host/server |



\### NAT Translation Pool

| Inside Local (Private) | Inside Global (Public) | Type |

|------------------------|------------------------|------|

| 192.168.1.2 | 209.165.200.229 | Static NAT |

| 192.168.1.0/24 | 209.165.200.230 | PAT (Overload) |



---



\## 🛠️ NAT Configuration Implemented



\### What is NAT?



\*\*Network Address Translation (NAT)\*\* allows private IP addresses (192.168.x.x) to communicate with the public Internet by translating them to public IP addresses. This lab implements two types:



1\. \*\*Static NAT\*\* - One private IP permanently mapped to one public IP

2\. \*\*PAT (Port Address Translation / NAT Overload)\*\* - Multiple private IPs share one public IP using port numbers



---



\## ⚙️ Configuration Steps



\### Step 1: Configure Router Interfaces



\*\*R1 Inside Interface (connects to private network):\*\*

```cisco

R1(config)# interface GigabitEthernet0/0/1

R1(config-if)# ip address 192.168.1.1 255.255.255.0

R1(config-if)# ip nat inside

R1(config-if)# no shutdown

```



\*\*R1 Outside Interface (connects to Internet/ISP):\*\*

```cisco

R1(config)# interface GigabitEthernet0/0/0

R1(config-if)# ip address 209.165.200.230 255.255.255.248

R1(config-if)# ip nat outside

R1(config-if)# no shutdown

```



---



\### Step 2: Configure Static NAT



Maps inside host 192.168.1.2 to public IP 209.165.200.229:



```cisco

R1(config)# ip nat inside source static 192.168.1.2 209.165.200.229

```



\*\*Purpose:\*\* Allows PC-B to be reachable from the Internet using a permanent public IP (useful for servers).



---



\### Step 3: Configure Dynamic NAT with PAT (Overload)



\*\*Create Access List to identify inside network:\*\*

```cisco

R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

```



\*\*Configure NAT Overload:\*\*

```cisco

R1(config)# ip nat inside source list 1 interface GigabitEthernet0/0/0 overload

```



\*\*Purpose:\*\* Allows all inside hosts (192.168.1.x) to share the single outside IP (209.165.200.230) using different port numbers.



---



\### Step 4: Configure Default Route



Router needs to know how to reach the Internet:



```cisco

R1(config)# ip route 0.0.0.0 0.0.0.0 209.165.200.225

```



This sends all unknown traffic to R2 (ISP router).



---



\### Step 5: Basic Security Configuration



```cisco

R1(config)# enable secret class

R1(config)# line console 0

R1(config-line)# password cisco

R1(config-line)# login

R1(config-line)# line vty 0 4

R1(config-line)# password cisco

R1(config-line)# login

R1(config)# banner motd ^C Authorized Users Only! ^C

R1(config)# service password-encryption

R1(config)# no ip domain-lookup

```



---



\## ✅ Verification \& Testing



\### Verify NAT Configuration



\*\*Check NAT is enabled on interfaces:\*\*

```cisco

R1# show ip interface brief

```



Expected output shows:

\- G0/0/1: 192.168.1.1 (inside)

\- G0/0/0: 209.165.200.230 (outside)



!\[R1 Interfaces](screenshots/R1-interfaces.png)



---



\### Verify NAT Statistics



```cisco

R1# show ip nat statistics

```



Expected output:

```

Total translations: 1 (1 static, 0 dynamic)

Outside interfaces: GigabitEthernet0/0/0

Inside interfaces: GigabitEthernet0/0/1

Hits: \\\[increases with traffic]

Misses: 0

```



!\[NAT Statistics](screenshots/show-ip-nat-statistics.png)



---



\### Test Connectivity from Inside Network



\*\*From PC-B, ping the outside network:\*\*

```

C:\\\\> ping 209.165.200.225

```



Expected: ✅ Successful replies



!\[Ping Test](screenshots/ping-test.png)



---



\### Verify NAT Translations Table



\*\*After generating traffic, check active translations:\*\*

```cisco

R1# show ip nat translations

```



Expected output:

```

Pro  Inside global      Inside local       Outside local      Outside global

icmp 209.165.200.230:1 192.168.1.2:1      209.165.200.225:1  209.165.200.225:1

---  209.165.200.229    192.168.1.2        ---                ---

```



\*\*Explanation:\*\*

\- First line: PAT translation (dynamic) - PC-B using port 1

\- Second line: Static NAT entry - permanent mapping for PC-B



!\[NAT Translations](screenshots/show-ip-nat-translations.png)



---



\### Verify Access List



```cisco

R1# show access-lists

```



Expected:

```

Standard IP access list 1

\&nbsp;   permit 192.168.1.0 0.0.0.255 (X matches)

```



---



\## 🐛 Troubleshooting



\### Issue 1: NAT Translations Not Appearing



\*\*Symptoms:\*\* `show ip nat translations` is empty



\*\*Causes:\*\*

\- No traffic has been generated yet

\- NAT not configured on interfaces

\- Access list blocking traffic



\*\*Solution:\*\*

```cisco

! Verify interfaces are marked inside/outside

R1# show ip interface GigabitEthernet0/0/1 | include NAT

R1# show ip interface GigabitEthernet0/0/0 | include NAT



! Generate traffic from inside

PC-B> ping 209.165.200.225



! Check translations again

R1# show ip nat translations

```



---



\### Issue 2: Inside Hosts Can't Reach Outside



\*\*Symptoms:\*\* Ping from PC-B to 209.165.200.225 fails



\*\*Troubleshooting Steps:\*\*



1\. \*\*Verify PC-B can reach its gateway:\*\*

```

PC-B> ping 192.168.1.1

```

Should succeed ✅



2\. \*\*Verify R1 can reach ISP router:\*\*

```

R1# ping 209.165.200.225

```

Should succeed ✅



3\. \*\*Check default route:\*\*

```

R1# show ip route

```

Should show: `S\\\* 0.0.0.0/0 \\\[1/0] via 209.165.200.225`



4\. \*\*Verify NAT access list:\*\*

```

R1# show access-lists

```

Should permit 192.168.1.0/24



---



\### Issue 3: Static NAT Not Working



\*\*Symptoms:\*\* Can't reach PC-B from outside using 209.165.200.229



\*\*Solution:\*\*

```cisco

! Verify static NAT entry exists

R1# show ip nat translations

! Should show: --- 209.165.200.229  192.168.1.2  ---  ---



! If missing, reconfigure:

R1(config)# ip nat inside source static 192.168.1.2 209.165.200.229

```



---



\## 💡 Key Concepts Learned



\### 1. Types of NAT



\*\*Static NAT (One-to-One):\*\*

\- Permanent mapping: 192.168.1.2 ↔ 209.165.200.229

\- Used for servers that need consistent public IP

\- Configured: `ip nat inside source static <local> <global>`



\*\*Dynamic NAT (One-to-One from pool):\*\*

\- Temporary mappings from a pool of public IPs

\- Not used in this lab



\*\*PAT / NAT Overload (Many-to-One):\*\*

\- Multiple inside hosts share one public IP

\- Uses port numbers to differentiate connections

\- Configured: `ip nat inside source list <ACL> interface <outside-if> overload`



---



\### 2. NAT Terminology



| Term | Definition | Example |

|------|------------|---------|

| \*\*Inside Local\*\* | Private IP of inside host | 192.168.1.2 |

| \*\*Inside Global\*\* | Public IP representing inside host | 209.165.200.229 |

| \*\*Outside Local\*\* | IP of outside host (as seen from inside) | 209.165.200.225 |

| \*\*Outside Global\*\* | Public IP of outside host | 209.165.200.225 |



---



\### 3. NAT Security Benefits



\- \*\*IP Address Conservation\*\* - Many devices share few public IPs

\- \*\*Internal Network Hiding\*\* - Outside users can't see internal topology

\- \*\*Flexible Internal Addressing\*\* - Can use any private addressing scheme



---



\### 4. NAT Limitations



\- \*\*Breaks End-to-End Connectivity\*\* - Some protocols have issues with NAT

\- \*\*Performance Impact\*\* - Router must track and translate every connection

\- \*\*Troubleshooting Complexity\*\* - Harder to trace packets through translations



---



\## 🎯 Real-World Applications



This lab simulates common enterprise and home network scenarios:



\*\*Scenario 1: Small Business\*\*

\- Inside network: 192.168.1.0/24 (50 employees)

\- ISP provides: 1 public IP (209.165.200.230)

\- PAT allows all employees to share that single public IP



\*\*Scenario 2: Web Server Hosting\*\*

\- Company hosts web server internally (192.168.1.2)

\- Static NAT maps it to public IP (209.165.200.229)

\- Internet users access server via public IP



\*\*Scenario 3: Home Network\*\*

\- Your home router performs NAT

\- All your devices (phones, laptops, IoT) use private IPs (192.168.x.x)

\- ISP gives you 1 public IP

\- Router uses PAT so all devices can access Internet



---



\## 📁 Files in This Directory



\- `README.md` - This documentation

\- `topology.png` - Network diagram

\- `NAT-Lab.pkt` - Packet Tracer file (working configuration)

\- `configs/R1-config.txt` - Complete R1 configuration

\- `configs/R2-config.txt` - Complete R2 configuration

\- `configs/S1-config.txt` - Switch 1 configuration

\- `configs/S2-config.txt` - Switch 2 configuration

\- `screenshots/R1-interfaces.png` - Interface verification

\- `screenshots/show-ip-nat-statistics.png` - NAT statistics

\- `screenshots/show-ip-nat-translations.png` - Active translations

\- `screenshots/ping-test.png` - Connectivity test

\- `screenshots/PC-B-IP-config.png` - Inside host IP configuration



---



\## 📚 Additional Resources



\- \[Cisco NAT Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr\_nat/configuration/xe-16/nat-xe-16-book/iadnat-addr-consv.html)

\- \[RFC 1918 - Private Address Space](https://datatracker.ietf.org/doc/html/rfc1918)

\- \[RFC 3022 - Traditional NAT](https://datatracker.ietf.org/doc/html/rfc3022)

\- \[Understanding NAT Types](https://www.cisco.com/c/en/us/support/docs/ip/network-address-translation-nat/26704-nat-faq-00.html)



---



\## 📝 Lab Notes



\*\*Completed:\*\* December 2025

\*\*Time Invested:\*\* 2.5 hours

\*\*Chapter:\*\* 11 - Network Address Translation

\*\*Course:\*\* CCNA 200-301



\*\*Challenges Faced:\*\*

\- Initially, NAT translations table was empty until traffic was generated

\- Learned that translations are dynamic and only appear when connections are active

\- Understanding the difference between static NAT and PAT was key



\*\*What I Learned:\*\*

\- NAT is essential for IPv4 address conservation

\- PAT allows thousands of devices to share one public IP

\- Static NAT is necessary for hosting services

\- Access lists control which traffic gets translated

\- NAT impacts troubleshooting and requires understanding of inside/outside terminology



---



\## 🔗 Related Labs



\- \[Lab 02 - VLAN Configuration](../02-VLAN-Configuration/) (coming soon)

\- \[Lab 03 - Access Control Lists](../03-ACL-Security/) (coming soon)

\- \[Lab 04 - DHCP Configuration](../04-DHCP-Configuration/) (coming soon)



---



\## 📧 Contact



\*\*Moustafa Elnobi Mohamed\*\*

📧 Email: elnobimostafa@gmail.com

💼 LinkedIn: https://linkedin.com/in/moustafa-elnobi-mohamed-852ab725a



---



\*\*⭐ This lab demonstrates practical understanding of NAT configuration and troubleshooting - a critical skill for network engineering roles.\*\*


