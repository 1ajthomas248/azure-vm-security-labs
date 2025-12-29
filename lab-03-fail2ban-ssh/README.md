# Lab 03: SSH Hardening with Fail2Ban

### Overview

This lab focuses on implementing host-based intrusion prevention on a Linux virtual machine using Fail2Ban. The goal is to add a second layer of defense beyond Azure Network Security Groups (NSGs) by detecting and responding to malicious behavior at the operating system level.

Fail2Ban monitors authentication logs in real time and automatically bans IP addresses that exhibit suspicious activity, such as repeated failed SSH login attempts.

### Objectives

- Install and configure Fail2Ban on an Azure Linux VM

- Enable and validate the sshd jail

- Understand how Fail2Ban integrates with systemd and journald

- Verify active protection without disrupting legitimate access

- Demonstrate defense-in-depth by combining cloud and host-based security

### Environment

- Cloud Provider: Microsoft Azure

- VM OS: Ubuntu Linux (systemd-based)

- VM Role: Public-facing web server

- Network Security: Azure NSGs + Linux firewall stack

- Intrusion Prevention: Fail2Ban

### Phase 1: Installing Fail2Ban

Fail2Ban was installed using the system package manager to ensure compatibility with the OS and systemd services.

```
sudo apt update
sudo apt install fail2ban -y
```

After installation, the Fail2Ban service was enabled to start automatically on boot.

```
sudo systemctl enable fail2ban
```

<img width="512" height="269" alt="image" src="https://github.com/user-attachments/assets/819e4617-86ae-4ed4-a5fa-5daac57f9e30" />

### Phase 2: Creating a Local Jail Configuration

To avoid modifying default configuration files, a local override file was created.

```
sudo nano /etc/fail2ban/jail.local
```

Minimal and intentional configuration was applied, enabling only the SSH jail.

This approach ensures:

- Default filters remain intact

- Configuration survives package updates

- Only necessary services are protected

<img width="512" height="153" alt="image" src="https://github.com/user-attachments/assets/482a7b15-1c51-4c04-a638-38f2406db3c8" />

### Phase 3: Service Debugging and Validation

During configuration, several real-world issues were encountered and resolved:

- Duplicate jail definitions

- Incorrect assumptions about log file paths

- Socket access errors related to systemd startup timing

The final resolution relied on:

- Allowing Fail2Ban to use systemd journal logging

- Keeping configuration minimal

- Restarting the service cleanly

```
sudo systemctl restart fail2ban
```

Service status was verified:

```
sudo systemctl status fail2ban
```

### Phase 4: Verifying Jail Status

The Fail2Ban client was used to confirm active monitoring of SSH.

```
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

Successful output confirmed:

- The Fail2Ban daemon is running

- The sshd jail is enabled

- Logs are being monitored correctly

- No bans are currently active (expected behavior)

<img width="512" height="196" alt="image" src="https://github.com/user-attachments/assets/e8134f94-bc82-4624-803a-b1f82bfb8d3b" />

### Security Outcome

With Fail2Ban enabled:

- Repeated SSH brute-force attempts are automatically blocked

- Malicious IPs are dynamically banned at the host level

- Legitimate administrative access remains unaffected

- The VM is protected even if cloud-layer rules are misconfigured

This setup significantly reduces attack surface for exposed Linux services.

### Key Takeaways

This lab demonstrates the importance of layered security. While Azure NSGs protect traffic at the network level, Fail2Ban provides intelligent, behavior-based protection directly on the host. Configuring Fail2Ban with systemd journaling highlights how modern Linux security tools integrate deeply with the operating system, making them both powerful and efficient when configured correctly.
