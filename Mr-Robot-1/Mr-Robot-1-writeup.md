# Mr Robot 1 VulnHub Writeup
| Machine | Mr-Robot-1 |
|---|---|
| Platform | VulnHub |
| Difficulty | Intermediate |
| OS | Linux |

## Lab Information

- Lab Name: Mr Robot 1
- Platform: VulnHub
- Difficulty: Beginner / Intermediate
- Category: Web, WordPress, Linux Privilege Escalation
- Status: Completed

---

## Objective

The objective was to practice ethical hacking in a controlled lab by discovering vulnerabilities, gaining access, and escalating privileges.

---

## Tools Used

- Nmap
- Gobuster / Dirb
- Burp Suite
- WPScan
- John the Ripper
- Linux commands

---

## Steps Followed

### 1. Network Discovery

```bash
nmap -sn 192.168.29.0/24
```

### 2. Port Scan

```bash
nmap -sV -sC -p- TARGET_IP
```

### 3. Website Enumeration

I checked the website manually and searched for hidden files/directories.

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

### 4. WordPress Testing

I checked WordPress login, usernames, and possible weak authentication issues.

```bash
wpscan --url http://TARGET_IP --enumerate u
```

### 5. Initial Access

After enumeration, I used the discovered information to gain access inside the lab machine.

### 6. Privilege Escalation

I checked for privilege escalation using common Linux commands:

```bash
whoami
id
sudo -l
find / -perm -4000 2>/dev/null
```

---

## Skills Learned

- Web enumeration
- WordPress security testing
- Password cracking basics
- Linux privilege escalation
- Report writing

---

## Important Note

This writeup is only for educational and legal lab practice. I do not perform testing on real systems without permission.
