# Lab 04: HTTPS with Certbot

### Overview

This lab implements HTTPS on an Ubuntu Linux web virtual machine running Nginx using Certbot and Let’s Encrypt. The goal is to secure web traffic with TLS encryption, configure automatic certificate renewal, and validate access across multiple security layers. Since no custom domain was available, the lab uses nip.io, a wildcard DNS service that maps a public IP address to a resolvable hostname, enabling real certificate issuance without purchasing a domain.

### Objectives

- Enable HTTPS on a public-facing Nginx web server

- Obtain and deploy a trusted TLS certificate using Certbot

- Configure HTTP → HTTPS redirection

- Validate certificate installation and renewal

- Troubleshoot connectivity issues across cloud, host, and application layers

### Environment

- Cloud Provider: Microsoft Azure

- VM OS: Ubuntu Linux

- Web Server: Nginx

- Certificate Authority: Let’s Encrypt

- ACME Client: Certbot (Nginx plugin)

- DNS Method: nip.io (IP-based hostname)

- Security Controls: Azure NSG, UFW, Fail2Ban

### Phase 1: HTTP Baseline Validation

Before issuing a certificate, the web server must be reachable over HTTP.

- Identified the VM’s public IP address

- Constructed a nip.io hostname using the IP format

- Verified DNS resolution and HTTP access

Example:

- Public IP: 172.174.113.108

- Hostname: 172-174-113-108.nip.io

HTTP access was confirmed via browser and command-line testing.

### Phase 2: Azure NSG Configuration for HTTPS

HTTPS requires inbound access on TCP port 443.

- Reviewed existing Network Security Group rules

- Added an inbound rule allowing TCP 443 from Internet

- Ensured SSH (TCP 22) remained restricted to a trusted IP

- Confirmed no higher-priority deny rules conflicted with HTTPS

This established the cloud-level perimeter required for TLS traffic.

### Phase 3: Certbot Installation and Certificate Issuance

Certbot and its Nginx plugin were installed to automate certificate management.

```
sudo apt update
sudo apt install -y certbot python3-certbot-nginx
```

A certificate was requested and deployed for the nip.io hostname:

```
sudo certbot --nginx -d 172-174-113-108.nip.io
```

Certbot successfully:

- Validated domain ownership

- Issued a trusted Let’s Encrypt certificate

- Installed the certificate and private key

- Updated the Nginx configuration

- Enabled HTTP → HTTPS redirection

- Configured automatic renewal

### Phase 4: HTTPS Validation

Post-deployment validation confirmed that TLS was correctly configured.

- Verified Nginx was listening on port 443

- Confirmed HTTPS responses using curl

- Tested browser access and verified trusted certificate status

Example validation:

```
sudo ss -lntp | grep ':443'
curl -I https://172-174-113-108.nip.io
```

### Phase 5: Troubleshooting Connectivity Issues (Defense-in-Depth)

During validation, HTTPS connections initially resulted in timeouts despite successful certificate issuance and Nginx listening on port 443.

A structured troubleshooting process was followed:

1. Application Layer

- Verified Nginx was actively listening on 0.0.0.0:443

- Confirmed TLS worked locally via https://localhost

2. Cloud Layer

- Revalidated Azure NSG rules allowed inbound TCP 443

- Confirmed no conflicting subnet or NIC-level NSGs

3. Host Firewall Layer

- Discovered UFW was active and blocking TCP 443

- Explicitly allowed HTTPS through the host firewall

```
sudo ufw allow 443/tcp
sudo ufw reload
```

Once UFW was updated, HTTPS connectivity was immediately restored. This reinforced the importance of validating all security layers—cloud perimeter, host firewall, and application configuration—when diagnosing access issues.

### Phase 6: Auto-Renewal Verification

Certbot configured automatic renewal using scheduled system tasks.

Renewal behavior was validated with:

```
sudo certbot renew --dry-run
```

This confirmed certificates would renew automatically without service disruption.

### Key Takeaways

This lab demonstrates a full HTTPS implementation using Let’s Encrypt and Certbot in a cloud environment. Beyond certificate issuance, it highlights real-world troubleshooting across layered security controls, including Azure NSGs and host-based firewalls. Successfully resolving HTTPS timeouts caused by UFW underscored the importance of defense-in-depth awareness and methodical diagnostics when working with secure infrastructure.
