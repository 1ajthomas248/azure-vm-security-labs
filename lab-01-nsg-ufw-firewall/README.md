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

<img width="512" height="366" alt="image" src="https://github.com/user-attachments/assets/0f5af3bd-d055-4198-bec1-cfbde4c88ff3" />
<img width="512" height="240" alt="image" src="https://github.com/user-attachments/assets/b779c1a1-23d1-434a-a292-ed483bd2823a" />

### Phase 2: Network Security Group (NSG) Configuration

Configured inbound NSG rules to enforce least-privilege access:

Allowed Inbound Traffic

- SSH (port 22) access was explicitly allowed only from a single trusted public IP address using a /32 CIDR restriction. This prevents unauthorized remote access attempts from the wider internet while still permitting secure administrative access.

- HTTP (Port 80) traffic was allowed from the internet to support public access to the web server. This rule enables users to reach the Nginx service while keeping all non-essential ports closed.

Denied Traffic

- All other inbound traffic is implicitly denied by Azure’s default NSG rule. No additional deny rules were created, as the default DenyAllInbound rule already enforces this behavior.

<img width="512" height="328" alt="image" src="https://github.com/user-attachments/assets/fbc378ed-d7b4-49bc-9b78-ab65187cd0da" />

### Phase 3: Secure Access via SSH

- Connected to the VM using SSH key authentication

- Verified secure access to the VM shell

<img width="512" height="378" alt="image" src="https://github.com/user-attachments/assets/96b25fab-b3dd-499c-b507-5ec729755573" />

### Phase 4: Web Server Installation

Installed and enabled Nginx:

- Updated package lists

- Installed Nginx

- Enabled and started the service

- Verified the default Nginx page was accessible via the public IP

<img width="512" height="253" alt="image" src="https://github.com/user-attachments/assets/5bd4f0f4-48ed-47ee-a91e-732eb12b5198" />
<img width="512" height="164" alt="image" src="https://github.com/user-attachments/assets/71bc3636-cfeb-4b4a-b165-7a4781c643de" />

### Phase 5: Host-Based Firewall (UFW) Configuration

Configured UFW with secure defaults:

- Default deny for all inbound traffic

- Default allow for outbound traffic

- Explicitly allowed:

    - SSH (port 22)

    - HTTP (port 80)

Enabled UFW and verified active firewall rules.

<img width="512" height="496" alt="image" src="https://github.com/user-attachments/assets/d5424938-f159-447c-9cf5-8c975df9e004" />

### Phase 6: Validation and Testing

Confirmed the following behaviors:

- SSH access works only from the authorized IP

- HTTP access is reachable from the internet

- Non-approved ports (e.g., 8080) are blocked

- Firewall rules are enforced at both NSG and OS levels

<img width="512" height="381" alt="image" src="https://github.com/user-attachments/assets/1fd2effc-c1f0-4c33-88ff-bd07fd801cf9" />
<img width="512" height="228" alt="image" src="https://github.com/user-attachments/assets/e337c1a9-f4a8-402c-83b8-0bb0b70d2f4e" />

### Key Takeaways and Conclusion

This lab highlights the importance of layered security controls in cloud environments. Azure NSGs act as the first line of defense by filtering traffic before it reaches the VM, while UFW provides an additional layer of protection directly on the operating system. Relying on only one of these controls can leave systems vulnerable to misconfiguration or exposure.
