           Project Report: Enterprise LAN Design 

1. Introduction
In today’s enterprise environments, building a scalable, resilient, and secure Local Area Network (LAN) is critical for seamless business operations. This project focuses on designing and deploying an enterprise-grade network topology using Cisco Packet Tracer. The architecture segregates different organizational departments—HR, IT, and Finance—into distinct broadcast domains using Virtual Local Area Networks (VLANs). To guarantee seamless communication between these departments while maintaining robust security boundaries, Inter-VLAN Routing (Router-on-a-Stick) is implemented using a main router. Furthermore, to address network bottlenecking and provide high-availability infrastructure, EtherChannel (PAgP) is used to aggregate parallel physical links between the Core and Distribution layers, ensuring seamless redundancy and structural efficiency.

2. Objectives
The primary technical and operational objectives of this project are:
Network Segmentation: To design and configure VLANs (VLAN 10, 20, and 30) to logically isolate departmental traffic, enhancing bandwidth management and minimizing broadcast storms.
Inter-VLAN Communication: To implement sub-interfaces on the GigabitEthernet trunk port of the router, allowing structured and routed communication across different subnets.
Link Aggregation & Redundancy: To configure PAgP-based EtherChannel between switches, increasing logical bandwidth and preventing a single point of failure (SPOF) at the link layer.
Centralized VLAN Management: To deploy VLAN Trunking Protocol (VTP) with a Server-Client model to synchronize VLAN databases consistently across all network switches.
Endpoint Port Security: To secure edge-layer distribution ports using modern port-security constraints (such as sticky MAC addresses and dynamic shutdown violations).




3. Network Topology Diagram




4. IP Addressing Scheme & VLAN Mapping

End-Device IP Configuration 
Device Name
IP Address
Subnet Mask
Default Gateway
PC0
172.31.10.10
255.255.255.0
172.31.10.1
PC1
172.31.10.11
255.255.255.0
172.31.10.1
PC2
172.31.20.10
255.255.255.0
172.31.20.1
PC3
172.31.20.11
255.255.255.0
172.31.20.1
PC4
172.31.30.10
255.255.255.0
172.31.30.1
PC5
172.31.30.11
255.255.255.0
172.31.30.1
PC6
172.31.20.13
255.255.255.0
172.31.20.1
PC7
172.31.20.15
255.255.255.0
172.31.20.1
PC8
172.31.10.15
255.255.255.0
172.31.10.1
PC9
172.31.10.13
255.255.255.0
172.31.10.1
PC10
172.31.30.13
255.255.255.0
172.31.30.1
PC11
172.31.30.15
255.255.255.0
172.31.30.1










IP Scheme & VLAN Mapping Table
VLAN ID
Department
Network Subnet
Subnet Mask
Default Gateway
Assigned Devices
10
HR
172.31.10.0/24
255.255.255.0
172.31.10.1
PC0, PC1, PC8, PC9
20
IT
172.31.20.0/24
255.255.255.0
172.31.20.1
PC2, PC3, PC6, PC7
30
Finance
172.31.30.0/24
255.255.255.0
172.31.30.1
PC4, PC5, PC10, PC11



5. Configuration Commands for Each Device

A. Core Switch (Core-Switch) 

Switch> enable
Switch# configure terminal
Switch(config)# hostname Core-Switch

vtp mode server
vtp domain Enterprise
vtp password cisco

vlan 10
 name HR
vlan 20
 name IT
vlan 30
 name Finance
exit

interface range fastEthernet 0/2 - 3
 channel-group 1 mode auto
exit

interface range fastEthernet 0/4 - 5
 channel-group 1 mode auto
exit

interface port-channel 1
 switchport mode trunk
exit

interface fastEthernet 0/1
 switchport mode trunk
end
write


B. Distribution Switch 1 (Dist-Switch1) 

Switch> enable
Switch# configure terminal
Switch(config)# hostname Dist-Switch1

vtp mode client
vtp domain Enterprise
vtp password cisco

interface range fastEthernet 0/1 - 2
 switchport mode trunk
 speed 100
 duplex full
 channel-protocol pagp
 channel-group 1 mode desirable
exit

interface range fastEthernet 0/3 - 4
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
exit

interface range fastEthernet 0/5 - 6
 switchport mode access
 switchport access vlan 20
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
exit

interface range fastEthernet 0/7 - 8
 switchport mode access
 switchport access vlan 30
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
end
write

C. Distribution Switch 2 (Dist-Switch2) 

Switch> enable
Switch# configure terminal
Switch(config)# hostname Dist-Switch2

vtp mode client
vtp domain Enterprise
vtp password cisco

interface range fastEthernet 0/1 - 2
 switchport mode trunk
 speed 100
 duplex full
 channel-protocol pagp
 channel-group 1 mode desirable
exit

interface range fastEthernet 0/5 - 6
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
exit

interface range fastEthernet 0/3 - 4
 switchport mode access
 switchport access vlan 20
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
exit

interface range fastEthernet 0/7 - 8
 switchport mode access
 switchport access vlan 30
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
end
write

D. Main Router (Main-Router) 

Router> enable
Router# configure terminal
Router(config)# hostname Main-Router

enable secret cisco123
line vty 0 4
 password class123
 login
exit

interface gigabitEthernet 0/0
 no shutdown
exit

interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 172.31.10.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 172.31.20.1 255.255.255.0
exit

interface gigabitEthernet 0/0.30
 encapsulation dot1Q 30
 ip address 172.31.30.1 255.255.255.0
end
write


6. Test Results & Verification 

A. Ping Tests (Inter-VLAN Communication) 


B. EtherChannel Redundancy Verification 




7. Conclusion
The implementation of the Enterprise LAN project successfully demonstrates the core principles of modern structural networking. Through systematic testing and inbound ICMP simulation, all inter-departmental ping tests between the HR, IT, and Finance subnets achieved a 100% success rate. The configuration of VTP minimized administrative overhead, while the PAgP EtherChannel mechanism validated active load balancing and network redundancy. Additionally, enforced access layer security protocols successfully minimized unauthorized perimeter access. In conclusion, the finalized infrastructure meets all enterprise requirements, establishing a highly reliable, secure, fault-tolerant, and perfectly scalable corporate network environment.

