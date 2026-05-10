Brute Force Security Lab

Description

  This project was developed during the DIO bootcamp with the objective of studying brute force attacks in a controlled laboratory environment using Kali Linux, Medusa, Hydra and vulnerable machines.
  The laboratory simulates authentication attacks against FTP, Web applications (DVWA) and SMB services using virtualized machines in an isolated Host-Only network.

Network Configuration

- Oracle VM VirtualBox
- Host-Only Network
- Isolated communication environment

Laboratory Architecture

- Kali Linux → Offensive machine
- Metasploitable 2 → Vulnerable environment
- DVWA → Vulnerable web application
- Medusa/Hydra → Brute force tools

Service Enumeration with Nmap

The first step consisted of identifying exposed services on the target machine.

Command used

bash
nmap -sV 192.168.56.102


Identified Services

- FTP
- HTTP
- SMB
- SSH

FTP Brute Force with Medusa

A brute force simulation was performed against the FTP service using Medusa.

Command used

bash
medusa -h 192.168.56.102 -u msfadmin -P passwords.txt -M ftp

Result

The tool successfully identified valid credentials in the controlled environment.

DVWA Web Brute Force with Hydra

The DVWA security level was configured as LOW to simulate vulnerable authentication behavior.

Command used

bash
hydra -l admin -P passwords.txt 192.168.56.102 http-get-form '/dvwa/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie\: security=low; PHPSESSID=SESSION:F=Username and/or password incorrect.'

Result

Hydra identified the valid credential:

text
admin : password

Observations

During testing it was necessary to:

- include the PHP session cookie
- configure DVWA with security=low
- correctly identify the failure message returned by the application

This demonstrated the importance of understanding HTTP sessions and authentication flows in web pentesting.

SMB Password Spraying

Password spraying tests were performed against SMB services using Medusa.

Command used

bash
medusa -h 192.168.56.102 -U users.txt -P smb_passwords.txt -M smbnt

Observations

During testing, the difference between:
- `-u` → single username
- `-U` → username list

was identified and corrected.

This stage demonstrated how weak and reused credentials may compromise network services.

Security Recommendations

The following mitigation measures are recommended:

- Multi-factor authentication (MFA)
- Strong password policies
- Account lockout after failed attempts
- CAPTCHA implementation
- Rate limiting
- Security monitoring and logging
- Network segmentation
- Fail2ban deployment
- Security awareness training

Lessons Learned

This laboratory enabled practical understanding of:

- brute force attacks
- password spraying
- service enumeration
- web authentication flows
- HTTP cookies and sessions
- SMB authentication
- use of offensive security tools
- defensive mitigation strategies

The project also reinforced the importance of controlled environments for cybersecurity studies.

Disclaimer

All tests performed in this project were executed exclusively in isolated and authorized environments for educational purposes only.
