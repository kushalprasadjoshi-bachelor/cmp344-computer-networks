# Lab 3: Creation and Configuration of Virtual Local Area Networks (VLANs) Using a Switch

## Objectives

- To understand the concept of Virtual Local Area Networks (VLANs).
- To create multiple VLANs on a switch.
- To assign switch ports to specific VLANs.
- To configure end devices with static IP addresses.
- To verify communication within the same VLAN.
- To observe communication restrictions between different VLANs.

---

## Theory

A Virtual Local Area Network (VLAN) is a logical segmentation of a switched network. VLANs divide a single physical network into multiple logical networks, allowing devices to communicate as if they were connected to separate switches.

Devices belonging to the same VLAN can communicate directly, whereas devices in different VLANs cannot communicate without the assistance of a Layer 3 device such as a router.

Benefits of VLANs include:

- Improved network security.
- Reduced broadcast traffic.
- Better network management.
- Logical grouping of users regardless of physical location.
- Increased network efficiency.

In Cisco switches, VLANs are created using the VLAN database and switch ports are assigned to the desired VLAN. Each VLAN acts as an independent broadcast domain.

In this experiment, three VLANs were created:

| VLAN ID | VLAN Name | Devices      |
| ------- | --------- | ------------ |
| 10      | HR        | PC0, Laptop0 |
| 20      | IT        | PC1, Laptop1 |
| 30      | Admin     | PC2, Laptop2 |

If you are doing **Inter-VLAN Routing (Router-on-a-Stick)**, the router interface connected to the switch must be divided into subinterfaces, with one subinterface per VLAN.

Assuming:

| VLAN ID | Network         | Gateway      |
| ------- | --------------- | ------------ |
|     10  | 192.168.10.0/24 | 192.168.10.1 |
|     20  | 192.168.20.0/24 | 192.168.20.1 |
|     30  | 192.168.30.0/24 | 192.168.30.1 |


---

## Implementation

### VLAN Assignment

**VLAN 10:**

- PC0
- Laptop0

**VLAN 20:**

- PC1
- Laptop1

**VLAN 30:**

- PC2
- Laptop2

### IP Addressing Scheme

**VLAN 10:**

| Device  | IP Address   |
| ------- | ------------ |
| PC0     | 192.168.10.2 |
| Laptop0 | 192.168.10.3 |

**VLAN 20:**

| Device  | IP Address   |
| ------- | ------------ |
| PC1     | 192.168.20.2 |
| Laptop1 | 192.168.20.3 |

**VLAN 30:**

| Device  | IP Address   |
| ------- | ------------ |
| PC2     | 192.168.30.2 |
| Laptop2 | 192.168.30.3 |

Subnet Mask for all devices:

```text
255.255.255.0
```

### Switch Configuration

#### Create VLANs

```bash
enable

configure terminal

vlan 10
name HR

vlan 20
name IT

vlan 30
name Admin

exit
```

#### Assign Ports to VLAN 10

```bash
interface range fa0/1 - 2 # interface fa0/1 to select single port

switchport mode access
switchport access vlan 10

exit
```

### Assign Ports to VLAN 20

```bash
interface range fa0/3 - 4

switchport mode access
switchport access vlan 20

exit
```

### Assign Ports to VLAN 30

```bash
interface range fa0/5 - 6

switchport mode access
switchport access vlan 30

exit
```

### Save Configuration

```bash
copy running-config startup-config
```

### Verification

Display VLAN information:

```bash
show vlan brief
```

Expected output should display VLAN 10, VLAN 20, and VLAN 30 along with their assigned ports.

### Connectivity Testing

#### Within VLAN 10

From PC0:

```bash
ping 192.168.10.3
```

Successful reply expected.

#### Within VLAN 20

From PC1:

```bash
ping 192.168.20.3
```

Successful reply expected.

#### Within VLAN 30

From PC2:

```bash
ping 192.168.30.3
```

Successful reply expected.

#### Between Different VLANs

**Example:** From PC0.

```bash
ping 192.168.20.2
```

**Result:**

```text
Request Timed Out
```

Because communication between different VLANs is not possible without routing.

**So, we need to configure the router as well.**

### Router Configuration

```bash
enable

configure terminal

hostname Router0

interface g0/0/0
no shutdown
exit

interface g0/0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface g0/0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface g0/0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

end

copy running-config startup-config
```

### Also configure the switch port connected to the router as a trunk

For example, if the router is connected to `Fa0/24`:

```bash
interface fa0/24
switchport mode trunk
exit
```

### Default Gateways on PCs

VLAN 10 devices:

```text
192.168.10.1
```

VLAN 20 devices:

```text
192.168.20.1
```

VLAN 30 devices:

```text
192.168.30.1
```

### Verification

```bash
show ip interface brief
```

You should see:

```text
Gig0/0/0       unassigned     up   up
Gig0/0/0.10    192.168.10.1   up   up
Gig0/0/0.20    192.168.20.1   up   up
Gig0/0/0.30    192.168.30.1   up   up
```

After this, devices in VLAN 10, VLAN 20, and VLAN 30 will be able to communicate with each other through the router.



---

# Discussion

Three VLANs were successfully created on a single switch. Ports were assigned to VLANs according to the required grouping of devices. Devices belonging to the same VLAN communicated successfully because they were part of the same broadcast domain.

Communication between different VLANs was unsuccessful because switches operate at Layer 2 and do not route packets between VLANs. A router or Layer 3 switch is required to enable Inter-VLAN communication.

The experiment demonstrated how VLANs logically segment a network while using the same physical switch infrastructure.

---

# Conclusion

The VLANs were successfully created and configured on the switch. Devices within the same VLAN communicated successfully, while communication between different VLANs was restricted. This experiment demonstrated the implementation of logical network segmentation using VLAN technology and highlighted the need for routing to enable communication between VLANs.

---

_Name: Kushal Prasad Joshi_  
_Program: BCE_  
_Roll No: 230345_
