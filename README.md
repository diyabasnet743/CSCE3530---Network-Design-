# CSCE3530 - Computer Network Design Project
  A working clinet-server network built in Cisco Packet Tracer, where devices communicate using the core networking protocols. The project covers designing the topology, configuring the router and servers, and cpaturing/analyizng traffic to demonstrate each protocol in action. 

# Overview
  The network is a single internal LAN connected to a simulated external network. AN edge router acts as the gateway and handles DHCP, NAT, and routing. A switch connects all internal devices, and two servers provide DNS and web services. A second router stands in for the internet, which makes it possible to demonstrate traffic moving between the private network and the outside. 
    Packet analysis was done in Packet Tracer's built-in Simulation Mode, which captures the exact traffic on the simulated topolg and allows stepping through each exchange packet by packet. 

# Topology
* Router 0 - Cicso ISR 4321 - Edge Router/ gateway (DHCP, NAT, Rotuing)
* Router 1 - Cisco 2911 - Simulated external network
* Switch0 - Cisco 2960-24TT - Connects all internal devices
* Web Server - Server-PT - Hosts HTTP/HTTPS
* DNS Server - Server-PT - Resolves www.diyanet.com
* Clents - 2 PCs, 2 Laptops - DHCP Clients

  # IP Addressing
  The internal LAN uses the private block 192.168.1.0/24, which gives plenty of usable host addresses and keeps a clean logical boundary for the whole network. The link between the two routers uses a small 203.0.113.0/30 subnet, which is the standard way to address a point-to-point link between two routers because it only needs two usable addresses. 


Device
IP Address
Subnet
Gateway
Assignment
Router0  (LAN - Gig0/0/0) 
192.168.1.1
255.255.255.0


Static
Router0 (WAN - Gig0/0/1)
203.0.113.1
255.255.255.252


Static
Router1(Gig0/0)
203.0.113.2
255.255.255.252


Static
Web Server
192.168.1.10
255.255.255.0
192.168.1.1
Static
DNS Server
192.168.1.20
255.255.255.0
192.168.1.1
Static
PC0
192.168.1.52
255.255.255.0
192.168.1.1
DHCP
PC1
192.168.1.51
255.255.255.0
192.168.1.1
DHCP
Laptop 0 & 1
192.168.1.53
255.255.255.0
192.168.1.1
DHCP


  
