# Lab 01: Azure NSGs + Linux UFW Firewall (Web VM)

### Overview

This lab demonstrates defense-in-depth network security for a Linux-based web server hosted in Microsoft Azure. The objective is to secure inbound traffic by combining Azure Network Security Groups (NSGs) with host-based firewall rules using UFW on the virtual machine itself.

By layering cloud-level and OS-level controls, this lab mirrors real-world infrastructure hardening practices used in production environments.

### Objectives

- Deploy an Ubuntu Linux virtual machine in Azure

- Assign and configure a static public IP address

- Restrict network access using Azure Network Security Groups

- Install and configure a web server (Nginx)

- Secure the VM using Linux UFW firewall rules

- Validate that only required services are reachable

- Understand traffic flow from the internet to the VM

### Lab Environment

- Cloud Provider: Microsoft Azure

- Operating System: Ubuntu Server (LTS)

- Web Server: Nginx

- Cloud Firewall: Azure Network Security Group (NSG)

- Host Firewall: UFW

- Access Method: SSH (public key authentication)

### Architecture

Internet -> Azure Public IP -> Azure Network Security Group (NSG) -> VM Network Interface -> Linux UFW Firewall -> Nginx Web Server

This layered model ensures that traffic must be explicitly allowed at both the Azure network layer and the Linux OS layer.

### Phase 1: Virtual Machine Deployment

- Created a resource group to house the VM

- Created an Ubuntu Server VM in Azure

- Assigned a static public IP address

- Enabled SSH access for initial configuration

- Attached an NSG to the VM’s network interface

### Phase 2: Network Security Group (NSG) Configuration

Configured inbound NSG rules to enforce least-privilege access:

Allowed Inbound Traffic

- SSH (port 22) access was explicitly allowed only from a single trusted public IP address using a /32 CIDR restriction. This prevents unauthorized remote access attempts from the wider internet while still permitting secure administrative access.

- HTTP (Port 80) traffic was allowed from the internet to support public access to the web server. This rule enables users to reach the Nginx service while keeping all non-essential ports closed.

Denied Traffic

- All other inbound traffic is implicitly denied by Azure’s default NSG rule. No additional deny rules were created, as the default DenyAllInbound rule already enforces this behavior.

### Phase 3: Secure Access via SSH

- Connected to the VM using SSH key authentication

- Resolved Windows OpenSSH private key permission issues

- Verified secure access to the VM shell

### Phase 4: Web Server Installation

Installed and enabled Nginx:

- Updated package lists

- Installed Nginx

- Enabled and started the service

- Verified the default Nginx page was accessible via the public IP

### Phase 5: Host-Based Firewall (UFW) Configuration

Configured UFW with secure defaults:

- Default deny for all inbound traffic

- Default allow for outbound traffic

- Explicitly allowed:

    - SSH (port 22)

    - HTTP (port 80)

Enabled UFW and verified active firewall rules.

### Phase 6: Validation and Testing

Confirmed the following behaviors:

- SSH access works only from the authorized IP

- HTTP access is reachable from the internet

- Non-approved ports (e.g., 8080) are blocked

- Firewall rules are enforced at both NSG and OS levels


### Key Takeaways and Conclusion

This lab highlights the importance of layered security controls in cloud environments. Azure NSGs act as the first line of defense by filtering traffic before it reaches the VM, while UFW provides an additional layer of protection directly on the operating system. Relying on only one of these controls can leave systems vulnerable to misconfiguration or exposure.