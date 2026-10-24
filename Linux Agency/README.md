# TryHackMe: Linux Agency — Write-up & Lab Portfolio

Room : Linux Agency
Category : PenTest

A comprehensive walkthrough and documentation of the **Linux Agency** room on TryHackMe. This lab uses a *Hitman* theme to test deep Linux fundamentals, command-line proficiency, multi-language scripting, and advanced privilege escalation methodologies by chaining through 30 mission accounts to ultimately achieve root.

---

##  Room Objectives
* **Initial Access:** Infiltrate the system as `agent47` using established SSH credentials.
* **Linux Fundamentals:** Traverse through 30 user accounts (`mission1` to `mission30`), where each flag retrieved serves as the SSH/su password for the subsequent user account.
* **Privilege Escalation:** Break out of restricted environments and exploit systemic misconfigurations to elevate privileges from the final mission user to `root`.

---

##  Skills & Technologies Demonstrated

| Category | Tools & Techniques Used |
| :--- | :--- |
| **File System & Navigation** | `ls -la`, `find`, `grep`, `strings`, hidden files, file metadata inspection |
| **Data Manipulation** | `base64`, `xxd`, binary/hex conversion, string filtering, `cat -v` |
| **Scripting & Languages** | Python, Ruby, C compilation (`gcc`), Java (`javac`), Bash environment manipulation |
| **Privilege Escalation** | Sudo abuse (GTFOBins), Cron Job hijacking, Environment Variable injection, Docker group exploitation |
| **Network & Profiling** | Host configuration (`/etc/hosts`), `curl` profiling, local loopback enumeration |

---

##  Operational Walkthrough

### Part 1: Initial Access & Banner Reconnaissance
* **Target Credentials:** `agent47` : `640509040147`
* **Entry Vector:** Connected via SSH. Discovered the initial configuration and the `mission1` flag embedded within the environment's initialization script (`.ssh/rc`).
* **Flag Format:** `username{md5sum}` (Each flag unlocks the next user via `su username`).

### Part 2: Linux Fundamental Missions (1 - 30)
Progressed dynamically through the core operating system layer by identifying hidden data and bypassing logical blocks:
* **Obfuscation & Hidden Files:** Located flags hidden via non-printable characters (`cat -v`), alternate streams, and hidden file extensions.
* **Coding & Multi-Language Compilation:** Executed logic across multiple runtime environments including compiling standard C scripts, executing compiled Java bytecode, and analyzing Ruby script structures.
* **Decoding Operations:** Decrypted and decoded dynamic payloads utilizing Base64, Hexadecimal transformations, and binary array strings.
* **Environment Exploitation:** Manipulated restrictive shells, missing `$PATH` variable boundaries, and injected values into custom environment variables (e.g., configuring `pocket=money` to unlock restricted functions).

### Part 3: Privilege Escalation to Root
After pivoting to the final tier of mission users, analyzed systemic misconfigurations to elevate to administrative control:
* **Cron Job Hijacking:** Identified scheduled tasks running as a higher-privileged user with writeable permissions on the target script.
* **GTFOBins Exploitation:** Exploited specific binaries possessing native administrative delegation or misconfigured Sudo rules (`sudo -l`).
* **Container Breakout:** Exploited the `docker` group membership to spawn a privileged container, mounting the host's root file system (`/`) to fully compromise the host machine.

---

##  Key Takeaways
1. **Sanitize Environment Variables:** Misconfigured user environments (like custom application configurations dependent on basic inputs) introduce serious injection entry-points.
2. **Enforce Least Privilege (RBAC):** Group memberships like `docker` or loosely managed `sudo` binary permissions can completely undo rigorous network access controls.
3. **Internal Forensic Awareness:** Attackers often pivot natively using standard built-in tools. Monitoring file modifications and checking hidden user artifacts is critical for detecting lateral movement.

---

### 📜 Disclaimer
*This repository contains conceptual walk-through methodologies strictly intended for educational purposes, security research, and portfolio demonstration. All operations were performed inside an isolated, authorized lab sandbox environment.*
