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
  
| Device                     | IP Address   | Subnet          | Gateway     | Assignment |
|----------------------------|--------------|-----------------|-------------|------------|
| Router0  (LAN - Gig0/0/0)  | 192.168.1.1  | 255.255.255.0   |             | Static     |
| Router0 (WAN - Gig0/0/1)   | 203.0.113.1  | 255.255.255.252 |             | Static     |
| Router1(Gig0/0)            | 203.0.113.2  | 255.255.255.252 |             | Static     |
| Web Server                 | 192.168.1.10 | 255.255.255.0   | 192.168.1.1 | Static     |
| DNS Server                 | 192.168.1.20 | 255.255.255.0   | 192.168.1.1 | Static     |
| PC0                        | 192.168.1.52 | 255.255.255.0   | 192.168.1.1 | DHCP       |
| PC1                        | 192.168.1.51 | 255.255.255.0   | 192.168.1.1 | DHCP       |
| Laptop 0 & 1               | 192.168.1.53 | 255.255.255.0   | 192.168.1.1 | DHCP       |

# Protocols Demonstrated 
|Protocol    |                                 Role in This Network                                |
|:---------------:|:-----------------------------------------------------------------------------------:|
| DHCP            | Automatically assigns IP, mask, gateway, and DNS to clients from a pool on Router0  |       
| DNS             | Resolves www.diyanet.com to the Web Server at 192.168.1.10                          |      
| ARP             | Maps a known IP to its MAC address before any Layer 2 data is sent                  |    
| HTTP            | Delivers the web page over an unencrypted TCP connection (port 80)                  |      
| HTTPS / SSL-TLS | Secure HTTP — negotiates an encrypted session (port 443) before transfer            |      
| ICMP            | ping and traceroute for reachability and path tracing                               |      
| NAT             | Translates private 192.168.1.x addresses to the public 203.0.113.1 (PAT / overload) |      
| TCP             | Reliable, connection-oriented transport — three-way handshake under HTTP/HTTPS      |      
| UDP             | Connectionless transport — carries DHCP and DNS traffic                             |    
| IP / Subnetting | 192.168.1.0/24 internal LAN; 203.0.113.0/30 router-to-router link                   |    

# Key Configuration 
* Router0 Interfaces
  interface gig0/0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
interface gig0/0/1
 ip address 203.0.113.1 255.255.255.252
 no shutdown

* Router0 -DHCP
  ip dhcp excluded-address 192.168.1.1 192.168.1.50
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 192.168.1.20

* Router0 - NAT
  interface gig0/0/0
 ip nat inside
interface gig0/0/1
 ip nat outside
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gig0/0/1 overload

# Testing and Verficiation 
|    Test    |           Command / Action          |                         Result                         | 
|:----------:|:-----------------------------------:|:------------------------------------------------------:|
| DHCP       | ipconfig /release → ipconfig /renew | Client receives 192.168.1.52 — DORA exchange completes |   
| ARP        | arp -d → ping 192.168.1.10          | ARP request floods, Web Server replies with its MAC    |   
| ICMP       | ping 192.168.1.10                   | 4/4 replies, 0% packet loss                            |   
| Traceroute | tracert 203.0.113.2                 | Path traced through gateway 192.168.1.1                |   
| DNS        | nslookup www.diyanet.com            | Resolves to 192.168.1.10                               |   
| HTTP       | Browse http://www.diyanet.com       | Page loads; TCP three-way handshake visible            |   
| HTTPS      | Browse https://www.diyanet.com      | Page loads over SSL/TLS encrypted session              |   
| NAT        | show ip nat translations            | Private 192.168.1.x → public 203.0.113.1               | 
# How to Open 
- Install Cisco Packet Tracer
- Download and Open CSCE3530.pkt
- Switch from RealTime to SImulation to view packet captures and see traffic flow 




  
