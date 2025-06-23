# 🛠️ Nmap Cheatsheet
Personal cheat sheet with Nmap scans used in CTFs and pentests.

---

## 🔎 Basic Nmap Scans

| Command | Description |
|--------|-------------|
| `nmap <target>` | Basic scan on default 1000 ports |
| `nmap -p 80 <target>` | Scan specific port (80) |
| `nmap -p 1-65535 <target>` | Scan all ports |
| `nmap -p- <target>` | Shorthand for all ports |
| `nmap -sS <target>` | SYN (stealth) scan (requires sudo) |
| `nmap -sT <target>` | TCP Connect scan (no root needed) |
| `nmap -sV <target>` | Detect service version |
| `nmap -O <target>` | Detect operating system |
| `nmap -A <target>` | Aggressive scan (OS, version, traceroute, script scan) |
| `nmap -sn <target>` | Ping scan (host discovery only) |
| `nmap -T4 <target>` | Faster scan speed |
| `nmap -Pn <target>` | Skip host discovery (assumes host is up) |

---

## ⚙️ Scan Types (`-s` flags)

- `-sS`: SYN Scan – Half-open scan. Fast and stealthy.
- `-sT`: TCP Connect Scan – Full 3-way handshake. Slower, more detectable.
- `-sU`: UDP Scan – Slow, can miss results.
- `-sN`: Null Scan – No flags. Useful against some firewalls.
- `-sF`: FIN Scan – May bypass basic filtering.
- `-sX`: Xmas Scan – Sends FIN, PSH, URG flags.
- `-sA`: ACK Scan – Used to map firewall rules.
- `-sV`: Version Detection – Finds service versions.
- `-sC`: Default Scripts – Runs default NSE scripts.

---

## 🧩 Additional Flags and Their Meaning

- `-T0` to `-T5`: Timing templates (T0 = stealthy, T5 = aggressive)
- `-p-`: Scan all 65535 ports
- `-Pn`: Assume host is up (skip ping)
- `-n`: No DNS resolution
- `-D IP1,IP2,ME`: Decoy scanning
- `--source-port <port>`: Spoof source port
- `--script=<script>`: Run specific NSE script
- `--script=vuln`: Run vulnerability detection scripts
- `-oN <file>`: Save output in normal format
- `-oG <file>`: Save output in grepable format
- `-oX <file>`: Save output in XML format

---

## 🏆 Top 30 Nmap Commands

1. `nmap <target>` – Basic scan of top 1000 ports  
2. `nmap -p- <target>` – Full port scan (1–65535)  
3. `sudo nmap -sS <target>` – Stealth SYN scan  
4. `sudo nmap -sT <target>` – Full TCP connect scan  
5. `sudo nmap -sU -p 53,161 <target>` – UDP scan  
6. `sudo nmap -A <target>` – Aggressive scan  
7. `sudo nmap -O <target>` – OS detection  
8. `sudo nmap -sV <target>` – Service/version detection  
9. `sudo nmap -sC <target>` – Default scripts  
10. `nmap -Pn <target>` – Disable ping  
11. `nmap -sn <target>/24` – Ping sweep  
12. `sudo nmap -sS -T4 -p- <target>` – Fast, full stealth scan  
13. `sudo nmap -sS -T5 -f <target>` – Fragmented packets  
14. `sudo nmap -sS --source-port 53 <target>` – DNS port spoof  
15. `sudo nmap -sS --spoof-mac 0 <target>` – Random MAC  
16. `sudo nmap -sS -D IP1,IP2,ME <target>` – Decoy scan  
17. `nmap -n <target>` – No DNS  
18. `nmap -iL targets.txt` – Input list of targets  
19. `nmap -oN output.txt <target>` – Save to file  
20. `sudo nmap --script vuln <target>` – Vulnerability scan  
21. `sudo nmap --script http-enum -p 80 <target>` – Web enum  
22. `sudo nmap --script smb-os-discovery -p 445 <target>` – SMB OS info  
23. `sudo nmap --script ftp-anon -p 21 <target>` – FTP anonymous login  
24. `sudo nmap --script ssh-auth-methods -p 22 <target>` – SSH auth check  
25. `sudo nmap --script http-title -p 80 <target>` – Get web title  
26. `sudo nmap --script dns-brute <target>` – DNS brute-force  
27. `sudo nmap --script mysql-empty-password -p 3306 <target>` – MySQL password check  
28. `sudo nmap --script ssl-cert -p 443 <target>` – SSL cert info  
29. `sudo nmap -sS -p 1-1000 -T4 -v <target>` – Verbose scan  
30. `sudo nmap --script firewall-bypass <target>` – Try bypassing firewalls  

---

## 🔐 Nmap Commands for Targets Behind Proxy or Firewall

31. `nmap --proxy http://<proxy-ip>:<port> <target>` – Scan via HTTP proxy  
32. `proxychains nmap -sT -Pn <target>` – SOCKS proxy scan  
33. `nmap -f <target>` – Fragmented packets  
34. `nmap --mtu 24 <target>` – Packet fragmentation  
35. `nmap --source-port 53 <target>` – Spoof DNS port  
36. `nmap -D RND:10 <target>` – 10 random decoys  
37. `nmap -p 80,443,53 <target>` – Scan common ports  
38. `nmap --script firewall-bypass <target>` – Exploit firewall weakness  
39. `nmap -T4 -Pn <target>` – Faster scan, skip ping  
40. `nmap --data-length 100 <target>` – Random packet size

---

# 🖥️ Screen + Nmap Cheat Sheet
Personal cheat sheet for running persistent Nmap scans inside `screen` terminal sessions. Perfect for long scans on remote servers or CTF labs.

---

🔹 screen
What is it for:
It allows you to run long-running commands (like nmap scans) in a new "virtual" terminal session that doesn't stop even if you close the terminal or SSH session.

Why useful in CyberSec:
If you connect via SSH, and accidentally disconnect, your Nmap session will be terminated without a screen. With the screen, you can go back and see the results.

## ⚙️ Quick Setup & Usage

# 1. Install screen (if not already)
sudo apt update && sudo apt install screen

# 2. Start a screen session named "nmapscan"
screen -S nmapscan

# 3. Inside screen, run your Nmap scan (example: SSH scan across local subnet)
nmap -Pn -p 22 192.168.1.1-254 -oN scan22.txt

# 4. Detach from screen (let the scan run in background)
# Press: Ctrl + A, then D

# 5. Later, reattach to screen session
screen -r nmapscan

# (If multiple sessions)
screen -ls
screen -r [session_id]

# 6. When done, exit the screen session from inside
exit


## 💬 Author's Note

> This list was compiled for your own use and practice through CTF challenges and test environments like TryHackMe and Hack The Box.  
> Do not use these commands on networks without permission. 🛡️


