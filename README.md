# 🔐 Cybersecurity Lab Environment Setup

<div align="center">

**Building a Virtual Cybersecurity Lab Using Kali Linux and VMware**

![Kali Linux](https://img.shields.io/badge/OS-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![VMware](https://img.shields.io/badge/Hypervisor-VMware-607078?style=for-the-badge&logo=vmware&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-7.99-4682B4?style=for-the-badge)
![Cybersecurity](https://img.shields.io/badge/Domain-Cybersecurity-red?style=for-the-badge)

</div>

---

## 📌 Project Overview

This project documents the setup and configuration of my cybersecurity laboratory using Kali Linux and VMware Workstation.

The purpose of this lab is to provide a virtual environment for learning Linux networking, network reconnaissance, vulnerability assessment, and authorized penetration testing.

The lab uses VMware virtual network adapters, including Host-only and NAT networking, to explore network configuration and communication between virtual machines.

## 🎯 Objectives

- Set up a cybersecurity lab using VMware.
- Configure Kali Linux as the security testing operating system.
- Understand Host-only and NAT network configurations.
- Identify IP addresses and network interfaces.
- Inspect routing tables and DNS configuration.
- Verify the installation of Nmap.
- Test network connectivity and troubleshoot configuration issues.
- Document the lab setup and test results.

---

## 🛠️ Lab Configuration

| Component | Configuration |
|---|---|
| Virtualization Software | VMware |
| Guest Operating System | Kali GNU/Linux Rolling |
| Allocated Memory | 6.1 GB |
| Processor Count | 4 |
| Virtual Hard Disk | 60 GB |
| Network Adapter 1 | Host-only |
| Network Adapter 2 | NAT |
| Active Ethernet Interface | `eth1` |
| Observed IPv4 Address 1 | `10.0.0.2/24` |
| Observed IPv4 Address 2 | `192.168.226.132/24` |
| Default Route | `10.0.0.1` via `eth1` |
| DNS Server 1 | `8.8.8.8` |
| DNS Server 2 | `192.168.226.2` |
| Network Tool | Nmap 7.99 |
| Target Machine | To be confirmed |

*Note: Both IPv4 addresses appeared on `eth1` in the observed output. The exact mapping between VMware adapters and Linux interfaces has not yet been verified.*

---

## 🏗️ Lab Architecture

The laboratory is configured using VMware virtual networking.

- **Kali Linux:** Security testing and network analysis machine.
- **Host-only network:** Intended for communication between the host and virtual machines on the private network.
- **NAT network:** Intended to provide outbound network connectivity through the host.
- **Target VM:** An additional machine, such as Metasploitable 2, may be added for authorized security testing.

The actual connectivity between these components must be verified before conducting testing.

---

## ⚙️ Lab Setup Procedure

### Step 1: Configure VMware

A Kali Linux virtual machine was configured in VMware with the following hardware settings:

- Memory: 6.1 GB
- Processors: 4
- Virtual disk: 60 GB
- Network Adapter 1: Host-only
- Network Adapter 2: NAT

### Step 2: Verify the Kali Linux Version

The following command was used:

```bash
grep PRETTY_NAME /etc/os-release
```

Observed result:

```text
PRETTY_NAME="Kali GNU/Linux Rolling"
```

### Step 3: Inspect Network Interfaces

The network interfaces were inspected using:

```bash
ip -4 addr
```

The output showed the following:

| Interface | Observed Configuration |
|---|---|
| `lo` | `127.0.0.1/8` |
| `eth0` | Disconnected in NetworkManager |
| `eth1` | `10.0.0.2/24` and `192.168.226.132/24` |
| `docker0` | `172.17.0.1/16`, interface down |

### Step 4: Inspect the Routing Table

The following command was executed:

```bash
ip route
```

Observed routes included:

```text
default via 10.0.0.1 dev eth1
10.0.0.0/24 dev eth1
172.17.0.0/16 dev docker0
192.168.226.0/24 dev eth1
```

These entries describe the routes configured in Kali Linux. They do not, by themselves, confirm that the destinations are reachable.

### Step 5: Check DNS Configuration

The DNS settings were inspected using:

```bash
cat /etc/resolv.conf
```

Observed nameservers:

```text
nameserver 8.8.8.8
nameserver 192.168.226.2
```

### Step 6: Verify Nmap Installation

Nmap was checked using:

```bash
nmap --version
```

Observed version:

```text
Nmap version 7.99
```

Nmap is a network discovery and security auditing tool used in authorized lab environments.

---

## 🔎 Network Connectivity Testing

Connectivity tests were performed against two configured network destinations.

### Test 1: Default Route Destination

Command:

```bash
ping -c 4 10.0.0.1
```

Observed result:

- Four packets transmitted.
- Zero packets received.
- Destination Host Unreachable messages.
- 100% packet loss.

### Test 2: Address on the 192.168.226.0/24 Network

Command:

```bash
ping -c 4 192.168.226.1
```

Observed result:

- Four packets transmitted.
- Zero packets received.
- 100% packet loss.

### Connectivity Summary

| Test | Result |
|---|---|
| Inspect IP addresses | Completed |
| Inspect routing table | Completed |
| Inspect DNS configuration | Completed |
| Verify Nmap installation | Completed |
| Ping `10.0.0.1` | Failed |
| Ping `192.168.226.1` | No replies |
| Verify VMware adapter mapping | Pending |
| Verify internet connectivity | Pending |
| Verify target VM connectivity | Pending |

The connectivity tests indicate that further investigation is required. Possible areas to inspect include VMware adapter connection status, network interface assignments, routing, and neighbor discovery.

A failed ping does not necessarily prove that a destination is unreachable, because ICMP traffic may be blocked.

---

## 🐞 Problems Encountered

### Problem 1: Network Connectivity Failure

**Observation:** The initial ping tests did not receive replies from the tested destinations.

**Troubleshooting plan:**

1. Check that the VMware network adapters are connected.
2. Verify that Host-only and NAT adapters are configured correctly.
3. Inspect Linux interface assignments.
4. Check the routing table and neighbor information.
5. Repeat connectivity tests after verifying the configuration.

**Status:** Further verification required.

### Problem 2: VMware Copy-and-Paste Integration

Copy-and-paste between Windows and Kali Linux was not working as expected.

The VMware Guest Isolation settings and VMware guest integration tools can be checked to resolve this issue.

**Status:** Further verification required.

---

## 📸 Screenshots and Evidence

The following screenshots can be added to the repository:

1. VMware virtual machine hardware configuration.
2. VMware Host-only network adapter settings.
3. VMware NAT network adapter settings.
4. Kali Linux version information.
5. Kali Linux IP address output.
6. Linux routing table.
7. DNS configuration.
8. Nmap version output.
9. Network connectivity test results.

Store screenshots in a folder named `screenshots` and reference them in this README after adding them to the repository.

Example:

```markdown
![Kali Network Configuration](screenshots/kali-network.png)
```

---

## 💡 What I Learned

Through this project, I learned how to:

1. Configure a Kali Linux virtual machine in VMware.
2. Inspect Linux network interfaces and IPv4 addresses.
3. Understand routing tables and default routes.
4. Identify DNS nameservers.
5. Verify Nmap installation.
6. Understand the intended purposes of Host-only and NAT networking.
7. Test network connectivity and document failures.
8. Troubleshoot virtual network configuration issues.
9. Maintain technical documentation for cybersecurity projects.

---

## 🔐 Security and Ethical Use

This laboratory is intended for educational purposes and authorized cybersecurity testing.

All scanning and security testing must be restricted to systems that I own or have explicit permission to assess.

No unauthorized systems should be targeted.

---

## 🔗 Tools and Resources

- **Kali Linux:** https://www.kali.org/get-kali/
- **VMware:** https://www.vmware.com/
- **Nmap:** https://nmap.org/

---

## 👤 Author

**Krrish Mukadam**

- **LinkedIn:** [Krrish Mukadam](https://www.linkedin.com/in/krrish-mukadam-473265371/)
- **Project:** Cybersecurity Lab Environment Setup
- **Platform:** VMware and Kali Linux

---

<div align="center">

**Cybersecurity Lab Environment Setup**

*Learn • Configure • Test • Document*

</div>
