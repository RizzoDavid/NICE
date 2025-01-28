# Alarmingly Available ICS: Analyze & Report

## Table of Contents
1. [Introduction](#introduction)
2. [Objective](#objective)
3. [Requirements](#requirements)
4. [Design](#design)
5. [Implementation](#implementation)
6. [Testing](#testing)
7. [Conclusion](#conclusion)
8. [References](#references)

## Introduction
An anonymous report concerning a Zapp Power substation's camera feed and controls being publicly exposed has confirmed two Zapp Power electrical engineers' eerie feelings that they were being watched while performing on-site maintenance at Zapp Substation 434. You have been tasked with reviewing and reporting anything, camera included, that is publicly exposed at Zapp Power Substation 434, so the exposures can be corrected by Zapp Power HQ staff.

## Objective
Find out if anything from that substation is publicly exposed and how. Finding out how the camera is exposed is likely where you should start. If/when you find something exposed, report your findings using the incident report form. For each exposure, you'll need to report the service name, IP, port, and protocol associated with the exposure. That way Zapp HQ can quickly confirm it and schedule a fix.

## Requirements
Identify the 2 exposed services on substation 434. 

## Design
Describe the design of the project. This can include architecture diagrams, flowcharts, and other design documents.
---
External Network: 172.31.3.0/16 *SPECIAL NOTE:* The IP Range 172.31.3.0/16 is considered a public range
ISP: 172.31.3.1
ZAP-HQ: 172.31.3.5
MY Workstation: 172.31.3.3
Backup: 172.31.3.4
Substation 434: 172.31.3.2
---
Internal Substation Network: 192.168.30.0/24
pfSense: 192.168.30.254
Historian: 192.168.30.77
HMI: 192.168.30.16
IP Camera: 192.168.30.32
Sensors & Equipment: 192.168.30.2


## Implementation
'Disable port fowarding for the IP Camera'
'Disable WAN rule for port 443 allowing external access to pfSense'

## Testing
First I connected to Susbstation VPN in Windows Settings. After VPN connection was established, I then loaded up the camera Substation Camera. It is a basic HTTP website to view and control the camera. Then I loaded up the HMI controlled which is another basic HTTP site to control and moniotr stats. I then loaded up Historian remote desktop which hosts a OpenHistroian server and client. I then opened the OpenHistroian Console and take a look around. 
---
The camera and controlls are exposed to the public network. I disconnected the VPN to see if I could access these devices. The camera's local IP address does not load. The camera and controls were not available over 172.31.3.2. The substations firewall is publically available at https://172.31.3.2.
---
I then connected back to the vpn and proceeded to login to the pfsense interface. There is a portforwarding rule in place to forward trafic coming in on port 8080 to go to the IP Camera. This makes the IP Camera available at 172.31.3.2:8080.         


## Conclusion
The bulk time of the project was spent exploring the remote system. Before diving into the pfSense management interface, I performed my own reconnisence of to determine how much I could access while connected and not connected to the VPN. After determining the Management interface was exposed, I then logged into the interface. I knew the camera was publicly exposed, however; port 80 was not directed to the camera. Being that there is a long list of ports that could be possibilites, I chose to use the management interface as nmap was not available on the workstation machine. This lead to the discovery that the firewall was configured to forward port 8080 to port 80. 

| Device | Address | 
| --- | ---------- |
| IP Camera| 172.31.3.2:8000 |
| pfSense | https://172.31.3.2 |

## References
List any references or resources used in the project.