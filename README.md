Project Results: Wazuh in Action

### Active Agent Monitoring
Successfully established a secure connection between the Windows 11 endpoint and the Wazuh Manager.
![Wazuh Active Agent](image_f76048.png)

### Vulnerability Assessment
Identified 138 high-priority risks, allowing for targeted patch management.
![Vulnerability Report](image_f76b31.png)

### Troubleshooting Evidence
Documented the initial configuration error ('0.0.0.0' address) to show the technical resolution process.
![Ossec Log Error](image_b42bf5.png)

# SOC-Security-Operations-Center-lab
A. Project Overview
Describe the lab's purpose. Mention that it simulates a real-world enterprise environment using a SIEM (Wazuh), a firewall (pfSense), and a monitored endpoint (Windows 11).

B. Skills & Tools Demonstrated
List the technical skills you applied:

SIEM/XDR Deployment: Installed and configured Wazuh to monitor endpoints.

Vulnerability Management: Identified over 130 critical/high vulnerabilities on a Windows host.

Log Analysis: Monitored Windows Event Logs (e.g., Event ID 4625 for failed logins).

OS Hardening: Re-secured a machine after using administrative bypass techniques.

C. Technical Challenges Faced
Recruiters value problem-solving. Document your troubleshooting process:

"Encountered a critical configuration error where the agent was defaulting to '0.0.0.0' for the manager address. Resolved by manually editing the ossec.conf file with administrative privileges to point to the correct manager IP (10.0.0.101), successfully establishing an 'Active' connection."
1. The Challenge: Agent Connection Failure
After installing the Wazuh agent on the Windows 11 endpoint, the agent failed to appear in the Wazuh Dashboard.

Initial Findings:

The Wazuh service would start and then immediately stop.

Logs Analysis: Checked C:\Program Files (x86)\ossec-agent\ossec.log and discovered the error: ERROR: (4112): Invalid server address found: '0.0.0.0'.

Root Cause: The agent was not configured with the Manager's IP address, causing it to exit the registration process.

2. The Solution: Manual Configuration & Service Restoration
Because the standard management GUI was unresponsive, I performed a manual "surgical" fix of the configuration file:

Elevated Access: Opened Notepad as an Administrator to bypass system file protections.

Config Edit: Modified C:\Program Files (x86)\ossec-agent\ossec.conf, changing the <address> tag from 0.0.0.0 to the Manager's static IP: 10.0.0.101.

Service Restart: Used PowerShell to force the agent to reload the new configuration:

PowerShell

NET STOP Wazuh
NET START Wazuh
Result: The agent successfully performed a handshake and appeared as Active in the dashboard.

3. The "Incident": Administrative Lockout
During the hardening phase, I accidentally disabled the built-in Administrator account while my primary user was still a "Standard User," resulting in a total loss of administrative privileges.

Recovery Methodology (The "Utilman" Technique):

Persistence Bypass: Rebooted the VM into Windows Recovery Environment (WinRE).

Privilege Escalation: Used the Command Prompt to swap utilman.exe with cmd.exe. This allowed me to trigger a System-level command prompt from the login screen without a password.

Account Restoration: Executed net user administrator /active:yes to regain entry.

4. Final Lab Hardening
To transform this from a "glitchy" lab into a secure environment, I performed the following professional hardening steps:

Account Promotion: Permanently added the regular user to the local Administrators group.

Backdoor Closure: Restored the original utilman.exe using icacls to fix file permissions.

SIEM Verification: Confirmed that Wazuh was correctly logging these administrative changes as security events.
