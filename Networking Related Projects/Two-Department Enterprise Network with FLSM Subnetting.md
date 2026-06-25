

# *Project Title:* Two-Department Enterprise Network with FLSM Subnetting

## Platform Used: Cisco Packet Tracer

### Date: June 25, 2026

---

## 1. Introduction

The objective of this project is to design, configure, and implement a scalable and secure local area network (LAN) for an organization with two distinct departments: **ACCOUNTS** and **DELIVERY**. The implementation focuses on proper network segmentation using Fixed Length Subnet Masking (FLSM), efficient IP address allocation, and ensuring inter-departmental routing via a Layer 3 router.

---

## 2. Network Requirements & Specifications

As per the project guidelines, the network structure adheres to the following conditions:

* **Departments:** Two separate zones named ACCOUNTS and DELIVERY.
* **Host Count:** Each department must host at least 2 PCs and networked network peripherals (e.g., Printers).
* **Base Network Address:** $192.168.40.0$
* **Connectivity:** All devices must communicate seamlessly across the departments, verified via successful ICMP (ping) status.

---

## 3. Subnetting & IP Addressing Scheme

To isolate the broadcast domains of both departments, the base network $192.168.40.0$ is split into two equal subnets using a `/25` subnet mask ($255.255.255.128$).

### Subnetting Table

| Department | Network ID | Subnet Mask | Valid Host Range | Broadcast ID | Gateway IP (Router) |
| --- | --- | --- | --- | --- | --- |
| **ACCOUNTS** | $192.168.40.0/25$ | $255.255.255.128$ | $192.168.40.1$ – $192.168.40.126$ | $192.168.40.127$ | $192.168.40.1$ |
| **DELIVERY** | $192.168.40.128/25$ | $255.255.255.128$ | $192.168.40.129$ – $192.168.40.254$ | $192.168.40.255$ | $192.168.40.129$ |

---

## 4. Hardware Components Used

The network topology depicted in **image_14dabd.png** utilizes the following hardware devices:

1. **Router (Cisco 2911):** Acts as the central Layer 3 gateway to route traffic between the ACCOUNTS and DELIVERY subnets.
2. **Switches (Cisco 2960-24TT):** Two Layer 2 switches (`Switch0` and `Switch1`) to provide local connectivity within each department.
3. **End Devices:**
* **ACCOUNTS:** 2 PCs (`PC0`, `PC1`) and 1 Printer (`Printer0`).
* **DELIVERY:** 2 PCs (`PC2`, `PC3`) and 1 Printer (`Printer1`).


4. **Cabling:** Copper Straight-Through cables for connecting end devices to switches, and switches to the router interfaces.

---

## 5. Network Topology Diagram

The physical and logical layout of the network infrastructure is successfully mapped out in Cisco Packet Tracer:

<img width="1646" height="862" alt="image" src="https://github.com/user-attachments/assets/59da35fe-771f-47dc-8853-3611b20040cd" />

---

## 6. Configuration Steps

### Step A: Router Interface Configuration

The router interfaces were brought up (`no shutdown`) and assigned the respective default gateways for each subnet:

* **Interface Gig0/0-1 (ACCOUNTS Gateway):**
```text

Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.40.1 255.255.255.128
Router(config-if)# no shutdown
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 192.168.40.129 255.255.255.128
Router(config-if)# no shutdown

```

### Step B: End Device Configuration

Each PC and Printer was configured with a static IP from its respective subnet pool, along with the correct Subnet Mask ($255.255.255.128$) and Default Gateway.

* *Example (PC0):* IP: `192.168.40.2` | Gateway: `192.168.40.1`
* *Example (PC3):* IP: `192.168.40.131` | Gateway: `192.168.40.129`

---

## 7. Testing & Results (Verification)

To verify network connectivity, ICMP traffic simulations were executed between the hosts across different subnets.

As captured in the **PDU Realtime List Window** of **image_14dabd.png**, the connectivity is 100% successful:

* **PC0 to PC3:** `Successful` (ICMP)
* **Printer0 to PC3:** `Successful` (ICMP)
* **Printer1 to PC0:** `Successful` (ICMP)
* **PC3 to PC1:** `Successful` (ICMP)

The routing tables are perfectly updated, and there is zero packet loss during inter-departmental communication.

---

## 8. Conclusion

The network designed for the ACCOUNTS and DELIVERY departments has been successfully implemented and tested. By applying subnets, broadcast traffic is confined locally within each department, boosting security and performance while the Cisco 2911 router safely ensures smooth communication between them.

---


---
