# [Ice]

## Basic Info
- **Platform**: TryHackMe 
- **IP Address**: 10.10.93.20
- **Date**: 05-04-25
- **Difficulty**: Easy
- **Tags**: Enumeration, Web Exploitation, File Upload, ColdFusion, Privilege Escalation, Windows, Reverse Shell

---

## Scanning & Enumeration

### Nmap
**Command:**
```bash
sudo nmap -sS -p- -T4 -A -oN nmap.out <ip>
sudo nmap --script vuln -p135,139,445,3389,5357,8000,49152-49154,49158-49160 -oN ice-vuln.nmap 10.10.93.20

```

**Result Summary:**
```
Nmap scan report for 10.10.93.20
Host is up (0.21s latency).
Not shown: 65500 closed tcp ports (reset)
PORT      STATE    SERVICE            VERSION
135/tcp   open     msrpc              Microsoft Windows RPC
139/tcp   open     netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open     microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
2439/tcp  filtered sybasedbsynch
2943/tcp  filtered ttnrepository
3334/tcp  filtered directv-web
3389/tcp  open     ssl/ms-wbt-server?
|_ssl-date: 2025-05-05T01:51:54+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=Dark-PC
| Not valid before: 2025-05-04T00:37:23
|_Not valid after:  2025-11-03T00:37:23
3818/tcp  filtered crinis-hb
5357/tcp  open     http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Service Unavailable
|_http-server-header: Microsoft-HTTPAPI/2.0
5515/tcp  filtered unknown
7708/tcp  filtered scinet
8000/tcp  open     http               Icecast streaming media server
|_http-title: Site doesn't have a title (text/html).
8153/tcp  filtered quantastor
49152/tcp open     msrpc              Microsoft Windows RPC
49153/tcp open     msrpc              Microsoft Windows RPC
49154/tcp open     msrpc              Microsoft Windows RPC
49158/tcp open     msrpc              Microsoft Windows RPC
49159/tcp open     msrpc              Microsoft Windows RPC
49160/tcp open     msrpc              Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=5/4%OT=135%CT=1%CU=35679%PV=Y%DS=4%DC=T%G=Y%TM=6818
OS:19BB%P=x86_64-pc-linux-gnu)SEQ(SP=100%GCD=1%ISR=10A%TI=I%CI=I%II=I%SS=S%
OS:TS=7)OPS(O1=M509NW8ST11%O2=M509NW8ST11%O3=M509NW8NNT11%O4=M509NW8ST11%O5
OS:=M509NW8ST11%O6=M509ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=
OS:2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M509NW8NNS%CC=N%Q=)T1(R=Y%DF=Y%T=80%S=O%
OS:A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF
OS:=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%
OS:RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W
OS:=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
OS:U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%D
OS:FI=N%T=80%CD=Z)

Network Distance: 4 hops
Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-05-05T01:51:48
|_  start_date: 2025-05-05T00:37:07
|_nbstat: NetBIOS name: DARK-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:f0:19:96:30:d3 (unknown)
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Dark-PC
|   NetBIOS computer name: DARK-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-05-04T20:51:48-05:00
|_clock-skew: mean: 1h15m00s, deviation: 2h30m00s, median: 0s

TRACEROUTE (using port 587/tcp)
HOP RTT       ADDRESS
1   94.58 ms  10.13.0.1
2   ... 3
4   214.17 ms 10.10.93.20

```

## Vulns

```
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
|_ssl-ccs-injection: No reply from server (TIMEOUT)
5357/tcp  open  wsdapi
8000/tcp  open  http-alt
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-vuln-cve2014-3704: ERROR: Script execution failed (use -d to debug)
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49158/tcp open  unknown
49159/tcp open  unknown
49160/tcp open  unknown

Host script results:
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143


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


## Summary/Understanding of what I've learned in this room
-
-
-
--------
