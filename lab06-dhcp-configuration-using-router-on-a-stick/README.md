# Lab 6: VLAN Creation and DHCP Configuration Using Router-on-a-Stick

## Objectives

* To understand the concept of Virtual Local Area Networks (VLANs).
* To subnet a network into multiple VLANs.
* To create VLANs on a switch.
* To assign switch ports to VLANs.
* To configure trunk links between a switch and a router.
* To implement Inter-VLAN Routing using Router-on-a-Stick.
* To configure the router as a DHCP server.
* To dynamically assign IP addresses to devices in different VLANs.
* To verify communication within and across VLANs.

---

## Theory

A Virtual Local Area Network (VLAN) is a logical division of a switched network that creates separate broadcast domains within the same physical infrastructure. VLANs improve security, reduce unnecessary broadcasts, and simplify network management.

By default, devices in different VLANs cannot communicate because VLANs create separate Layer 2 networks. To allow communication between VLANs, a Layer 3 device such as a router must be used.

Router-on-a-Stick is a technique that uses a single physical router interface divided into multiple logical subinterfaces. Each subinterface is associated with a VLAN and acts as its default gateway.

Dynamic Host Configuration Protocol (DHCP) is used to automatically assign IP addresses, subnet masks, default gateways, and DNS server information to clients. In this experiment, the router acts as the DHCP server for all VLANs.

---

## Network Design

### VLAN Information

| VLAN ID | VLAN Name | Devices      |
| ------- | --------- | ------------ |
| 10      | HR        | PC0, Laptop0 |
| 20      | IT        | PC1, Laptop1 |
| 30      | Admin     | PC2, Laptop2 |


### Subnetting

Given Network:

```text
192.168.1.0/24
```

Subnetted into three VLAN networks:

| VLAN | Network Address  | Gateway       |
| ---- | ---------------- | ------------- |
| 10   | 192.168.1.0/26   | 192.168.1.1   |
| 20   | 192.168.1.64/26  | 192.168.1.65  |
| 30   | 192.168.1.128/26 | 192.168.1.129 |

Subnet Mask:

```text
255.255.255.192
```

---

## Implementation

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
name ADMIN

end
```

#### Assign Ports to VLAN 10

```bash
configure terminal

interface range fa0/1 - 2

switchport mode access

switchport access vlan 10

exit
```

#### Assign Ports to VLAN 20

```bash
interface range fa0/3 - 4

switchport mode access

switchport access vlan 20

exit
```

#### Assign Ports to VLAN 30

```bash
interface range fa0/5 - 6

switchport mode access

switchport access vlan 30

exit
```

#### Configure Trunk Port

Assuming the router is connected to Fa0/24:

```bash
interface fa0/24

switchport mode trunk

exit

end
```

#### Save Configuration

```bash
copy running-config startup-config
```

## Router Configuration

### Configure Physical Interface

```bash
enable

configure terminal

interface g0/0/0

no shutdown

exit
```

### Configure Subinterfaces

#### VLAN 10

```bash
interface g0/0/0.10

encapsulation dot1Q 10

ip address 192.168.1.1 255.255.255.192

exit
```

#### VLAN 20

```bash
interface g0/0/0.20

encapsulation dot1Q 20

ip address 192.168.1.65 255.255.255.192

exit
```

#### VLAN 30

```bash
interface g0/0/0.30

encapsulation dot1Q 30

ip address 192.168.1.129 255.255.255.192

exit
```

### DHCP Configuration

#### Excluded Addresses

You may not need to configure the excluded-address in most of the cases.
But it is good to exclude the network and broadcasting addresses.

```bash
ip dhcp excluded-address 192.168.1.1
ip dhcp excluded-address 192.168.1.65
ip dhcp excluded-address 192.168.1.129
```

#### DHCP Pool for VLAN 10

```bash
ip dhcp pool HR

network 192.168.1.0 255.255.255.192

default-router 192.168.1.1

dns-server 8.8.8.8 # Not compulsory
```

#### DHCP Pool for VLAN 20

```bash
ip dhcp pool IT

network 192.168.1.64 255.255.255.192

default-router 192.168.1.65

dns-server 8.8.8.8
```

---

#### DHCP Pool for VLAN 30

```bash
ip dhcp pool ADMIN

network 192.168.1.128 255.255.255.192

default-router 192.168.1.129

dns-server 8.8.8.8
```

#### Save Configuration

```bash
end

copy running-config startup-config
```

### Client Configuration

For every PC and Laptop:

```text
Desktop → IP Configuration

Select DHCP
```

The router automatically assigns:

* IP Address
* Subnet Mask
* Default Gateway
* DNS Server

### Verification

#### Verify VLAN Configuration

```bash
show vlan brief
```

#### Verify Trunk Configuration

```bash
show interfaces trunk
```

#### Verify Router Interfaces

```bash
show ip interface brief
```

Expected Output:

```text
Gig0/0/0      unassigned        up   up
Gig0/0/0.10   192.168.1.1       up   up
Gig0/0/0.20   192.168.1.65      up   up
Gig0/0/0.30   192.168.1.129     up   up
```

#### Verify DHCP Assignments

```bash
show ip dhcp binding
```

#### Verify Client IP Address

On any PC:

```bash
ipconfig
```

---

# Discussion

Three VLANs were successfully created and configured on a single switch. A trunk link was established between the switch and router. Router-on-a-Stick was implemented using router subinterfaces, allowing communication between different VLANs. The router was also configured as a DHCP server and dynamically assigned IP addresses to all devices.

---

# Conclusion

The network was successfully divided into three VLANs through subnetting. VLANs were configured on the switch, and Inter-VLAN Routing was implemented using Router-on-a-Stick. The router provided DHCP services for all VLANs, automatically assigning IP addresses to clients. 

---

_Name: Kushal Prasad Joshi_  
_Program: BCE_  
_Roll No: 230345_
