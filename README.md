# Azure Web VM Security Labs

### Overview

This repository contains a progressive series of hands-on labs focused on securing a Linux-based web server in Microsoft Azure. The labs are designed to demonstrate real-world defense-in-depth principles by combining cloud-level network controls with host-based security configurations.

Each lab builds on the same foundational infrastructure and expands its security posture step by step, mirroring how production environments are hardened over time.

### Objectives

The goals of this lab series are to:

- Reinforce Azure networking fundamentals and traffic flow

- Implement and manage Network Security Groups (NSGs)

- Secure Linux servers using host-based firewalls

- Apply layered security controls to protect web workloads

- Validate and troubleshoot network access and restrictions

- Document infrastructure and security decisions clearly and professionally

### Lab Environment

- Cloud Platform: Microsoft Azure

- Operating System: Ubuntu Server (LTS)

- Workload: Nginx Web Server

- Security Controls: Azure NSGs, Linux UFW, additional security tooling in later labs

- Access Method: SSH with IP-based restrictions

### Labs Included

### Lab 01: Azure NSGs + Linux UFW Firewall

Focus: Network access control and host-based firewalling

- Deploy a Linux web VM in Azure

- Configure Azure Network Security Group (NSG) inbound rules

- Restrict SSH access by source IP

- Install and configure UFW on the VM

- Allow only required services (SSH, HTTP)

- Validate layered firewall behavior

### Lab 02: NSG Flow Logs and Traffic Monitoring

Focus: Network visibility and auditing

- Enable NSG Flow Logs

- Capture allowed and denied traffic

- Analyze network flow behavior

- Understand how Azure logs network activity at scale

### Lab 03: SSH Hardening with Fail2Ban

Focus: Host-based intrusion prevention

- Install and configure Fail2Ban

- Protect SSH from brute-force attacks

- Observe automatic IP banning behavior

- Complement cloud-level controls with OS-level security

### Lab 04: HTTPS with Certbot

Focus: Secure web traffic

- Configure HTTPS using Let’s Encrypt and Certbot

- Redirect HTTP to HTTPS

- Open and validate secure ports

- Verify encrypted traffic flow through NSGs and UFW

### Key Concepts Demonstrated

- Defense in depth

- Cloud vs host-based firewalling

- Principle of least privilege

- Secure network design

- Traffic validation and troubleshooting

- Infrastructure documentation