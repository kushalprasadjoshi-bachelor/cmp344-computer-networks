# Lab 4: Configuration of DHCP, DNS, and Web Servers with Dynamic IP Addressing and Website Access Across Networks**

## Objectives

* To configure a DHCP server for automatic IP address allocation.
* To configure a DNS server for domain name resolution.
* To configure a Web Server and host a website.
* To configure DHCP Relay Agent using the router.
* To assign dynamic IP addresses to client devices.
* To access a hosted website using a domain name from another network.
* To verify communication between client and server networks through a router.

---

## Theory

Modern computer networks use various server services to simplify network administration and improve accessibility.

A **DHCP (Dynamic Host Configuration Protocol) Server** automatically assigns IP addresses and other network parameters such as subnet mask, default gateway, and DNS server address to client devices. This eliminates the need for manual IP configuration and reduces configuration errors.

A **DNS (Domain Name System) Server** translates human-readable domain names into IP addresses. Instead of remembering numerical IP addresses, users can access network resources using easy-to-remember domain names.

A **Web Server** hosts websites and responds to HTTP requests from clients. Users can access webpages through a web browser using either an IP address or a domain name.

In large networks, servers are often placed in a dedicated server network separate from user devices. Since DHCP requests are broadcast messages and routers do not forward broadcasts by default, a **DHCP Relay Agent** is required. Cisco routers use the `ip helper-address` command to forward DHCP requests from client networks to a remote DHCP server.

In this experiment, two different networks were created:

* User Network: 192.168.1.0/24
* Server Network: 192.168.2.0/24

The DHCP, DNS, and Web Servers were placed in the server network. A router connected both networks and acted as a DHCP Relay Agent. Clients obtained IP addresses dynamically from the DHCP server, resolved domain names through the DNS server, and accessed a website hosted on the Web Server.

---

## Implementation

### Network Topology

#### User Network

```text
Network: 192.168.1.0/24

PC0
PC1     ----- Switch0 -------- Router2 (192.168.1.1)
PC2
Laptop0
```

#### Server Network

```text
Network: 192.168.2.0/24

                                        DHCP Server  : 192.168.2.2
Router2 (192.168.2.1) --- Switch1 ----  DNS Server   : 192.168.2.3
                                        Web Server   : 192.168.2.4
```

### IP Addressing Table

| Device        | IP Address           |
| ------------- | -------------------- |
| Router G0/0/0 | 192.168.1.1          |
| Router G0/0/1 | 192.168.2.1          |
| DHCP Server   | 192.168.2.2          |
| DNS Server    | 192.168.2.3          |
| Web Server    | 192.168.2.4          |
| Client PCs    | Dynamically Assigned |

Subnet Mask for all devices:

```text
255.255.255.0
```

### DHCP Server Configuration

#### Server IP Configuration

```text
IP Address : 192.168.2.2
Subnet Mask: 255.255.255.0
Gateway    : 192.168.2.1
DNS Server : 192.168.2.3
```

#### DHCP Service Configuration

```text
Pool Name       : USER_POOL
Default Gateway : 192.168.1.1
DNS Server      : 192.168.2.3
Start IP        : 192.168.1.10
Subnet Mask     : 255.255.255.0
Maximum Users   : 100
```

DHCP Service was enabled.

### DNS Server Configuration

#### Server IP Configuration

```text
IP Address : 192.168.2.3
Subnet Mask: 255.255.255.0
Gateway    : 192.168.2.1
```

DNS Service was enabled.

#### DNS Record

| Domain Name    | IP Address  |
| -------------- | ----------- |
| www.demo.com   | 192.168.2.4 |


### Web Server Configuration

Server IP Configuration:

```text
IP Address : 192.168.2.4
Subnet Mask: 255.255.255.0
Gateway    : 192.168.2.1
DNS Server : 192.168.2.3
```

HTTP Service was enabled.

### Router Configuration

#### Configure Interface Connected to User Network

```bash
enable

configure terminal

interface g0/0/0
ip address 192.168.1.1 255.255.255.0

ip helper-address 192.168.2.2

no shutdown
exit
```

#### Configure Interface Connected to Server Network

```bash
interface g0/0/1
ip address 192.168.2.1 255.255.255.0

no shutdown
exit

end

copy running-config startup-config
```

### Client Configuration

On each client device:

```text
Desktop → IP Configuration

Select DHCP
```

The DHCP server automatically assigned:

* IP Address
* Subnet Mask
* Default Gateway
* DNS Server Address

Example:

```text
IP Address : 192.168.1.10
Subnet Mask: 255.255.255.0
Gateway    : 192.168.1.1
DNS Server : 192.168.2.3
```

### Verification

#### DHCP Verification

On any client:

```bash
ipconfig
```

Output confirmed successful dynamic IP allocation.

#### DNS Verification

```bash
ping www.demo.com
```

Expected Output:

```text
Pinging 192.168.2.4
```

This confirmed successful DNS resolution.

#### Web Server Verification

Open:

```text
Desktop → Web Browser
```

Enter:

```text
www.demo.com
```

The webpage hosted on the Web Server was displayed successfully.

#### Network Connectivity Verification

Client to Router:

```bash
ping 192.168.1.1
```

Client to DNS Server:

```bash
ping 192.168.2.3
```

Client to Web Server:

```bash
ping 192.168.2.4
```

All tests were successful.

---

# Discussion

The DHCP server successfully assigned dynamic IP addresses to clients located in a different network. Since DHCP broadcasts cannot cross routers, the router was configured with the `ip helper-address` command, enabling DHCP Relay functionality.

The DNS server was configured with a domain record mapping the hostname `www.demo.com` to the IP address of the Web Server. This allowed users to access the website using a domain name instead of an IP address.

The Web Server hosted a webpage that was accessible from the user network through the router. Successful website access confirmed proper operation of DHCP, DNS, HTTP, and routing services.

The experiment demonstrated how multiple network services work together in a client-server environment to provide automated addressing, name resolution, and web access.

---

# Conclusion

The DHCP, DNS, and Web Servers were successfully configured in the server network. Dynamic IP addresses were assigned to client devices through DHCP Relay. Domain name resolution was achieved using the DNS server, and a website hosted on the Web Server was successfully accessed from a different network through the router. The experiment verified the integration of DHCP, DNS, HTTP, and routing services in a networked environment.

---

_Name: Kushal Prasad Joshi_  
_Program: BCE_  
_Roll No: 230345_
