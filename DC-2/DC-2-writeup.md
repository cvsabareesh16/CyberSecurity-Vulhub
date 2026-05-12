# DC-2 VulnHub Writeup
| Machine | DC-2 |
|---|---|
| Platform | VulnHub |
| Difficulty | Intermediate |
| OS | Linux |

## Lab Information



---

## Objective

The objective of this lab was to identify the target machine, enumerate services, find vulnerabilities, gain initial access, and escalate privileges in a legal practice environment.

---

## Tools Used

- Nmap
- Gobuster / Dirb
- WordPress enumeration tools
- John the Ripper
- Linux privilege escalation commands

---

## Methodology

### 1. Host Discovery

I first identified the target IP address in my lab network.

```bash
netdiscover
```

or

```bash
nmap -sn 192.168.29.0/24
```

---

### 2. Port Scanning

After finding the target IP, I scanned open ports.

```bash
nmap -sV -sC -p- TARGET_IP
```

### Findings

- HTTP service was running
- SSH service was available
- Web application was the main attack surface

---

### 3. Web Enumeration

I opened the website in a browser and checked the pages manually.

I also performed directory enumeration:

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

---

### 4. WordPress Enumeration

The website appeared to be WordPress-based. I checked users and possible weak points.

```bash
wpscan --url http://TARGET_IP --enumerate u
```

---

### 5. Password Attack / Initial Access

After finding valid usernames, I tested weak password possibilities in the lab environment.

I used password cracking methods only inside the legal VulnHub lab.

---

### 6. Privilege Escalation

After getting access, I checked:

```bash
whoami
id
sudo -l
uname -a
```

Then I looked for misconfigurations and privilege escalation paths.

---

## What I Learned

- How to scan a target properly
- How to enumerate a WordPress website
- How weak credentials can lead to access
- How Linux privilege escalation works
- Why documentation is important in cybersecurity

---

## Final Notes

This lab helped me understand the real penetration testing flow:

Recon → Enumeration → Exploitation → Privilege Escalation → Reporting

