# Analyzing Traffic with Wazuh: SIEM Home Lab

## Project Overview
[cite_start]This project's main purpose is to architect a local security monitoring environment to simulate, detect, and analyze network-based attacks[cite: 32]. [cite_start]The project culminated in the engineering of custom Wazuh detection rules designed to successfully identify and alert on automated credential stuffing attempts, demonstrating a practical understanding of both offensive tactics and defensive security operations[cite: 35].

## Objectives
* [cite_start]Connect a Windows virtual machine to the Wazuh manager[cite: 36].
* [cite_start]Run a simulated attack in a controlled environment using a second Kali virtual machine on the same internal network[cite: 36].
* [cite_start]Investigate system notifications and engineer custom detection rules using custom Wazuh detection rules[cite: 36].

## Architecture & Tools
[cite_start]The lab environment consisted of three main components[cite: 33]:
* [cite_start]**Wazuh Manager:** Hosted on an Ubuntu Server virtual machine[cite: 33, 37].
* [cite_start]**Target Endpoint:** A Windows 10 virtual machine configured with the Wazuh agent and Sysmon for advanced endpoint telemetry[cite: 33].
* [cite_start]**Attacker Machine:** A Kali Linux virtual machine utilizing tools like Nmap for reconnaissance and Hydra for SSH brute-force attacks[cite: 33, 34].

## Methodology & Walkthrough

### 1. SIEM Deployment & Endpoint Configuration
* [cite_start]Installed the open-source SIEM software Wazuh on an Ubuntu server and configured an Ubuntu GUI for easier dashboard management[cite: 37, 89].
* [cite_start]Installed the Wazuh agent on the Windows target VM to establish telemetry with the manager[cite: 136].
* [cite_start]Downloaded and configured Sysmon on the Windows machine using a custom GitHub configuration file for consolidated and efficient logging[cite: 185, 367, 396].

!<img width="975" height="800" alt="image" src="https://github.com/user-attachments/assets/836149d2-6f4e-4227-a302-cd1e31757685" />


### 2. Vulnerability Provisioning & Log Ingestion
* [cite_start]Installed an SSH client and server on the Windows target and established a firewall rule to allow port 22 connections[cite: 439, 679].
* [cite_start]Modified the Wazuh `ossec.conf` file to change `<logall_json>` to `yes`, ensuring all logs originating from the Windows agent were actively monitored[cite: 732, 733].

!<img width="975" height="646" alt="image" src="https://github.com/user-attachments/assets/f77c55ce-3717-4045-8277-78242528da6b" />


### 3. Attack Simulation
* [cite_start]Utilized **Nmap** (`nmap -p 22 -Pn <Target_IP>`) to verify the SSH port was open and actively accepting connections[cite: 838, 854].
* [cite_start]Executed an SSH brute-force attack using **Hydra** paired with the `rockyou.txt` password dictionary[cite: 838].

!<img width="975" height="387" alt="image" src="https://github.com/user-attachments/assets/3254f18b-1fd6-45b7-9e18-962383796213" />


### 4. Custom Detection Engineering
* [cite_start]Created a custom rule in the Wazuh `local_rules.xml` directory designed to trigger a high-level alert when an SSH brute-force attempt happens more than 5 times within a 60-second window[cite: 904].

!<img width="975" height="574" alt="image" src="https://github.com/user-attachments/assets/563c0038-a7ba-4f82-b86d-8ba187f89ff9" />


## Security Alert Analysis & Incident Triage

[cite_start]With Sysmon enhancing the logging capabilities and password authentication actively enforced, the Wazuh manager successfully ingested the resulting authentication failures[cite: 1205].

* [cite_start]**Detection:** The malicious activity triggered the custom rule engineered to alert when more than five failed SSH login attempts occur within 60 seconds[cite: 1206].
* [cite_start]**Investigation:** Reviewing the SIEM logs revealed a high volume of `Event ID 5` (Logon Failures) originating from a single internal IP address[cite: 1207]. [cite_start]The rapid succession of these attempts showed a clear indication of an automated script pointing toward a compromise rather than standard user error[cite: 1208].
* [cite_start]**Remediation:** In a live production environment, immediate containment would involve blocking the offending IP address at the firewall level and verifying that there were no successful logon events[cite: 1209]. [cite_start]Finally, the targeted account's password would be reset to comply with the organization's standard password policy[cite: 1209].

!<img width="975" height="550" alt="image" src="https://github.com/user-attachments/assets/0c077e28-222d-4320-9d1b-3420bb630d34" />
