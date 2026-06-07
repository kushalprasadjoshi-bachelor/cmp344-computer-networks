# Lab 1: LAN Implementation Using Switches and End Devices with Static IP Configuration

## Objective

To design and implement a **Local Area Network (LAN)** using a switch and multiple end devices, and configure **static IP addresses** on the end devices through the **FastEthernet0 (Ethernet0)** interface to enable communication within the network.

---

## Theory

### What is a LAN?

A **Local Area Network (LAN)** is a network that connects computers and other devices within a limited geographical area such as:

- Home
- Office
- Laboratory
- School
- Building

LANs allow devices to:

- Share files
- Share printers
- Access common resources
- Communicate with each other

A LAN typically operates at high speed and is managed by a single organization.

### What is a Switch?

A **switch** is a networking device that connects multiple devices within a LAN.

#### Functions of a Switch

- Receives data frames from devices.
- Learns MAC addresses of connected devices.
- Forwards frames only to the intended destination port.
- Reduces network traffic.
- Improves network performance.

#### Working Principle

When a device sends data:

1. The switch examines the source MAC address.
2. Stores it in the MAC Address Table.
3. Checks the destination MAC address.
4. Sends the frame only through the appropriate port.

This process is called **frame switching**.

### End Devices

End devices are the devices used by users in the network.

Examples:

- Personal Computers (PCs)
- Laptops
- Servers
- Printers
- Smartphones

In Cisco Packet Tracer, PCs are commonly used as end devices for LAN implementation.

### Ethernet Interface

Each PC contains a network interface card (NIC).

The interface is represented as:

```text
FastEthernet0
```

or

```text
Ethernet0
```

This interface allows the device to connect to the switch using an Ethernet cable.

## IP Addressing

### What is an IP Address?

An IP (Internet Protocol) address is a logical address assigned to a device in a network.

It uniquely identifies the device.

Example:

```text
192.168.1.10
```

### IPv4 Address Structure

IPv4 consists of 32 bits divided into four octets.

Example:

```text
192.168.1.10
```

| Octet 1 | Octet 2 | Octet 3 | Octet 4 |
| ------- | ------- | ------- | ------- |
| 192     | 168     | 1       | 10      |

### Static IP Address

A **Static IP Address** is manually assigned by the network administrator.

#### Advantages

- Fixed and predictable.
- Easy device identification.
- Suitable for servers and laboratory setups.
- No dependency on DHCP.

#### Example

| Device | IP Address  |
| ------ | ----------- |
| PC0    | 192.168.1.1 |
| PC1    | 192.168.1.2 |
| PC2    | 192.168.1.3 |

## Subnet Mask

A subnet mask identifies the network and host portions of an IP address.

For a Class C network:

```text
255.255.255.0
```

This means:

```text
Network Portion : 192.168.1
Host Portion    : X
```

All devices must belong to the same network for direct communication.

## Network Topology

### Star Topology

In a switched LAN, devices are connected in a star formation.

```text
        PC0
         |
         |
PC1 --- Switch --- PC2
         |
         |
        PC3
```

#### Advantages:

- Easy troubleshooting
- Reliable communication
- Easy expansion
- Centralized management

### Devices Required

| Device                        | Quantity    |
| ----------------------------- | ----------- |
| Switch 2960                   | 1           |
| PCs                           | 3–4         |
| Copper Straight-Through Cable | As required |

_What if you want to have many number of devices connnected?_

## Role of Multiple Switches in a LAN

A LAN is not limited to a single switch. In larger networks, multiple switches are interconnected to extend the network and accommodate more end devices.

When multiple switches are connected:

- Devices connected to different switches can still communicate.
- The LAN can cover a larger area such as multiple rooms, floors, or departments.
- Additional devices can be added without replacing the existing switch.
- Network management becomes more scalable.

### Example

```text
PC0 ----- Switch0 ----- Switch1 ----- PC2
             |              |
            PC1            PC3
```

In this topology, all PCs belong to the same LAN and can communicate with each other if they are configured with IP addresses from the same network.

### Connecting Switches Together

To interconnect switches in Cisco Packet Tracer:

1. Select **Connections**.
2. Choose **Copper Cross-Over Cable** (or **Automatic Connection** in newer versions).
3. Connect a FastEthernet port of one switch to a FastEthernet port of another switch. (You can also use GigabitEthernet.)

Example:

```text
Switch0 FastEthernet0/24  →  Switch1 FastEthernet0/24
```

The link LEDs will turn green once the connection is established.

### IP Configuration When Using Multiple Switches

Switches operate at the Data Link Layer (Layer 2) and do not require IP addresses for forwarding traffic between end devices.

Only the PCs need IP configuration.

Example:

| Device | IP Address  | Subnet Mask   |
| ------ | ----------- | ------------- |
| PC0    | 192.168.1.1 | 255.255.255.0 |
| PC1    | 192.168.1.2 | 255.255.255.0 |
| PC2    | 192.168.1.3 | 255.255.255.0 |
| PC3    | 192.168.1.4 | 255.255.255.0 |

Even if PC0 and PC3 are connected to different switches, they can communicate because the switches forward frames between them.

### Verification Across Switches

To verify inter-switch communication:

#### From PC0:

```bash
ping 192.168.1.4
```

If replies are received, it confirms that communication is successfully occurring through multiple interconnected switches within the same LAN.

---

## Implementation in Cisco Packet Tracer

### Step 1: Add Devices

From the device menu:

1. Drag one Switch (2960) into the workspace.
2. Drag multiple PCs into the workspace.

Example:

```text
Switch0
PC0
PC1
PC2
PC3
```

---

### Step 2: Connect Devices

Choose:

```text
Connections
→ Copper Straight-Through
```

Connect:

```text
PC0 FastEthernet0 → Switch FastEthernet0/1

PC1 FastEthernet0 → Switch FastEthernet0/2

PC2 FastEthernet0 → Switch FastEthernet0/3

PC3 FastEthernet0 → Switch FastEthernet0/4
```

After a few seconds:

```text
Green Light = Successful Connection
```

### Step 3: Configure Static IP Address

#### Configure PC0

1. Click PC0.
2. Select:

```text
Desktop
→ IP Configuration
→ fastethernet0
```

Enter:

```text
IP Address: 192.168.1.1
Subnet Mask: 255.255.255.0
```

You can only assign IP this time and subnet mask is automatically detected.

Simillary, you can configure IP addresses for other PCs as well.

#### Configure PC1

```text
IP Address: 192.168.1.2
Subnet Mask: 255.255.255.0
```

#### Configure PC2

```text
IP Address: 192.168.1.3
Subnet Mask: 255.255.255.0
```

#### Configure PC3

```text
IP Address: 192.168.1.4
Subnet Mask: 255.255.255.0
```

### Addressing Table

| Device | Interface     | IP Address  | Subnet Mask   |
| ------ | ------------- | ----------- | ------------- |
| PC0    | FastEthernet0 | 192.168.1.1 | 255.255.255.0 |
| PC1    | FastEthernet0 | 192.168.1.2 | 255.255.255.0 |
| PC2    | FastEthernet0 | 192.168.1.3 | 255.255.255.0 |
| PC3    | FastEthernet0 | 192.168.1.4 | 255.255.255.0 |

### Verification

#### Using Ping Command

##### From PC0

Open:

```text
Desktop
→ Command Prompt
```

Type:

```bash
ping 192.168.1.2
```

Expected Output:

```text
Reply from 192.168.1.2
```

##### Ping PC2

```bash
ping 192.168.1.3
```

Expected Output:

```text
Reply from 192.168.1.3
```

##### From PC1

```bash
ping 192.168.1.1
```

Expected Output:

```text
Reply from 192.168.1.1
```

Successful replies indicate that all devices can communicate through the switch.

**A LAN with multiple switches can also be created easily as explained in the theory.**

---

## Discussion

### How Communication Occurs

1. PC0 wants to send data to PC1.
2. PC0 checks if PC1 is in the same network.
3. PC0 sends an ARP request to find PC1's MAC address.
4. PC1 replies with its MAC address.
5. PC0 sends the frame.
6. The switch forwards the frame to the correct port.
7. PC1 receives the data.

Thus, communication is established within the LAN.

---

## Conclusion

A LAN can be implemented by connecting multiple end devices to a switch using Ethernet cables. Each device is assigned a static IP address through its **FastEthernet0/Ethernet0** interface. When all devices are configured within the same IP network and subnet mask, they can communicate successfully using the switch. This setup forms the foundation of modern computer networking and is one of the most fundamental experiments in Cisco Packet Tracer.

---

_Name: Kushal Prasad Joshi_  
_Program: BCE_  
_Roll No: 230345_
