This project contains a Splunk dashboard and alerting setup for:
-  Monitoring successful SSH logins
-  Detecting brute force attacks through authentication failures
-  Tracking blocked IPs by UFW firewall
-  Sending email alerts for brute force activity and firewall events


##  Dashboard Overview

The dashboard provides real-time visibility into authentication and network security logs from Metasploitable and Kali Linux systems.

##  Queries Used
``` SPL ```
### SSH Logins

index="auth_logs" sourcetype="linux_secure" "session opened"
| rex "session opened for user (?<user>\w+)"
| timechart count by user


### Brute Force Attack

index="auth_logs" source_type="Metasploit" "authentication failure" "rhost=XX.XX.XX.XX"
| stats count by rhost

### UFW BLOCK

index=network_logs sourcetype=ufw "[UFW BLOCK]"
| rex "SRC=(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| timechart count by src_ip

