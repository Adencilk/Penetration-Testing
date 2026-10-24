# Penetration Testing Assessment Report: Linux Agency (TryHackMe)

**Date:** 24 October 2026   
**Target Machine:** TryHackMe - Linux Agency  
**Objective:** Progressively audit 30 mission-based Linux user tiers to evaluate configuration hygiene and escalate to root-level privileges.

---

## 1. Executive Summary

### 1.1 Scope & Goal
The objective of this assessment was to conduct a local security review and privilege escalation audit against the **Linux Agency** target infrastructure. The system simulates a highly segmented user environment consisting of 30 distinct mission profiles and a primary root-level supervisor configuration.

### 1.2 Key Findings Summary
* **Privilege Escalation Risk:** **CRITICAL** (Systemic misconfigurations allow lateral movement and full root compromise).
* **System Misconfiguration:** **HIGH** (Hardcoded credentials in backup files, weak environmental restrictions, and unsafe file permissions).
* **Vulnerable Binary Delegations:** **HIGH** (Abuse of specific binaries with root privileges / sudo configurations).

---

## 2. Walkthrough & Technical Progression

### Phase 1: Initial Access (`agent47`)
* **Entry Vector:** SSH connection using provided credentials (`agent47`).
* **Initial Reconnaissance Findings:** 
  * *[Insert any details about the target environment found immediately upon logging in]*
* **Commands Used:**
  ```bash
  ssh agent47@<TARGET_IP>
  # Insert other discovery commands here
  ```

---

### Phase 2: Lateral Movement (Missions 1 to 30)
*Below is a summary tracking matrix of credentials and vectors utilized to pivot dynamically across the system environment:*

| Source User | Target User | Finding / Vulnerability Location | Password / Flag Discovered |
| :--- | :--- | :--- | :--- |
| `agent47` | `mission1` | *[e.g., Hidden file in home directory]* | `mission1{...}` |
| `mission1` | `mission2` | *[e.g., Environment variable extraction]* | `mission2{...}` |
| `mission2` | `mission3` | *[e.g., Base64 decoded string]* | `mission3{...}` |
| *[...]* | *[...]* | *[...]* | *[...]* |
| `mission29`| `mission30`| *[e.g., Deciled multi-language binary output]*| `mission30{...}` |

#### Highlighted Pivot Breakdown: Mission [X] to Mission [Y]
* **Vulnerability Description:** *[Detail a specific interesting challenge here, such as a Python/Ruby script or a custom environment validation]*
* **Exploitation / Retrieval Steps:**
  ```bash
  # Insert the exact command or script used to extract the next credential
  ```

---

### Phase 3: Privilege Escalation to Root
* **Vector Discovered:** *[Identify the exact privilege escalation flaw found on the final mission user (e.g., Sudo -l misconfiguration, writable Cron job, or Docker group membership)]*
* **Exploitation Code / Proof of Concept (PoC):**
  ```bash
  # Insert the exact command string used to capture the root shell
  # Example: sudo -l  OR  docker run -v /:/mnt -it alpine sh
  ```
* **Post-Exploitation Verification:**
  ```bash
  whoami
  # Output: root
  cat /root/root.txt
  ```

---

## 3. Vulnerability Analysis & Remediation Recommendations

### 3.1 Hardcoded Credentials / Cleartext Password Rotation
* **Risk Level:** High  
* **Impact:** Allows low-privileged users to trivially read and chain horizontal privileges to reach highly sensitive accounts.
* **Remediation:** Remove plain text backups or environment files containing credential strings. Migrate user verification to asymmetric keys or a centralized Identity Provider (IDP).

### 3.2 Unsafe Sudo Binaries & Group Delegation (GTFOBins)
* **Risk Level:** Critical  
* **Impact:** Enables local users to run native administrative tools that drop into unrestricted root interactive terminal environments.
* **Remediation:** Enforce the principle of least privilege. Audit `/etc/sudoers` to restrict access to binaries that support shell spawning functions (like `less`, `more`, `vi`, or binary execution engines) unless heavily locked down via explicit configuration flags.

### 3.3 Over-Privileged Group Membership (Docker Group)
* **Risk Level:** Critical  
* **Impact:** Users inside the `docker` socket group can easily bypass standard operating system kernels protections to read/write the host filesystem directly.
* **Remediation:** Treat `docker` group membership as equivalent to root access. Avoid adding untrusted user profiles to this group. Instead, leverage rootless Docker daemons or restricted `podman` namespaces.

---

*Report Ends.*
