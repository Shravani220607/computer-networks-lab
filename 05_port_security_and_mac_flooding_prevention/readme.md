# Port Security and MAC Flooding Prevention

## Overview

This experiment demonstrates the implementation of Port Security on Cisco switches to prevent unauthorized devices from accessing the network. Normally, a switch learns MAC addresses dynamically and allows any new device connected to a port to communicate. This behavior can be exploited by attackers who connect unauthorized devices or perform MAC flooding attacks.

To improve network security, Port Security was configured using Sticky MAC learning. Legitimate devices were automatically learned and stored as secure MAC addresses, while violation actions were configured to restrict or disable unauthorized access attempts.

---

## Objectives

* Configure Port Security on Cisco switches.
* Implement Sticky MAC address learning.
* Restrict the number of devices allowed on a switch port.
* Prevent unauthorized devices from accessing the network.
* Understand the concept of MAC flooding attacks.
* Verify learned secure MAC addresses and port security status.

---

## Network Topology

### Existing Configuration

The topology consists of:

* One Main Switch (Aggregation Switch)
* Three Access Switches
* Nine Legitimate PCs
* Three Attacker PCs

The Main Switch connects all access switches, while each access switch connects three end devices.

### Security Design

Port Security was applied to:

* User-facing ports on all access switches.
* Uplink ports on the Main Switch.

The Main Switch was configured differently because each uplink carries traffic from multiple PCs connected through an access switch.

---

## IP Addressing Scheme

All devices belong to the same network.

| Device Group | Address Range |
| ------------ | ------------- |
| Legitimate PCs | 10.0.0.1 – 10.0.0.9 |
| Attacker PCs | 10.0.0.10 – 10.0.0.12 |

Subnet Mask Used:

```text
255.0.0.0
```

---

## Access Switch Configuration

Port Security was configured on all PC-facing ports.

```bash
interface range fa0/1 - 3

switchport mode access

switchport port-security

switchport port-security maximum 1

switchport port-security mac-address sticky

switchport port-security violation shutdown
```

### Why Maximum 1?

```bash
switchport port-security maximum 1
```

Each access port is connected to a single PC.

Allowing only one MAC address ensures that only one authorized device can use the port.

### Sticky MAC Learning

```bash
switchport port-security mac-address sticky
```

Automatically learns and stores the MAC address of the connected device.

After traffic is generated, the learned MAC address becomes a secure MAC address.

### Shutdown Violation Mode

```bash
switchport port-security violation shutdown
```

If a different device is connected:

```text
Authorized PC Connected
          ↓
MAC Learned
          ↓
Attacker Replaces PC
          ↓
Different MAC Detected
          ↓
Port Shutdown
```

This provides strong protection against unauthorized access.

---

## Main Switch Configuration

The Main Switch connects to three access switches.

Each uplink receives traffic from multiple PCs rather than a single device.

Configuration:

```bash
interface fa0/1

switchport mode access

switchport port-security

switchport port-security maximum 4

switchport port-security mac-address sticky

switchport port-security violation restrict
```

The same configuration was applied to:

```text
Fa0/2
Fa0/3
```

### Why Maximum 4?

Each access switch contains three PCs.

Example:

```text
Fa0/1
   ↓
Access Switch
   ├─ PC0
   ├─ PC1
   └─ PC2
```

The Main Switch must learn multiple MAC addresses arriving through a single uplink.

A secure MAC limit of four allows legitimate devices while preventing excessive MAC learning.

### Why Restrict Mode?

```bash
switchport port-security violation restrict
```

Behavior:

```text
Unauthorized MAC Detected
          ↓
Traffic Blocked
          ↓
Violation Counter Increased
          ↓
Port Remains Operational
```

Unlike Shutdown mode, Restrict mode prevents unauthorized traffic without disabling the uplink.

---

## How Port Security Works

Every network device has a unique MAC address.

Normally, switches learn MAC addresses dynamically and store them in the MAC Address Table.

Without Port Security:

```text
Legitimate Device Connected
          ↓
MAC Learned

Legitimate Device Removed
          ↓
Attacker Device Connected
          ↓
New MAC Learned
          ↓
Access Granted
```

With Port Security:

```text
Legitimate Device Connected
          ↓
MAC Learned and Secured
          ↓
Attacker Device Connected
          ↓
MAC Mismatch Detected
          ↓
Security Violation Triggered
```

Only trusted MAC addresses are allowed to use secured ports.

---

## MAC Flooding Attack

A MAC flooding attack attempts to fill the switch MAC Address Table with fake MAC addresses.

Example:

```text
AAAA
BBBB
CCCC
DDDD
EEEE
FFFF
...
```

As the switch learns these fake MAC addresses:

```text
MAC Table Fills Up
          ↓
Unknown Traffic Flooded
          ↓
Possible Traffic Interception
```

An attacker may exploit this behavior to observe traffic that would normally not reach their device.

---

## MAC Flooding Prevention

Port Security limits the number of MAC addresses that can be learned on a port.

Example:

```bash
switchport port-security maximum 1
```

Result:

```text
First MAC Address Accepted
          ↓
Additional MAC Addresses Rejected
```

This prevents attackers from overflowing the MAC Address Table with fake entries.

---

## Verification Commands

### Verify Port Security Status

```bash
show port-security
```

Displays:

* Maximum Secure Addresses
* Current Secure Addresses
* Security Violations
* Security Action

### View Learned Secure MAC Addresses

```bash
show port-security address
```

Displays:

* SecureSticky MAC Addresses
* Associated Ports
* Learned Secure Entries

### View MAC Address Table

```bash
show mac address-table
```

Displays:

* Dynamic MAC Entries
* Secure MAC Entries
* Port Associations

---

## Testing

### Step 1: Generate Traffic

Ping requests were exchanged between legitimate devices to allow switches to learn MAC addresses automatically.

### Step 2: Verify Learned MAC Addresses

Command:

```bash
show port-security address
```

Result:

```text
SecureSticky MAC addresses learned successfully.
```

### Step 3: Verify Port Security Status

Command:

```bash
show port-security
```

Observed Results:

### Access Switches

```text
Maximum Secure Addresses : 1
Current Secure Addresses : 1
Security Violations      : 0
Security Action          : Shutdown
```

### Main Switch

```text
Maximum Secure Addresses : 4
Current Secure Addresses : 4
Security Violations      : 0
Security Action          : Restrict
```

### Step 4: Simulated Unauthorized Access

Three attacker systems were prepared:

```text
PC9
PC10
PC11
```

If an attacker replaces a legitimate device:

```text
Legitimate PC Removed
          ↓
Attacker PC Connected
          ↓
Different MAC Detected
          ↓
Port Security Violation
```

Access switch ports would enter a secure state according to the configured violation mode.

---

## Results

Before Port Security:

```text
Any Device Could Connect
          ↓
Switch Learns New MAC
          ↓
Access Granted
```

After Port Security:

```text
Only Learned MAC Addresses Allowed
          ↓
Unauthorized Devices Detected
          ↓
Security Action Applied
```

The switches successfully learned trusted MAC addresses using Sticky MAC learning and enforced secure MAC limits on protected ports.

---

## Conclusion

Port Security was successfully implemented on Cisco switches using Sticky MAC learning. Access switch ports were restricted to a single device, while Main Switch uplinks were configured to allow a limited number of secure MAC addresses. The experiment demonstrated how Port Security can prevent unauthorized access, control MAC address learning, and reduce the effectiveness of MAC flooding attacks in a switched network environment.