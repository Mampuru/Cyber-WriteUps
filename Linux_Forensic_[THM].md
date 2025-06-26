# **TryHackMe**

## **Linux Forensic**

### Difficulty: Medium - 120 mins

## **Task 3: OS and account information**
1. Which two users are the members of the group audio?
- `cat /etc/group | grep audio`
- *ubuntu,pulse*

2. In the attached VM, there is a user account named tryhackme. What is the uid of this account?
- `cat /etc/passwd | grep tryhackme`
- *1001*

3. A session was started on this machine on Sat Apr 16 20:10. How long did this session last?
- `last -f /var/log/wtmp`
- *01:32*

## **Task 4: System Configuration**
1. What is the hostname of the attached VM?
- `cat /etc/hostname`
- *Linux4n6*

2. What is the timezone of the attached VM?
- `cat /etc/timezone`
- *Asia/Karachi*

3. What program is listening on the address 127.0.0.1:5901?
- `netstat -natp`
- *Xtigervnc*

4. What is the full path of this program? Read about the flags used above with the netstat and ps commands in their respective man pages.
- `ps aux | grep Xtigervne`
- */usr/bin/Xtigervnc*

## **Task 5: Persistence mechanisms**
1. In the bashrc file, the size of the history file is defined. What is the size of the history file that is set for the user Ubuntu in the attached machine?
- `cat ~/.bashrc`
- *2000*

## **Task 6: Evidence of Execution**
1. The user tryhackme used apt-get to install a package. What was the command that was issued?
- `cat /var/log/auth.log* | grep -i apt-get`
- *sudo apt-get install apache2*

2. What was the current working directory when the command to install net-tools was issued?
- `cat /var/log/auth.log* | grep -i apt-get`
- */home/ubuntu*

## **Task 7: Log files**
1. Though the machine's current hostname is the one we identified in Task 4. The machine earlier had a different hostname. What was the previous hostname of the machine?
- `cd /var/log`
- `gunzip -d syslog.2.gz`
- `cat syslog.2 | grep hostname`
- *tryhackme*