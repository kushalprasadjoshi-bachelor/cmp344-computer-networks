# Lab 2: Configure Switch and Router with CLI

## Objectives

- To create two separate LANs using switches and end devices.
- To configure static IP addresses on PCs.
- To configure management IP addresses on switches.
- To configure router interfaces to connect different networks.
- To verify communication within the same network.
- To verify communication between different networks through a router.
- To understand the role of routers in inter-network communication.

---

## Theory

A Local Area Network (LAN) consists of interconnected devices within a limited geographical area. Switches are used to connect devices within the same LAN and forward frames based on MAC addresses.

When multiple LANs exist, devices in one network cannot communicate directly with devices in another network because each network has a different network address. A router is required to connect these separate networks and forward packets between them.

In this experiment, two independent networks were created:

- Network 1: 192.192.192.0/24
- Network 2: 200.200.200.0/24

Each network contains a switch and two PCs. The router acts as the gateway between the networks by providing an interface in each network.

Static IP addresses were assigned manually to all devices. The router interfaces were configured with IP addresses belonging to their respective networks, allowing devices from different LANs to communicate through the router.

---

## Implementation

### Network Topology

```text
                Router0
          /                   \
         /                     \
  Switch0                     Switch1
   /    \                      /    \
 PC0    PC1                 PC2    PC3
```

### Addressing Table

| Device  | Interface     | IP Address     | Subnet Mask   | Default Gateway |
| ------- | ------------- | -------------- | ------------- | --------------- |
| PC0     | FastEthernet0 | 192.192.192.2  | 255.255.255.0 | 192.192.192.1   |
| PC1     | FastEthernet0 | 192.192.192.3  | 255.255.255.0 | 192.192.192.1   |
| Switch0 | VLAN 1        | 192.192.192.10 | 255.255.255.0 | 192.192.192.1   |
| Router0 | G0/0/0        | 192.192.192.1  | 255.255.255.0 | -               |
| PC2     | FastEthernet0 | 200.200.200.2  | 255.255.255.0 | 200.200.200.1   |
| PC3     | FastEthernet0 | 200.200.200.3  | 255.255.255.0 | 200.200.200.1   |
| Switch1 | VLAN 1        | 200.200.200.10 | 255.255.255.0 | 200.200.200.1   |
| Router0 | G0/0/1        | 200.200.200.1  | 255.255.255.0 | -               |

### PC Configuration

#### Network 1

**PC0:**

```text
IP Address: 192.192.192.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.192.192.1
```

**PC1:**

```text
IP Address: 192.192.192.3
Subnet Mask: 255.255.255.0
Default Gateway: 192.192.192.1
```

#### Network 2

**PC2:**

```text
IP Address: 200.200.200.2
Subnet Mask: 255.255.255.0
Default Gateway: 200.200.200.1
```

**PC3:**

```text
IP Address: 200.200.200.3
Subnet Mask: 255.255.255.0
Default Gateway: 200.200.200.1
```

### Switch Configuration

#### Switch0

```bash
enable
configure terminal

hostname Switch0

interface vlan 1
ip address 192.192.192.10 255.255.255.0
no shutdown

ip default-gateway 192.192.192.1

end
copy running-config startup-config
```

#### Switch1

```bash
enable
configure terminal

hostname Switch1

interface vlan 1
ip address 200.200.200.10 255.255.255.0
no shutdown

ip default-gateway 200.200.200.1

end
copy running-config startup-config
```

### Router Configuration

```bash
enable
configure terminal

hostname Router0

interface gigabitEthernet0/0/0
ip address 192.192.192.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet0/0/1
ip address 200.200.200.1 255.255.255.0
no shutdown
exit

end

copy running-config startup-config
```

### Connectivity Testing

#### Communication Within Network 1

**From PC0:**

```bash
ping 192.192.192.3
```

Successful replies confirm communication between PC0 and PC1.

#### Communication Within Network 2

**From PC2:**

```bash
ping 200.200.200.3
```

Successful replies confirm communication between PC2 and PC3.

#### Communication Between Networks

**From PC0:**

```bash
ping 200.200.200.2
ping 200.200.200.3
```

**From PC2:**

```bash
ping 192.192.192.2
ping 192.192.192.3
```

Successful replies confirm that the router is forwarding packets between the two networks.

---

## Discussion

Two independent LANs were created using switches and end devices. Each LAN was assigned a unique network address. Static IP addresses were manually configured on all PCs, switches, and router interfaces.

The switches enabled communication among devices within the same LAN. However, communication between different networks required a router because switches operate only at Layer 2 of the OSI model.

The router was configured with one interface in each network, allowing it to act as a gateway for both LANs. Once the default gateways were configured on the PCs, packets destined for another network were forwarded to the router, which then delivered them to the appropriate destination network.

Ping tests confirmed successful communication both within individual LANs and across different networks through the router.

---

## Conclusion

The experiment successfully demonstrated the configuration of two separate networks connected through a router. Static IP addresses were assigned to PCs, switches, and router interfaces. Communication within each LAN was verified through switches, while inter-network communication was achieved through the router. The successful ping results confirmed proper network configuration and routing between the two networks.

---

_Name: Kushal Prasad Joshi_  
_Program: BCE_  
_Roll No: 230345_
