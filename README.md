# ICT Core Banking Project

## Overview
This project focuses on deploying a robust ICT core banking infrastructure using VMware virtualization technologies, networking devices, and high-availability mechanisms. It spans across multiple branches, ensuring each site has a reliable and scalable environment with IP planning, load balancing, and backup solutions. The infrastructure is designed to handle the unique requirements of core banking operations with minimal downtime and disaster recovery options.

## Platforms and Technology Used
- **VMware vSphere** for virtualization management
- **VMware vCenter Server** for centralized management of ESXi hosts
- **ESXi Hosts** to deploy and run virtual machines
- **vSphere Web Client** for managing virtual infrastructure
- **Mikrotik Router** for routing and network configuration
- **KEMP Load Balancer** for distributing traffic among servers
- **Veritas Backup Solution** for data backup and recovery
- **Microsoft Windows Server 2012** for DNS services and IP management
- **iSCSI Shared Storage** for centralized storage used by multiple hosts

## Branches
The infrastructure spans across three branches with specific IP planning:
- **Yangon**
- **Mandalay**
- **Nay Pyi Taw**

## Project Contents

### 1. Physical Topology
The following image represents the physical topology of the project, including network devices, servers, and other essential infrastructure components.

**Place the physical topology diagram here**
```markdown
![Physical Topology](path_to_physical_topology_image)

## 2. Logical Network Topology
The logical topology showcases how the devices communicate with each other across the network, including routing paths, VLANs, and IP planning.

Place the logical network topology diagram here

![Logical Network Topology](path_to_logical_topology_image)

3. IP Planning
Detailed IP configurations for each branch:

Yangon Branch
ISP Router: 103.24.2.1/30
Core Switch VLANs: 192.168.10.2/24, 10.0.10.1/24, etc.
Mandalay Branch
ISP Router: 103.25.2.1/24
Core Switch VLANs: 192.168.70.2/24, 10.1.10.1/24, etc.
Nay Pyi Taw Branch
ISP Router: 103.26.2.1/24
Core Switch VLANs: 192.168.100.2/24, 10.2.10.1/24, etc.
4. Configuration for Core and Access Switches
In each branch, VLANs are configured on the Core Switch (CSW) and Access Switches (ASW). VLANs are created for different departments, and EtherChannel is set up for redundancy between switches.

Example of Core Switch (CSW) Configuration in Yangon:

interface Vlan10
 ip address 10.0.10.1 255.255.255.0
 ip helper-address 10.0.10.10

Example of Access Switch (ASW) Configuration in Yangon:

interface Ethernet0/2
 switchport access vlan 300
 switchport mode access
 duplex auto

5. DHCP Configuration
DHCP is configured to dynamically assign IP addresses to devices within each VLAN. Exclusions are set for reserved IPs, and DHCP options are configured to provide subnet, gateway, and DNS information.

Example of DHCP Server Configuration:

interface Ethernet0/0
 ip address 10.0.10.10 255.255.255.0

Create DHCP pools for each VLAN.
Configure DHCP options such as Option 1 (Subnet Mask), Option 2 (Default Gateway), and Option 6 (DNS Server).

6. IGW (Internet Gateway) Configuration
Each branch has an Internet Gateway (IGW) router, which handles NAT and allows devices to communicate with the internet. NAT rules are applied to map internal IP addresses to external ones.

Example of IGW Configuration:

interface Ethernet0/1
 description "WAN"
 ip address 103.24.2.2 255.255.255.252
 ip nat outside

7. OSPF Configuration
OSPF (Open Shortest Path First) is configured on the routers to dynamically route traffic between branches and ISP routers. The configuration includes defining OSPF areas and assigning networks to these areas.

Example of OSPF Configuration:

router ospf 1
 network 192.168.10.0 0.0.0.255 area 0

Verification of OSPF:

do show run | sec ospf

8. Virtual Machine Deployment
Four ESXi hosts are deployed and configured within each branch. After deployment, IP addresses and DNS are configured. Each ESXi host is assigned a static IP and added to vCenter for centralized management.

Example of ESXi Host Configuration:

Host Name: ICT-ESXI-01
IP: 10.0.255.25/24
DNS: 10.0.255.29
Gateway: 10.0.255.254

9. ISCSI Shared Storage Configuration
ISCSI shared storage is configured to allow multiple ESXi hosts to access a central datastore. The storage is set up for redundancy and high availability.

Steps:
Install iSCSI on the storage server.
Add iSCSI targets to ESXi hosts using the vSphere Web Client.
Rescan the storage devices to detect the shared storage.
10. Load Balancer Configuration with KEMP
KEMP Load Balancer is deployed to distribute web traffic between the two ESXi hosts. The load balancer ensures that Web 1 and Web 2 servers are evenly balanced.

Steps:
Install KEMP Load Balancer on IP 10.0.255.31.
Add real servers (ESXi hosts) with IPs 10.0.255.169 for load balancing.
Bind the load balancer to the DNS server.
11. Website Deployment
Two web servers are deployed on ESXi hosts. Ubuntu Web Servers are installed, and Apache2 is configured to host websites.

Steps:
Install Ubuntu on ESXi-01 and ESXi-02.
Install Apache2 on both servers.
Configure websites and test access via the load balancer.
12. High Availability (HA) Setup
vSphere High Availability (HA) and Distributed Resource Scheduler (DRS) are enabled to ensure that virtual machines are automatically restarted on another ESXi host in case of a failure.

Steps:
Enable vSphere HA in vCenter.
Configure DRS to automatically balance resources between hosts.
13. Backup Solution with Veritas
Veritas is used to perform regular backups of the vCenter and ESXi hosts. In case of system failure, Veritas ensures that data can be restored quickly with minimal downtime.

Steps:
Deploy Veritas on Windows Server 2016.
Add storage for backup and configure backup schedules.
Link Veritas to vCenter for automated backups.
Results
High Availability: In the event of an ESXi host failure, virtual machines are migrated to other hosts within minutes, ensuring continuous availability.
Load Balancing: Traffic is distributed evenly between Web 1 and Web 2, ensuring optimal performance.
Backup: Regular backups are performed using Veritas to ensure that data can be restored in the event of a disaster.
Conclusion
This project successfully implemented a multi-branch core banking infrastructure using cutting-edge virtualization, networking, and storage technologies. The deployment achieves high availability, load balancing, and reliable data backup, making it a robust solution for core banking operations.

















