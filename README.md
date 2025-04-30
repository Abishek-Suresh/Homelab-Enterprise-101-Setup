# Homelab-Enterprise-101-Setup
<br>

![Image](https://github.com/user-attachments/assets/a7e0afa2-8eea-4a8a-8d68-d970f5f9ea40)
<br> <br>

This homelab, developed from the Project security's [courseware](https://projectsecurity.teachable.com/p/build-a-cybersecurity-homelab-a-practical-guide-to-offense-defense-enterprise-101) led by Mr.Collins Grant, will have a functional virtual network which simulates a real world enterprise network including the following business components : email server, workstations, security server, SIEM, XDR and security sandbox. Simulated an end-to-end cyberattack on this business network, capturing sensitive files and achieving persistence. Emulated a real-world enterprise environment to enhance security testing and resilience.
<br> <br>

## SETTING UP THE VIRTUAL MACHINES

- [Windows Server 2025](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025) is the Domain controller for corp.project-x-dc.com domain in our homelab.
- [Windows 11 Enterprise](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise), one of the enterprise workstation (Windows client) connected to the active directory belonging to the user: John Doe
- [Ubuntu Desktop 22.04.5 LTS (Jammy Jellyfish)](https://releases.ubuntu.com/jammy/), one of the enterprise workstation (Linux Client) connected to the active directory belonging to the user: Jane Doe
- [Ubuntu 22.04 Server](https://releases.ubuntu.com/jammy/) , is gonna be the email-svr with the postfix and s-nail configuration.
- [Ubuntu Desktop 22.04.5 LTS (Jammy Jellyfish)](https://releases.ubuntu.com/jammy/) - This virtual machine is gonna be our security box with the Wazuh Configuration.
- [Security Onion](https://securityonionsolutions.com/software/) - Alongside Ubuntu, security onion will act as another security box.
- [Kali Linux](https://www.kali.org/get-kali/#kali-installer-images) will be the attacker machine.

<br>

As you can see from the below image, virtual machines are provisioned in the virtual box.
<br>

![Image](https://github.com/user-attachments/assets/bbc1f7b6-a0bb-432a-8135-7d68f757d3f7)
<br><br>

A dedicated NAT Network with IP Range 10.0.0.0/24 is configured to all our virtual machines, and a static IP is set to every one of them within the NAT network range.
<br>

![Image](https://github.com/user-attachments/assets/d48e3436-554a-4133-98f0-7fa242df12dc)
<br><br>

# SIMULATING END-TO-END CYBER ATTACK

### CONFIGURING VULNERABALITIES IN OUR ENVIRONMENT:
- SSH is enabled and the sshd config files are configured in such a way that we allow root login access with a password authentication in both email server as well as our linux client.
- WinRM (Windows Remote Management) is configured on our windows client.
- RDP (Remote Desktop Protocol) is enabled on our domain controller machine.

<br>

### CONFIGURING DETECTION ALERTS IN SECURITY BOX (WAZUH):
- In Wazuh, we have configured three agents, windows client, linux client and our domain controller.
- Navigating to the alerting tab, we have created three monitors to monitor failed SSH login attempts, WinRM login and FIM(File Integrity Management) Integration respectively. Adding syscheck rule to local_rules.xml file helps us to monitor the 'secrets.txt' file.

<br>

### RECONNAISSANCE PHASE:

- Leveraging NMAP, we can scan our network and look for devices and open ports in the current network. After identifying the running hosts, a brute force attempt for an SSH open port is carried out using HYDRA. This brute force is carried out using rockyou.txt wordlist which is already available in Kali linux.
- After cracking the password, connection to the email server is made via SSH and further more information about the system is gathered.
- Finding about the email-svr user, we navigate into that user in the email-svr and janed@corp.project-x-dc.com is found in the mail directory.
- As an attacker, we are going to set up a spear-phishing email website impersonating a password verification website in order for us to grab the credentials from jane so that we can access the linux client via ssh. Sending an email to this user, impersonating the email server (trusted domain), this is a possible thing.

<br>

### LATERAL MOVEMENT AND PRIVILEGE ESCALATION:
- Getting the credentials from jane, now as an attacker we can laterally move from email svr workstation into the linux workstation.
- Further gathering the information and performing Nmap scan, we can see that the windows client is up and running with the open ports 5985 and 5986 belonging to the WinRM
- Leveraging <b>NetExec</b>, a powerful tool which can be used to compromise services like SMB,SSH etc, even WinRM included. Like HYDRA, NetExec takes a list of users and passwords. Access to the windows client is gained using this tool as well as Evil-WinRM, an opensource, command- line tool that provides remote shell access to windows machines over WinRM.
- After getting the access to this machine, we can then try to pivot to the domain controller. Using nmap we found out that RDP port is open. Leveraging <b>Xfreerdp</b>, a free implementation of RDP, which can be run on the command-line and comes pre-installed in kali linux, a connection is established.

<br>

### DATA EXFILTERATION:
- The 'secrets.txt' file is exported to the attacker's machine using scp successfully performing the data exfiltration.
<br>

### PERSISTENCE:
- Creating a local account in Domain controller and adding it to the administrators group will provide us privileged access.
- Implementation of a reverse shell, or even deploying the KEYLOGGER project that you can find in my other repository creates a persistent backdoor or eavsdrop.
- A scheduled task is created that runs a poweshell script daily at 12'O clock.
<br>

### CONCLUSION:
- The Project-X Home Lab provides a comprehensive environment for understanding both the offensive and defensive aspects of cybersecurity. By simulating real-world infrastructure—complete with Active Directory, Linux and Windows clients, a mail server, and security monitoring systems like Wazuh and Security Onion—this lab bridges the gap between theoretical knowledge and hands-on experience. Thanks to project security for this one.

