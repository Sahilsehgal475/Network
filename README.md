# Network

1. VLAN Creation

On your Core Switch (MLS):

vlan 10
 name HR
vlan 20
 name IT
vlan 30
 name Sales


---

2. VTP Configuration

Set Core Switch as Server and Access switches as Clients:

Core (VTP Server):

vtp domain xceed
vtp mode server

Access SWs:

vtp domain xceed
vtp mode client


---

3. Trunk Ports

Make the ports between Core, Distribution, and Access switches as trunk:

interface FastEthernet0/1
 switchport mode trunk

Do this on all inter-switch links.


---

4. Inter-VLAN Routing (on MLS/Core Switch)

Use SVIs on MLS:

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown

Make sure the SVI interfaces are up and the Core switch has a connected port in each VLAN (or use routed links for testing).


---

5. Assign Access Ports to VLANs

On Access Switches:

interface fa0/2
 switchport mode access
 switchport access vlan 10

Repeat for VLAN 20, 30 depending on the department (IT, HR, Sales).


---

6. DHCP Configuration on Core Switch

ip dhcp pool HR
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1

ip dhcp pool IT
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1

ip dhcp pool SALES
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1

ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10


---

7. Final Testing

Check PCs if they receive IPs in correct ranges.

Ping between PCs from different VLANs to test Inter-VLAN routing.
