# VLAN and Trunking

## Overview

This project demonstrates the implementation of Virtual Local Area Networks (VLANs) and trunking using Cisco Packet Tracer. The network consists of two switches interconnected through a trunk link and multiple end devices grouped into different VLANs. VLANs were used to logically segment the network into separate broadcast domains, while trunking was used to allow VLAN traffic to travel between switches.

The purpose of this project is to understand how VLANs improve network organization, security, and scalability, and how trunk links enable communication between devices belonging to the same VLAN across multiple switches.

---

## Objective

* Understand the concept of VLANs and their role in network segmentation.
* Configure multiple VLANs on Cisco switches.
* Assign switch ports to different VLANs.
* Configure a trunk link between switches.
* Verify communication between devices belonging to the same VLAN.
* Observe isolation between devices belonging to different VLANs.
* Study the operation of VLAN tagging and trunking.

---

## Network Topology

The network consists of:

* 2 Cisco Switches
* 12 PCs
* 3 VLANs
* 1 Trunk Link connecting both switches

The devices are grouped into different VLANs using color-based segmentation.

### VLAN Distribution

| VLAN ID | Group        | Devices              |
| ------- | ------------ | -------------------- |
| VLAN 10 | Blue Group   | PC0, PC1, PC6, PC7   |
| VLAN 20 | Orange Group | PC2, PC3, PC10, PC11 |
| VLAN 30 | Purple Group | PC4, PC5, PC8, PC9   |

### Topology Screenshot

![Network Topology](topology.png)

---

## Theory

### What is a VLAN?

A Virtual Local Area Network (VLAN) is a logical grouping of devices within a network regardless of their physical location. VLANs divide a switch into multiple independent broadcast domains.

Without VLANs, all devices connected to a switch belong to the same broadcast domain. As networks grow, this can lead to excessive broadcast traffic and reduced security.

VLANs provide:

* Improved network security
* Better traffic management
* Reduced broadcast traffic
* Simplified network administration
* Enhanced scalability

Devices within the same VLAN can communicate directly, while communication between different VLANs requires a Layer 3 device such as a router or Layer 3 switch.

---

### What is Trunking?

A trunk is a network link that carries traffic for multiple VLANs simultaneously.

Instead of using separate physical links for every VLAN, trunking allows multiple VLANs to share a single connection.

Trunk ports use IEEE 802.1Q tagging to identify which VLAN each Ethernet frame belongs to.

In this project, a trunk link connects the two switches and carries traffic for VLAN 10, VLAN 20, and VLAN 30.

---

### Broadcast Domains

A broadcast domain is a group of devices that receive the same broadcast traffic.

In this network:

* VLAN 10 forms one broadcast domain.
* VLAN 20 forms another broadcast domain.
* VLAN 30 forms a third broadcast domain.

Broadcast traffic generated in one VLAN is not forwarded to devices in other VLANs.

---

## VLAN Configuration

### VLAN 10 – Blue Group

This VLAN contains:

* PC0
* PC1
* PC6
* PC7

All devices belonging to VLAN 10 can communicate with each other through the switches and trunk link.

---

### VLAN 20 – Orange Group

This VLAN contains:

* PC2
* PC3
* PC10
* PC11

All devices belonging to VLAN 20 can communicate with each other while remaining isolated from VLAN 10 and VLAN 30.

---

### VLAN 30 – Purple Group

This VLAN contains:

* PC4
* PC5
* PC8
* PC9

All devices belonging to VLAN 30 form a separate broadcast domain and communicate independently of the other VLANs.

---

## Trunk Link Configuration

A trunk connection was established between the two switches.

```text
Switch0 <------ Trunk Link ------> Switch1
```

The trunk carries traffic for:

* VLAN 10
* VLAN 20
* VLAN 30

This allows devices belonging to the same VLAN but connected to different switches to communicate successfully.

Without trunking, VLAN traffic would remain confined to the local switch and communication across switches would not be possible.

---

## Communication Verification

### Same VLAN Communication

Devices belonging to the same VLAN were able to communicate successfully.

Examples:

#### VLAN 10

```text
PC0 ↔ PC6
PC1 ↔ PC7
```

#### VLAN 20

```text
PC2 ↔ PC10
PC3 ↔ PC11
```

#### VLAN 30

```text
PC4 ↔ PC8
PC5 ↔ PC9
```

Successful ping replies confirmed correct VLAN membership and trunk operation.

---

### Different VLAN Communication

Communication between different VLANs was unsuccessful.

Examples:

```text
PC0 ↔ PC2
PC1 ↔ PC10
PC4 ↔ PC7
```

This behavior is expected because Inter-VLAN Routing was not configured.

The inability of different VLANs to communicate confirms proper VLAN isolation and segmentation.

---

## Verification Commands

### Display VLAN Information

```bash
show vlan brief
```

This command displays all configured VLANs and their assigned switch ports.

---

### Display Trunk Information

```bash
show interfaces trunk
```

This command verifies trunk configuration and displays VLANs allowed on the trunk link.

---

### Display Running Configuration

```bash
show running-config
```

This command displays the current switch configuration including VLAN and trunk settings.

---

## Concepts Demonstrated

* VLAN Creation
* VLAN Membership Assignment
* Access Port Configuration
* Trunk Port Configuration
* IEEE 802.1Q Trunking
* Broadcast Domains
* Network Segmentation
* Logical Network Design
* VLAN Isolation
* Switch Configuration and Verification

---

## Advantages of VLANs

* Improved network security
* Reduced broadcast traffic
* Better network performance
* Simplified management
* Flexible network organization
* Easier troubleshooting
* Enhanced scalability

---

## Advantages of Trunking

* Efficient use of physical links
* Supports multiple VLANs over one connection
* Simplifies network expansion
* Reduces cabling requirements
* Enables VLAN communication across switches

---

## Result

Three VLANs were successfully created and configured across two switches. A trunk link was established between the switches to carry VLAN traffic. Devices belonging to the same VLAN communicated successfully even when connected to different switches, while devices belonging to different VLANs remained isolated. The project successfully demonstrated VLAN segmentation, broadcast domain separation, and trunking using IEEE 802.1Q.

---

## Tools Used

* Cisco Packet Tracer
* Cisco Catalyst Switches
* VLAN Configuration
* IEEE 802.1Q Trunking
* IPv4 Networking
