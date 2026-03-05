# Linux-Log-Monitoring-Threat-Detection-Lab-
SOC Lab – Ubuntu Victim + Splunk Server

# Overview

This lab demonstrates Security Operations Center (SOC) workflow using Splunk to monitor logs from a Ubuntu server. The lab simulates real-world monitoring by collecting system and authentication logs, generating events, and creating alerts.

The main goals of the lab:

- Configure a Splunk Enterprise server on Ubuntu.

- Install a Splunk Universal Forwarder on a Ubuntu victim VM.

- Forward system logs (/var/log) from the victim to the Splunk server.

- Search, visualize, and alert on suspicious activity, such as failed login attempts.

# Lab Architecture

Component - Splunk Server 	

OS/Version:
Ubuntu 22.04

Purpose:
Central log collection, searching, reporting, and alerting

Storage:
60 GB

Component - Ubuntu Victim 

OS/ Version:
Ubuntu 22.04

Purpose:
Generates logs for monitoring

Storage:
25 GB

Networking: 
- VirtualBox host-only network connecting Splunk server ↔ Ubuntu victim

- IP addresses: Splunk server 192.168.56.2, Ubuntu victim 192.168.56.x

# Step-by-Step Implementation

# 1. Setting up the Splunk Server

<img width="623" height="389" alt="Screenshot 2026-03-05 at 2 11 54 AM" src="https://github.com/user-attachments/assets/cd86857d-ece3-4be9-b321-f999dab7e727" />

<img width="626" height="386" alt="Screenshot 2026-03-05 at 2 13 23 AM" src="https://github.com/user-attachments/assets/8d32e19f-f2a0-4570-941b-dc26b4fe69a2" />

<img width="624" height="398" alt="Screenshot 2026-03-05 at 3 03 07 AM" src="https://github.com/user-attachments/assets/bb6846bc-c3f4-4261-b6ac-7815cd34506a" />

<img width="627" height="532" alt="Screenshot 2026-03-05 at 3 03 36 AM" src="https://github.com/user-attachments/assets/39e41cd2-12ca-42c2-97d2-739388e887c7" />

- Download the Splunk Enterprise installation package for Linux using wget. This retrieves the latest version of Splunk and stores it on the Ubuntu server.
- Extract the downloaded tarball to /opt and start Splunk for the first time while accepting the license agreement. This initializes the Splunk instance and prepares it for configuration.
- Create an administrator username and password to access the Splunk Web UI. Verify that the interface is accessible via http://<splunk-server-ip>:8000.

# 2. Setting up Ubuntu Victim with Splunk Forwarder

<img width="627" height="395" alt="Screenshot 2026-03-05 at 3 10 22 AM" src="https://github.com/user-attachments/assets/d18d24cf-a21d-4188-8d5e-3d59977af6d3" />

<img width="628" height="398" alt="Screenshot 2026-03-05 at 3 10 46 AM" src="https://github.com/user-attachments/assets/2eac8add-855c-49cf-b395-08a71d2b7068" />

<img width="628" height="391" alt="Screenshot 2026-03-05 at 3 18 02 AM" src="https://github.com/user-attachments/assets/59dcac0b-fcbf-4529-b1b4-5d73ef8b65e5" />

<img width="625" height="498" alt="Screenshot 2026-03-05 at 3 18 27 AM" src="https://github.com/user-attachments/assets/c53a8d09-909f-4411-b7f2-7a50783920b9" />


<img width="629" height="428" alt="Screenshot 2026-03-05 at 3 18 46 AM" src="https://github.com/user-attachments/assets/469ef656-a0e1-41c1-b19b-f62826f47d96" />

<img width="627" height="419" alt="Screenshot 2026-03-05 at 3 21 10 AM" src="https://github.com/user-attachments/assets/b939150d-0784-4ffa-88bc-3a0d1b0f0cd6" />

<img width="627" height="423" alt="Screenshot 2026-03-05 at 3 21 33 AM" src="https://github.com/user-attachments/assets/ad9d1cca-0747-4503-b14d-89294cb2dc7d" />



- Download the Splunk Universal Forwarder .deb package on the Ubuntu victim machine. The forwarder securely sends logs to the Splunk server.
- Install the forwarder using dpkg -i, then add the Splunk server’s IP and port as a forward destination. This ensures logs are forwarded to the correct central server.
- Configure the forwarder to monitor /var/log, including system and authentication logs. Restart the forwarder to apply all settings and start forwarding logs.
- Verify the forwarder is working by checking the forwarder status with:

/opt/splunkforwarder/bin/splunk list forward-server
/opt/splunkforwarder/bin/splunk list monitor

This confirms the forwarder is actively sending logs to the Splunk server and monitoring the correct directories.

# 3. Generating Logs for Testing

<img width="628" height="420" alt="Screenshot 2026-03-05 at 3 34 50 AM" src="https://github.com/user-attachments/assets/5b743380-09f6-4ea7-a514-25495dd928b6" />

<img width="627" height="564" alt="Screenshot 2026-03-05 at 3 35 22 AM" src="https://github.com/user-attachments/assets/fac762df-1af4-4a69-b9bc-132fdd2c7cc8" />

<img width="625" height="563" alt="Screenshot 2026-03-05 at 3 35 48 AM" src="https://github.com/user-attachments/assets/e7fca78b-8306-4ea2-a0cc-cea397bbbde6" />

- Run sudo apt update to generate system activity logs. These logs simulate normal server operations for SOC monitoring.
- Use logger "SOC Lab test log message" to create custom test logs in /var/log/syslog. This allows verification that logs are properly collected.
- Simulate failed login attempts using ssh wronguser@localhost. These attempts generate authentication errors that can be monitored and alerted on in Splunk.

 index=main "SOC Lab test log message"  
index=main sourcetype=syslog "Failed password"

 If the logs appear, the forwarder and Splunk server are successfully communicating.

# 4. Searching Logs in Splunk

<img width="624" height="417" alt="Screenshot 2026-03-05 at 3 39 22 AM" src="https://github.com/user-attachments/assets/73e5ed06-e372-4d89-bc13-2e6e621550c1" />

<img width="626" height="397" alt="Screenshot 2026-03-05 at 3 39 55 AM" src="https://github.com/user-attachments/assets/d9ef1a7f-6bcd-4236-a152-5946c3b841f2" />

<img width="628" height="574" alt="Screenshot 2026-03-05 at 3 40 20 AM" src="https://github.com/user-attachments/assets/90bbad53-41df-47fb-9191-374f641507c4" />

<img width="627" height="575" alt="Screenshot 2026-03-05 at 3 40 53 AM" src="https://github.com/user-attachments/assets/cdbb2621-c82f-43b9-a15b-850b38e6ad7b" />

<img width="625" height="390" alt="Screenshot 2026-03-05 at 3 41 26 AM" src="https://github.com/user-attachments/assets/e9ea1f58-5275-444a-ad7b-03ccd6799a47" />

<img width="623" height="580" alt="Screenshot 2026-03-05 at 3 43 47 AM" src="https://github.com/user-attachments/assets/04957be5-2b59-4836-b906-0c8e3bfae12e" />

- Search all events with index=main to verify that the forwarder is sending logs correctly.
- Search for failed login attempts using index=main sourcetype=syslog "Failed password". This isolates security-relevant events for monitoring.
- Count failed logins per host using stats count by host and visualize trends over time with timechart span=1h count. These analyses help identify suspicious activity patterns.

# 5. Creating Reports, Alerts, and Dashboards

<img width="621" height="418" alt="Screenshot 2026-03-05 at 3 48 27 AM" src="https://github.com/user-attachments/assets/9f367aa6-4c7f-4470-8e2b-d7d4f43ae74f" />

<img width="622" height="525" alt="Screenshot 2026-03-05 at 3 51 45 AM" src="https://github.com/user-attachments/assets/d8726148-c80b-4a2e-af49-523ed0747bbe" />

<img width="548" height="101" alt="Screenshot 2026-03-05 at 3 52 42 AM" src="https://github.com/user-attachments/assets/224a339e-6875-4cbf-be0b-bc19e3857277" />

<img width="620" height="557" alt="Screenshot 2026-03-05 at 3 50 12 AM" src="https://github.com/user-attachments/assets/add3e10a-10dd-4b31-ac2f-07a632dc4faa" />

<img width="624" height="442" alt="Screenshot 2026-03-05 at 3 53 10 AM" src="https://github.com/user-attachments/assets/53c70235-5aef-4172-872b-fa4691dd48e5" />

- Save the failed login search as a report called Failed Login Monitor for repeated monitoring.
- Create an alert that triggers when failed logins exceed a threshold (e.g., 5 in 10 minutes). Configure actions such as console alerts or email notifications.
- Build a dashboard named Ubuntu Login Activity with panels for timecharts of failed login attempts and counts per host. Dashboards provide a live view of activity for rapid monitoring.


# 6. Results

- Logs from the Ubuntu victim were successfully forwarded to the Splunk server.
- Searches, reports, and dashboards displayed real-time system events.
- Alerts triggered correctly when thresholds were exceeded, allowing monitoring of suspicious activity.

# 7. Key Learning Points

- Understand how Splunk forwarders centralize log collection from multiple sources.
- Filter and analyze security-relevant events for proactive monitoring.
- Create saved searches, reports, alerts, and dashboards to simulate a SOC workflow.
- Implement a SOC using free tools and limited hardware.






