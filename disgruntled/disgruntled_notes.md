# Disgruntled - Incident Response Investigation

## Scenario
This TryHackMe room simulated an insider threat involving an ex-employee who may have left behind malicious artifacts on Linux system. My task was to investigate the system, analyze logs, and identify evidence of compromise. 

---

## 1. Privileged Commands Investigation
I started by checking authentication logs to identify commands executed with elevated privileges:

```bash
cat /var/log/auth* |grep -i COMMAND| head
```

### Findings: 
- The user executed:

```bash
/usr/bin/apt install 
```
- The directory the command was executed from 

## 2. Unauthorized User Creation
To continue the investigation, I ran:
```bash
cat /var/log/authlog* |grep -i COMMAND|tail 

cat /var/log/authlog* |grep -i visudo|tail
```

### Findings:
- A new user was added
- The new user was granted sudo permissions
- When the sudoers file had been modified

## 3. Bash and Vim History
I checked shell histories to uncover how the script was created:
```bash
cat /home/<user>/.bash_history
```
I also inspected for Vim history:

```bash
cat /home/<usr>/.viminfo
```

### Findings:
- The script was saved under a new name
- Who created and saved the script

## 5. File Metadata Verification
To confirm the script's timestamp, I navigated to /bin:
```bash
cd /bin
ls -al --full-time
```

### Findings:
- When the file was modified

## 6. Malicious Script Behavior
I opened the file to analyze its contents:
```bash
cat /bin/<filename>
```

### Findings: 
- The script creates a file called goodbye.txt upon execution
- Its functionality appears to be simulate a data wipe and goodbye message symbolic of a disgruntled employee's exit

## 7. Persistence Discover via Crontab
Finally, I checked scheduled tasks:

```bash
cat /etc/crontab
```

### Findings:
- The malicious file was run on a schedule 

# Summary of Actions Taken
 - Identified suspicious command execution and package installation
 - Discovered unauthorized user and related privilege escalation
 - Traced deleted script that was renamed and moved
 - Verified file modification times and persistence via cron
 - Analyzed the malicious script's behavior and potential impact

# Skills Demonstrated
- Log forensics and shell history analysis
- Linux user and privilege investigation
- File tracking and metabase inspection
- Crontab persistence detection and script analysis