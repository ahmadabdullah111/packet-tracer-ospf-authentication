# Cisco Packet Tracer: Configure Syslog, NTP, and SSH Operations

**File:** `configure-syslog-ntp-ssh.pka`

## Objective

Configure secure remote access, centralized logging, and time synchronization on Cisco routers and switches using Cisco Packet Tracer. This lab demonstrates how SSH secures device management, Syslog collects network events, and NTP synchronizes the router's clock for accurate log timestamps.

## Key Concepts

- Secure remote device access using SSH
- Local username and password authentication
- RSA key generation and SSH version 2
- Login blocking after repeated authentication failures
- Centralized Syslog server configuration
- Generating and monitoring network events
- Manually configuring switch clocks
- NTP-based time synchronization
- Timestamped system log verification

## Network Topology

- **R1:** Cisco Router
- **S1, S2:** Cisco Switches
- **Admin PC:** Console access to R1
- **PC1, PC2, L1, L2:** End devices
- **Syslog Server:** `10.0.1.254`
- **NTP Server:** `64.103.224.2`

## IP Addressing

| Device | Interface | IP Address |
|---|---|---|
| R1 | G0/1 | 10.0.1.1 |
| R1 | S0/0/0 | 209.165.14.2 |
| S1 | VLAN 1 | 10.0.1.2 |
| S2 | VLAN 1 | 10.0.2.2 |
| Syslog Server | NIC | 10.0.1.254 |
| NTP Server | NIC | 64.103.224.2 |

*Note: Verify the interface addressing against the original activity file.*

## Typical Commands

### 1. Configure SSH on R1

```cisco
ip domain-name bdren.local
username SSHuser privilege 15 secret SSHuserpass
crypto key generate rsa
ip ssh version 2

line vty 0 4
 login local
 transport input ssh
 exec-timeout 5 0
exit

login block-for 300 attempts 4 within 120
```

Generate a 1024-bit RSA key when prompted.

### 2. Configure Syslog

Apply the following configuration to R1, S1, and S2:

```cisco
configure terminal
logging 10.0.1.254
end
```

Enable the Syslog service on the server through the **Services** tab.

### 3. Generate Network Events

```cisco
configure terminal
interface loopback 0
 ip address 1.1.1.1 255.255.255.255
 shutdown
end
```

Restart PC1 and PC2 to generate additional events.

### 4. Configure Switch Timestamps

Set the clock manually on S1 and S2:

```cisco
clock set 21:00:00 Oct 11 2026
```

Enable timestamped logging:

```cisco
configure terminal
service timestamps log datetime msec
end
```

### 5. Configure NTP on R1

```cisco
configure terminal
ntp server 64.103.224.2
service timestamps log datetime msec
end
```

### 6. Verification

```cisco
show clock
show ntp status
show ntp associations
show running-config
```

Check the Syslog server to verify that events are received with timestamps.

## Expected Results

- SSH provides secure remote access to network devices.
- R1, S1, and S2 forward log events to the Syslog server.
- Switches use manually configured clocks.
- R1 synchronizes its clock with the NTP server.
- New log messages contain timestamps for easier troubleshooting and monitoring.

## How to Open

Install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer), then open the `.pka` activity file. Follow the activity instructions to complete the configuration.

## Author

**Md Abdullah Talukdar**  
Sub Assistant Engineer (Core)  
Bangladesh Research and Education Network (BdREN), Dhaka

*Shared for learning, networking practice, and portfolio purposes.*
