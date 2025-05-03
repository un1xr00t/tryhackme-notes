# [Room Name / Challenge Title]

## Basic Info
- **Platform**: TryHackMe / HTB / Other
- **IP Address**: 
- **Date**: 
- **Difficulty**: 
- **Tags**: Enumeration, Exploitation, Privilege Escalation

---

## Scanning & Enumeration

### Nmap
**Command:**
```bash
nmap -sC -sV -oN nmap.txt <IP>
```

**Result Summary:**
```
Port     Service      Version
22/tcp   ssh          OpenSSH 7.6p1
80/tcp   http         Apache 2.4.29
```

---

## Service Analysis

### Port 80 - HTTP
- **What’s running:** 
- **Interesting files/directories:** 
- **Vulnerabilities identified:**

---

## Exploitation

### Method Used:
- Vulnerability:
- Exploit Used:
- Shell Access Gained:

**Commands:**
```bash
python3 exploit.py <target>
```

---

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
