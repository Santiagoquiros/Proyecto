Setting up a SOC lab from scratch requires a structured approach. This guide takes you from a blank Ubuntu server to a fully functional Wazuh environment with the custom rules we discussed and automated scripts to test them.
Phase 1: Zero to Hero Installation

The fastest and most reliable way to install the full Wazuh stack (Indexer, Server, and Dashboard) is using the All-in-One assistant.

    Prepare the Server (Ubuntu 22.04/20.04):
    Bash

    sudo apt update && sudo apt upgrade -y
    sudo apt install curl apt-transport-https unzip -y

    Run the Installer:
    Bash

    curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
    sudo bash wazuh-install.sh -a

    This will take 5-10 minutes. At the end, it will print your admin password. Save it!

Phase 2: Configuring Your Custom Rules

Now we will apply the "Essential Detection Rules" we discussed earlier.

    Open the Custom Rules file:
    sudo nano /var/ossec/etc/rules/local_rules.xml

    Paste the rules: (Paste the XML blocks for SSH Brute Force, Sudo Escalation, and FIM that were provided in our previous conversation).

    Validate and Reload:
    Bash

    # Check for syntax errors
    /var/ossec/bin/wazuh-analysisd -t
    # If no errors, reload the manager
    systemctl reload wazuh-manager

Phase 3: The "Attack Sim" Scripts

Run these scripts on a monitored endpoint (the agent) to verify the rules are working.
1. Trigger SSH Brute Force (Rule 100011)

Save this as attack_ssh.sh on a different machine (or the same one to self-test):
Bash

#!/bin/bash
# Simulates 6 failed login attempts
for i in {1..6}; do
  sshpass -p "WrongPassword$i" ssh invaliduser@<TARGET_IP> -o StrictHostKeyChecking=no
  echo "Attempt $i sent..."
done

2. Trigger Suspicious Sudo (Rule 100020)

Run this directly on the agent:
Bash

sudo /usr/bin/passwd  # Simply opening the prompt triggers the 'match'
# OR
sudo /bin/bash -c "whoami"

3. Trigger Critical FIM (Rule 100030)

Note: Ensure you have <directories realtime="yes" report_changes="yes">/etc</directories> in the agent's ossec.conf.
Bash

# Add a dummy comment to the passwd file
sudo echo "# Test FIM" >> /etc/passwd

4. Trigger Malware Execution (Rule 100050)

Note: You don't need real malware. We are matching the process name.
Bash

# Create a dummy file and rename it to a known tool
cp /bin/ls /tmp/mimikatz
chmod +x /tmp/mimikatz
/tmp/mimikatz

Phase 4: Monitoring Workflow

Once you trigger an alert, follow this data flow to ensure everything is working:
Step	How to Verify
Log Receipt	Check /var/ossec/logs/archives/archives.log (if logall is enabled).
Rule Matching	Use /var/ossec/bin/wazuh-logtest to paste the raw log and see which SID fires.
Dashboard	Go to Wazuh -> Security Events and filter by Rule ID.
Final Checklist for "0 to 100"

    [ ] Port 1514/1515: Opened on Manager firewall for Agent communication.

    [ ] Auditd: Installed on Linux agents (sudo apt install auditd).

    [ ] Sysmon: Installed on Windows agents with a config (like Olaf Hartong's).

    [ ] Agent Auth: Agent is showing as "Active" in the Dashboard.

Ready to move to Active Response? I can provide the configuration to automatically block the IP address of anyone running that attack_ssh.sh script.
Since you are using Kali Linux as your attack machine, you have a powerhouse of tools (hydra, nmap, mimikatz, etc.) to test your SOC.

Below is the complete setup guide for your Ubuntu and Windows clients, followed by the specific Kali scripts to trigger the alerts we created.
1. Client Configuration (The "Blue Team" Setup)
Ubuntu Client Setup

    Install Auditd: Necessary for process and command execution tracking.
    Bash

    sudo apt install auditd -y

    Configure Wazuh Agent: Open /var/ossec/etc/ossec.conf and ensure these blocks exist under <syscheck> and <localfile>:
    XML

    <syscheck>
      <directories realtime="yes" report_changes="yes">/etc/passwd,/etc/shadow,/etc/sudoers</directories>
    </syscheck>

    <localfile>
      <location>/var/log/audit/audit.log</location>
      <log_format>audit</log_format>
    </localfile>

    Restart Agent: sudo systemctl restart wazuh-agent

Windows Client Setup

    Enable Auditing: Open PowerShell as Administrator to log account creation and logon failures.
    PowerShell

    auditpol /set /subcategory:"Logon" /success:enable /failure:enable
    auditpol /set /subcategory:"User Account Management" /success:enable /failure:enable

    Install Sysmon: This is critical for detecting Mimikatz and process execution.

        Download Sysmon.

        Install with a config (e.g., SwiftOnSecurity's config):
        .\sysmon64.exe -i sysmonconfig.xml -accepteula

    Configure Wazuh Agent: Open C:\Program Files (x86)\ossec-agent\ossec.conf and add:
    XML

    <localfile>
      <location>Microsoft-Windows-Sysmon/Operational</location>
      <log_format>eventchannel</log_format>
    </localfile>

    Restart Service: Restart-Service -Name Wazuh

2. Attack Scripts (The "Red Team" Kali Setup)

Run these from your Kali Linux machine targeting your client IPs.
Script 1: Brute Force Attack (Triggers Rule 100011 / 100041)

We will use Hydra, which is pre-installed on Kali.

    For Ubuntu (SSH):
    Bash

    # -t 4 limits threads to ensure Wazuh catches the frequency
    hydra -l root -P /usr/share/wordlists/rockyou.txt -t 4 ssh://<UBUNTU_IP>

    For Windows (RDP):
    Bash

    hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p wrongpass rdp://<WINDOWS_IP>

Script 2: Port Scanning (Triggers Rule 100081)

Use Nmap to trigger the "many blocked connections" rule.
Bash

# -T5 is "Insane" speed, making it very loud for the IDS
nmap -Pn -sS -T5 -p 1-1000 <CLIENT_IP>

Script 3: Windows Malware/Mimikatz (Triggers Rule 100051)

You don't need to actually run a virus. You just need to execute a file named "mimikatz.exe" or use Kali's built-in version if you have a shell.
If you have an SMB share or shell on the Windows machine:
Bash

# Rename any safe executable to mimikatz.exe and run it
cp C:\Windows\System32\calc.exe C:\Temp\mimikatz.exe
C:\Temp\mimikatz.exe

Script 4: Automated Lab "Chaos" Script

Save this on your Kali machine as trigger_all.sh. It will run a sequence of attacks against your Ubuntu agent to light up your Wazuh dashboard.
Bash

#!/bin/bash
TARGET="<UBUNTU_IP>"

echo "[!] Starting Attack Simulation on $TARGET..."

# 1. Trigger SSH Brute Force
echo "[+] Simulating SSH Brute Force..."
hydra -l admin -p password123 -t 8 ssh://$TARGET &
sleep 5

# 2. Trigger FIM (Requires SSH access or local execution)
echo "[+] Modifying sensitive files (FIM)..."
ssh user@$TARGET "sudo echo '# Attacker Entry' >> /etc/passwd"

# 3. Trigger Malware Detection
echo "[+] Executing 'Mimikatz' placeholder..."
ssh user@$TARGET "cp /bin/ls /tmp/mimikatz; /tmp/mimikatz"

# 4. Trigger Recon Alert
echo "[+] Running Aggressive Port Scan..."
nmap -F -T5 $TARGET

echo "[!] Simulation Complete. Check your Wazuh Dashboard!"

3. How to verify in Wazuh

    Security Events: Go to the "Security Events" module in the Wazuh Dashboard.

    Filter by Rule ID: Use the search bar to look for rule.id: 100011 (SSH Brute Force) or rule.id: 100051 (Mimikatz).

    Check Archives: If an alert didn't fire but you want to see the raw log, ensure <logall>yes</logall> is set in your Manager's ossec.conf and check /var/ossec/logs/archives/archives.json.

Pro Tip: If the alerts aren't showing up, run the command /var/ossec/bin/wazuh-logtest on the Manager and paste a sample log (like a failed login line from auth.log). It will tell you exactly which rule is (or isn't) matching.
