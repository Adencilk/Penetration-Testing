# Rootme penetration testing report

This report was a documentation of rootme lab in TryHackMe.

## Reconnaissance
I did nmap scan of the target to identify the open ports. And I found out that 2 ports were open.
I used the command:

```bash
 nmap -A -sV <TARGET_ip>
````
nmap results:

![](screenshots/rootme_nmap.png)

### Web Directory Enumeration
I used gobuster to identify the directories in the web.

```bash
gobuster dir -u  http://<TARGET_IP> -w /usr/share/wordlists/dirb/common.txt
```

![](screenshots/rootme_gobuster.png)





