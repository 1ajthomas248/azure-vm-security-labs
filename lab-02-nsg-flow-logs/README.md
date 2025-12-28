# Lab 02: Virtual Network Flow Logs (NSG Traffic Monitoring)

### Overview

This lab builds on Lab 01 by introducing network visibility and monitoring through Azure Virtual Network Flow Logs. While Lab 01 focused on restricting traffic using Azure Network Security Groups (NSGs) and host-based firewalls, this lab focuses on observing and validating how those rules are enforced in practice.

Virtual Network Flow Logs capture metadata about network traffic decisions, allowing administrators to verify allowed and denied connections and understand real-world traffic patterns targeting cloud resources.

### Objectives

- Enable Virtual Network Flow Logs using Azure Network Watcher

- Capture network traffic decisions at the VNet level

- Store flow log data in Azure Blob Storage

- Generate both allowed and denied traffic

- Interpret flow log records to validate NSG behavior

- Gain visibility into unsolicited internet traffic targeting a public VM

### Lab Environment

- Cloud Provider: Microsoft Azure

- Virtual Network: vm-web-01-vnet

- Virtual Machine: Ubuntu Linux web server from Lab 01

- Security Controls: Azure Network Security Groups

- Logging Service: Azure Network Watcher

- Storage Backend: Azure Blob Storage

### Phase 1: Virtual Network Flow Log Enablement

Virtual Network Flow Logs were enabled using Azure Network Watcher, which represents Azure’s current and recommended approach to network traffic logging.

Key configuration actions:

- Selected the VM’s Virtual Network as the logging target

- Enabled flow logging at the VNet scope

- Configured an Azure Blob Storage account as the log destination

- Set log retention to 7 days

- Used the latest available flow log schema version

This configuration allows NSG traffic decisions to be captured as traffic traverses the virtual network.

<img width="512" height="362" alt="image" src="https://github.com/user-attachments/assets/6c8a172d-8e18-4e4d-b2a0-ffbbd872b4b0" />
<img width="512" height="382" alt="image" src="https://github.com/user-attachments/assets/e7b6dc8b-b6f1-4733-a548-6ba37c9305a2" />

### Phase 2: Traffic Generation

To ensure meaningful log data, both allowed and denied traffic was generated:

- SSH connections from a trusted public IP

- HTTP requests to the Nginx web server

- Manual connection attempts to non-allowed ports (e.g., port 8080)

- Passive observation of unsolicited internet scans

This approach mirrors real-world exposure of a public cloud VM.

<img width="512" height="361" alt="image" src="https://github.com/user-attachments/assets/3274ef10-696b-4ea3-8544-0d89cffb84cf" />

### Phase 3: Log Collection and Storage

Once enabled, flow logs were automatically written to Azure Blob Storage under a system-generated container.

Logs are organized hierarchically by:

- Resource ID

- Year, month, day, and hour

Each log file contains structured JSON records describing network flow decisions.

<img width="512" height="150" alt="image" src="https://github.com/user-attachments/assets/b0b6df37-31cc-4946-af3b-7554646cbb48" />

### Phase 4: Log Analysis and Validation

Flow log JSON files were reviewed to validate NSG behavior and traffic patterns. The JSON file uused is also in the repo.

Allowed SSH Traffic

Rule hit: UserRule_allow-ssh-my-ip
```
1766905827009,71.62.1.247,10.1.0.4,49722,22,6,I,B,NX,0,0,0,0
```

Allowed HTTP Traffic

Rule hit: UserRule_allow-http
```
1766906015067,71.62.1.247,10.1.0.4,49717,80,6,I,E,NX,32,4881,31,3379
```

Denied Inbound Traffic

Rule hit: DefaultRule_DenyAllInBound
```
1766905987032,71.62.1.247,10.1.0.4,49734,8080,6,I,D,NX,0,0,0,0
```

This analysis confirmed that only explicitly permitted traffic reached the VM. 

### Key Takeaways

This lab demonstrates that security without visibility is incomplete. While NSGs enforce access controls, Virtual Network Flow Logs provide the necessary insight to validate those controls, detect abnormal traffic patterns, and support auditing or forensic analysis.

Together, Labs 01 and 02 illustrate both preventive and detective cloud security controls in a real-world Azure environment.
