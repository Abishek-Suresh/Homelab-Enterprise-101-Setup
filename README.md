# Homelab-Enterprise-101-Setup
<br>

![Image](https://github.com/user-attachments/assets/a7e0afa2-8eea-4a8a-8d68-d970f5f9ea40)
<br> <br>

This homelab, developed from the Project security's [courseware](https://projectsecurity.teachable.com/p/build-a-cybersecurity-homelab-a-practical-guide-to-offense-defense-enterprise-101) led by Mr.Collins Grant, will have a functional virtual network which simulates a real world enterprise network including the following business components : email server, workstations, security server, SIEM, XDR and security sandbox. Simulated an end-to-end cyberattack on this business network, capturing sensitive files and achieving persistence. Emulated a real-world enterprise environment to enhance security testing and resilience.

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
<br>
