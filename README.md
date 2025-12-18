# 🛡️ Network Security Simulation with Mininet

## 📋 Overview

This project implements a complete network topology with **internal and external segments**, a **custom firewall using iptables**, **DNS-based content filtering**, and **automated security testing**.

It is designed for **educational purposes**, demonstrating key **network security concepts**, including:

- Firewall configuration and packet filtering  
- DNS-based content blocking  
- Network segmentation  
- NAT and routing  
- Traffic monitoring with `tcpdump`  

---

## 🏗️ Network Topology
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│ INTERNAL │ │ │ │ EXTERNAL │
│ NETWORK │ │ FIREWALL │ │ NETWORK │
│ 10.0.0.0/28 │ │ (fw) │ │192.168.5.0/24│
│ │ │ │ │ │
│ h1 h2 h3 │ │ fw-eth0 fw-eth1 │ │ h5 h6 │
│ │ │ │ │ │ │ │ │ │ │
│ └──┴──┴─┘ └───┼────┼─────┘ └───┴─────┴──┘
│ │ │ │
│ ┌──┴───────────┐ └───────────────┐
│ │ Switch s1 │ │
│ └──────────────┘ │
│ │
│ ┌───────┴──────┐
│ │ Switch s2 │
│ └───────┬──────┘
│ │
│ ┌───────┴──────┐
│ │ veth-pair │
│ │ (connection │
│ │ to physical │
│ │ host) │
│ └───────┬──────┘
│ │
│ ┌───────┴──────┐
│ │ PHYSICAL │
│ │ HOST │
│ │ (Internet) │
│ └──────────────┘
---

## ✨ Features

- **Complete Network Segmentation**
  - Internal: `10.0.0.0/28`
  - External: `192.168.5.0/24`

- **Custom Firewall**
  - Stateful `iptables` rules
  - Separate INPUT, FORWARD, and NAT policies

- **DNS Content Filtering**
  - Blocks YouTube and Instagram via DNS poisoning

- **Internet Connectivity**
  - NAT masquerading for outbound access

- **Automated Testing**
  - End-to-end validation of firewall and DNS rules

- **Traffic Monitoring**
  - Packet captures with `tcpdump` on firewall interfaces

- **Safe Cleanup**
  - Restores host system configuration after execution

---

## 🚀 Quick Start

### Prerequisites

```bash
# Install Mininet
sudo apt-get update
sudo apt-get install mininet

# Install required Python packages
pip install mininet

# Ensure Python 3.x is installed
python3 --version
```

### Configuration

Before running, update the physical network interface in the script:

Line 9 in the script:
PHYSICAL_IF = "wlp0s20f3"  # Change to your interface (e.g., eth0, enp0s3)
```
ip link show
# or
ifconfig
```
### Running the Simulation

```
# Clone the repository
git clone <your-repo-url>
cd <repository-directory>

# Run with sudo (required for network operations)
sudo python3 topo.py
```
## 📊 Firewall Rules

**INPUT Chain**
Accept localhost traffic
Accept established/related connections
Accept ICMP echo requests
Accept DNS and SSH from internal network

**FORWARD Chain**
Allow ICMP within internal network
Block ICMP from internal to external
Allow HTTP/HTTPS from internal to external
Allow DNS from internal to external
Accept established connections

**NAT**

Masquerading for internal network traffic

## 🚫 DNS Blocking

### Blocked domains (resolved to 127.0.0.1):

-youtube.com
-www.youtube.com
-instagram.com
-www.instagram.com

## 📁 Files Generated

### During execution, the following files are created:

fw-eth0.pcap – Firewall internal interface capture
fw-eth1.pcap – Firewall external interface capture
/tmp/dnsmasq-fw.log – DNS server logs

Temporary configuration files in /tmp/

## 🛠️ Manual Testing

### After automated tests, the Mininet CLI is available:

mininet> h1 ping h2
mininet> h1 ping h5
mininet> h1 curl http://192.168.5.20:80
mininet> h1 nslookup youtube.com 10.0.0.1

## 🧹 Cleanup Process
### The script automatically:
Stops tcpdump and dnsmasq
Removes virtual interfaces
Deletes custom iptables chains
Removes temporary files
Restores original IP forwarding settings

## ⚠️ Troubleshooting
### Common Issues:

#### Interface not found
- Update PHYSICAL_IF with the correct interface name
#### No internet access
- Ensure host has internet connectivity
- Check host firewall rules
#### Permission errors
- Run with sudo
#### Mininet not installed
- sudo apt-get install mininet
#### Debugging
- Enable verbose logging:
```
if __name__ == '__main__':
    setLogLevel('debug')  # Change from 'info' to 'debug'
    setup_network()
```

📚 Learning Resources

Mininet Documentation
iptables Tutorial
DNS Masquerading with dnsmasq
Network Security Fundamentals

👥 Contributing

Issues, suggestions, and enhancements are welcome!

📄 License

This project is intended for educational purposes only.
Use responsibly.
