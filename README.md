# Basic VLAN Setup Homelab
## Download Packet Tracer Lab
[Download Part 4: Basic VLAN Configuration](Basic%20Vlan%20Config.pkt)

## Objectives
- To set up an office network with four VLANs: one for regular office staff, one for HR,
 one for IT and one for VoIP (Voice over IP) devices like IP phones. A core switch will
 Connect two access switches, and you will test connectivity between them

## Software Requirements
- Cisco Packet Tracer

---

## Lab Setup



<p align="center">
  <img src="https://i.postimg.cc/sxZY60mg/Basic-Data-and-Voice-VLAN.png" alt="Basic Data and Voice VLAN Setup"/>
</p>

# Cisco VLAN and Voice Network Setup

This README guides you through configuring a network with multiple PCs, IP phones, and switches, including VLAN creation, trunk configuration, and port assignments.

Step 1: Add Devices to the Workspace
- Open Cisco Packet Tracer.
- From the End Devices section, drag and drop the following devices onto the workspace:
  - 5 PCs: PC-0, PC-1, PC-2, PC-3, PC-5
  - 2 IP Phones: IP Phone 0, IP Phone 1
- From the Network Devices section, drag and drop:
  - 3 Switches: Switch-0, Switch-1, Switch-2

Step 2: Physically Connect Devices

Wired Connections:
- Click on the cable icon and select a copper straight-through cable.
- Connect devices as follows:
  - PC-0 to Switch-0 (FastEthernet 0/1)
  - IPPhone0 to Switch-0 (FastEthernet 0/2)
  - PC-1 to IPPhone0 (PC to FastEthernet 0)
  - IPPhone1 to Switch-0 (FastEthernet 0/6)
  - PC-2 to IPPhone1 (PC to FastEthernet 0)
  - PC-3 to Switch-1 (FastEthernet 0/1)
  - PC-2 to Switch-1 (FastEthernet 0/2)
- Connect the Access Switches to the Core Switch:
  - Switch-0 to Switch-2 (GigabitEthernet 0/1 to GigabitEthernet 0/1)
  - Switch-1 to Switch-2 (GigabitEthernet 0/1 to GigabitEthernet 0/2)

Step 3: Configure the Core Switch
- Click on Switch-2 and go to the CLI tab.
- Configure the hostname:
  - Switch-2> enable
  - Switch-2# configure terminal
  - Switch-2(config)# hostname CoreSwitch
- Create VLANs:
  - CoreSwitch(config)# vlan 10
  - CoreSwitch(config-vlan)# name Office
  - CoreSwitch(config)# vlan 20
  - CoreSwitch(config-vlan)# name HR
  - CoreSwitch(config)# vlan 30
  - CoreSwitch(config-vlan)# name IT
  - CoreSwitch(config)# vlan 40
  - CoreSwitch(config-vlan)# name Voice
  - CoreSwitch(config-vlan)# exit
- Configure Trunk Ports for Access Switches:
  - Access Switch 1 trunk:
    - CoreSwitch(config)# int gi0/1
    - CoreSwitch(config-if)# switchport mode trunk
    - CoreSwitch(config-if)# switchport trunk allowed vlan all
  - Access Switch 2 trunk:
    - CoreSwitch(config)# int gi0/2
    - CoreSwitch(config-if)# switchport mode trunk
    - CoreSwitch(config-if)# switchport trunk allowed vlan all
- Save configuration:
  - CoreSwitch# write memory

Step 4: Configure VLANs on Access Switch 1
- Click on Switch-0 and go to the CLI tab.
- Configure the hostname:
  - Switch-0> enable
  - Switch-0# configure terminal
  - Switch-0(config)# hostname Access1
- Create VLANs:
  - Access1(config)# vlan 10
  - Access1(config-vlan)# name Office
  - Access1(config)# vlan 20
  - Access1(config-vlan)# name HR
  - Access1(config)# vlan 40
  - Access1(config-vlan)# name Voice
  - Access1(config-vlan)# exit
- Configure uplink to Core Switch as trunk:
  - Access1(config)# int gi0/1
  - Access1(config-if)# switchport mode trunk
  - Access1(config-if)# switchport trunk allowed vlan 10,20,40
- Assign access and voice ports for Office VLAN 10:
  - Access1(config)# int range fa0/1-5
  - Access1(config-if)# switchport mode access
  - Access1(config-if)# switchport access vlan 10
  - Access1(config-if)# switchport voice vlan 40
- Assign access and voice ports for HR VLAN 20:
  - Access1(config)# int range fa0/6-10
  - Access1(config-if)# switchport mode access
  - Access1(config-if)# switchport access vlan 20
  - Access1(config-if)# switchport voice vlan 40
- Save configuration:
  - Access1# write memory

Step 5: Configure VLANs on Access Switch 2
- Click on Switch-1 and go to the CLI tab.
- Configure the hostname:
  - Switch-1> enable
  - Switch-1# configure terminal
  - Switch-1(config)# hostname Access2
- Create VLANs:
  - Access2(config)# vlan 30
  - Access2(config-vlan)# name IT
  - Access2(config-vlan)# exit
- Configure uplink to Core Switch as trunk:
  - Access2(config)# int gi0/2
  - Access2(config-if)# switchport mode trunk
  - Access2(config-if)# switchport trunk allowed vlan 30
- Assign access ports for IT VLAN 30:
  - Access2(config)# int range fa0/1-5
  - Access2(config-if)# switchport mode access
  - Access2(config-if)# switchport access vlan 30
- Save configuration:
  - Access2# write memory

Step 6: Set Up Static IP Address for All PCs
- Click on each PC, go to the Desktop tab, and open IP Configuration.
- Assign the appropriate static IP address and subnet mask for the subnet.
- Repeat this for all remaining PCs.

Step 7: Testing the Configuration
- Test connectivity by pinging PCs in the same VLAN:
  - PCs in the same VLAN should successfully ping each other.
- Test connectivity between PCs in different VLANs:
  - PCs in different VLANs should fail to ping each other.
## Access Switch vs Core Switch



<p align="center">
  <img src="https://i.postimg.cc/wv5y18wJ/a-S-vs-CSW.png" alt="Access Switch vs Core Switch Topology"/>
</p>

## Data VLAN vs Voice VLAN

<p align="center">
  <img src="https://i.postimg.cc/3RhW5f3w/Data-VLAN-vs-Voice-VLA.png" alt="Data VLAN vs Voice VLAN Setup"/>
</p>
