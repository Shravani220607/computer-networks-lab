# Inter-VLAN Routing Using Router-on-a-Stick

## Overview

This experiment extends the previous VLAN and IEEE 802.1Q trunking configuration by enabling communication between different VLANs using a router. Initially, devices within the same VLAN could communicate across switches through trunk links, but devices in different VLANs were isolated. To overcome this limitation, Router-on-a-Stick Inter-VLAN Routing was implemented.

---

## Objectives

* Extend an existing VLAN and trunking topology.
* Configure Router-on-a-Stick Inter-VLAN Routing.
* Enable communication between different VLANs.
* Configure IEEE 802.1Q encapsulation on router subinterfaces.
* Verify successful communication across VLAN boundaries.

---

## Network Topology

### Existing Configuration

The topology already contained:

* Two Cisco switches
* VLAN 10 (BLUE)
* VLAN 20 (ORANGE)
* VLAN 30 (LAVENDER)
* IEEE 802.1Q trunk link between switches

Communication Status:

* VLAN 10 ↔ VLAN 10 : Successful
* VLAN 20 ↔ VLAN 20 : Successful
* VLAN 30 ↔ VLAN 30 : Successful
* VLAN 10 ↔ VLAN 20 : Failed
* VLAN 10 ↔ VLAN 30 : Failed
* VLAN 20 ↔ VLAN 30 : Failed

### Added Component

A Cisco 2911 Router was connected to the switch through a trunk port to perform Layer 3 routing between VLANs.

---

## VLAN Information

| VLAN ID | VLAN Name |
| ------- | --------- |
| 10      | BLUE      |
| 20      | ORANGE    |
| 30      | LAVENDER  |

---

## IP Addressing Scheme

### VLAN 10 (BLUE)

| Device Type | Address Range |
| ----------- | ------------- |
| PCs         | 10.0.10.x     |
| Gateway     | 10.0.10.1     |

### VLAN 20 (ORANGE)

| Device Type | Address Range |
| ----------- | ------------- |
| PCs         | 10.0.20.x     |
| Gateway     | 10.0.20.1     |

### VLAN 30 (LAVENDER)

| Device Type | Address Range |
| ----------- | ------------- |
| PCs         | 10.0.30.x     |
| Gateway     | 10.0.30.1     |

Subnet Mask Used:

```text
255.255.255.0
```

---

## Switch Configuration

The switch port connected to the router was configured as a trunk.

```bash
interface fa0/8
switchport mode trunk
```

This allows VLAN traffic from VLANs 10, 20, and 30 to traverse a single physical connection between the switch and router.

---

## Router Configuration

### Enable Physical Interface

```bash
interface g0/0
no shutdown
```

### VLAN 10 Subinterface

```bash
interface g0/0.10
encapsulation dot1Q 10
ip address 10.0.10.1 255.255.255.0
```

### VLAN 20 Subinterface

```bash
interface g0/0.20
encapsulation dot1Q 20
ip address 10.0.20.1 255.255.255.0
```

### VLAN 30 Subinterface

```bash
interface g0/0.30
encapsulation dot1Q 30
ip address 10.0.30.1 255.255.255.0
```

---

## How Router-on-a-Stick Works

A single router interface is divided into multiple virtual subinterfaces.

Each subinterface:

* Represents a VLAN.
* Receives frames tagged using IEEE 802.1Q.
* Acts as the default gateway for that VLAN.
* Routes traffic between VLAN networks.

Example:

```text
g0/0.10 → VLAN 10 Gateway
g0/0.20 → VLAN 20 Gateway
g0/0.30 → VLAN 30 Gateway
```

---

## Verification Commands

### Verify VLANs

```bash
show vlan brief
```

### Verify Trunking

```bash
show interfaces trunk
```

### Verify Router Interfaces

```bash
show ip interface brief
```

---

## Testing

Successful ping tests were performed between devices belonging to different VLANs.

Examples:

```text
VLAN 10 PC → VLAN 20 PC
VLAN 10 PC → VLAN 30 PC
VLAN 20 PC → VLAN 30 PC
```

The first ping may occasionally experience packet loss due to ARP resolution, after which communication becomes successful.

---

## Results

Before Inter-VLAN Routing:

```text
VLAN 10 ❌ VLAN 20
VLAN 10 ❌ VLAN 30
VLAN 20 ❌ VLAN 30
```

After Inter-VLAN Routing:

```text
VLAN 10 ✅ VLAN 20
VLAN 10 ✅ VLAN 30
VLAN 20 ✅ VLAN 30
```

All VLANs successfully communicated through the router using Router-on-a-Stick configuration.

---

## Conclusion

Inter-VLAN Routing was successfully implemented using a Cisco 2911 Router and IEEE 802.1Q trunking. Router subinterfaces were configured for VLANs 10, 20, and 30, allowing devices in separate VLANs to communicate while maintaining logical network segmentation. This experiment demonstrates how Layer 3 routing enables communication across multiple VLANs in a switched network environment.
