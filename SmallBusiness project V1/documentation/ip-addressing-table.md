# IP Addressing Scheme

## VLAN Gateways (SW-CORE SVIs)
| VLAN | Network | Gateway | Subnet Mask |
|------|---------|---------|-------------|
| 10 | 192.168.10.0/24 | 192.168.10.1 | 255.255.255.0 |
| 20 | 192.168.20.0/24 | 192.168.20.1 | 255.255.255.0 |
| 30 | 192.168.30.0/24 | 192.168.30.1 | 255.255.255.0 |
| 40 | 192.168.40.0/24 | 192.168.40.1 | 255.255.255.0 |
| 50 | 192.168.50.0/24 | 192.168.50.1 | 255.255.255.0 |
| 60 | 192.168.60.0/24 | 192.168.60.1 | 255.255.255.0 |
| 99 | 192.168.99.0/24 | 192.168.99.1 | 255.255.255.0 |

## Servers (VLAN 30)
| Device | IP | Gateway | DNS | Type |
|--------|-----|---------|-----|------|
| IT-Server | 192.168.30.10 | 192.168.30.1 | 192.168.30.21 | Static |
| Employee-Server | 192.168.30.11 | 192.168.30.1 | 192.168.30.21 | Static |
| DHCP-Server | 192.168.30.20 | 192.168.30.1 | 192.168.30.21 | Static |
| DNS-Server | 192.168.30.21 | 192.168.30.1 | 192.168.30.21 | Static |
| Email-Server | 192.168.30.22 | 192.168.30.1 | 192.168.30.21 | Static |
| Backup-Server | 192.168.30.23 | 192.168.30.1 | 192.168.30.21 | Static |

## DMZ (VLAN 60)
| Device | IP | Gateway | Type |
|--------|-----|---------|------|
| Web-Server | 192.168.60.10 | 192.168.60.1 | Static |

## IT Department (VLAN 10)
| Device | IP | Gateway | DNS | Type |
|--------|-----|---------|-----|------|
| IT-PC-1 | 192.168.10.11 | 192.168.10.1 | 192.168.30.21 | DHCP |
| IT-PC-2 | 192.168.10.12 | 192.168.10.1 | 192.168.30.21 | DHCP |
| IT-PC-3 | 192.168.10.13 | 192.168.10.1 | 192.168.30.21 | DHCP |
| IT-PC-4 | 192.168.10.14 | 192.168.10.1 | 192.168.30.21 | DHCP |
| IT-Laptop-Head | 192.168.10.21 | 192.168.10.1 | 192.168.30.21 | DHCP |
| IT-Laptop-Senior | 192.168.10.22 | 192.168.10.1 | 192.168.30.21 | DHCP |

## Employee Department (VLAN 20)
| Device | IP | Gateway | DNS | Type |
|--------|-----|---------|-----|------|
| EMP-PC-1 | 192.168.20.11 | 192.168.20.1 | 192.168.30.21 | DHCP |
| EMP-PC-2 | 192.168.20.12 | 192.168.20.1 | 192.168.30.21 | DHCP |
| EMP-PC-3 | 192.168.20.13 | 192.168.20.1 | 192.168.30.21 | DHCP |
| EMP-PC-4 | 192.168.20.14 | 192.168.20.1 | 192.168.30.21 | DHCP |

## Printers (VLAN 40)
| Device | IP | Gateway | Type |
|--------|-----|---------|------|
| IT-Printer-1 | 192.168.40.11 | 192.168.40.1 | Static |
| IT-Printer-2 | 192.168.40.12 | 192.168.40.1 | Static |
| EMP-Printer-1 | 192.168.40.21 | 192.168.40.1 | Static |
| EMP-Printer-2 | 192.168.40.22 | 192.168.40.1 | Static |

## Management (VLAN 99)
| Device | IP | Purpose |
|--------|-----|---------|
| SW-CORE | 192.168.99.1 | Core Switch Management |
| SW-IT | 192.168.99.2 | IT Switch Management |
| SW-EMP | 192.168.99.3 | Employee Switch Management |
| RT-EDGE | 192.168.99.254 | Router Management |
```

---

## FINAL FOLDER STRUCTURE
```
Small-Business-Network-Design/
├── README.md ← (Use the one I gave you earlier)
├── documentation/
│   ├── network-requirements.md
│   ├── ip-addressing-table.md ← (Create this)
│   └── troubleshooting-notes.md
├── configurations/
│   ├── RT-EDGE-config.txt
│   ├── SW-CORE-config.txt
│   ├── SW-IT-config.txt
│   └── SW-EMP-config.txt
├── diagrams/
│   ├── network-topology.png ← (Screenshot your PT workspace)
│   └── vlan-logical-diagram.png (optional)
├── testing/
│   ├── test-results.md ← (Use the artifact above)
│   └── screenshots/
│       ├── topology/
│       ├── switch-configs/
│       ├── connectivity-tests/
│       ├── dhcp-verification/
│       └── acl-verification/
└── packet-tracer-files/
    └── SmallBusiness-FINAL-Complete.pkt