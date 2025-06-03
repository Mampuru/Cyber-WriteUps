# **TryHackMe**

## **Snort Challenge: Live Attack**

### Difficulty: Medium - 90 mins

## **Scenario 1 | Brute-Force**
First of all, start Snort in sniffer mode and try to figure out the attack source, service and port.

Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!

Here are a few points to remember:

- Create the rule and test it with "-A console" mode. 
- Use "-A full" mode and the default log path to stop the attack.
- Write the correct rule and run the Snort in IPS "-A full" mode.
- Block the traffic at least for a minute and then the flag file will appear on your desktop.

1. Stop the attack and get the flag (which will appear on your Desktop)
    - *Generating the logs by capturing traffic*
    - `sudo snort -r dev -l ` 

    - *Reading the logs that were captured*
    - *locate all packets that is inbound traffic*
    - *look for ports that repeats often [http:80, ssh:22]*
    - *filter the traffic via the ports found*
    - `sudo snort -r snort.log.678910117`
    - `sudo snort -r snort.log.67891011 'port 80'`
    - `sudo snort -r snort.log.67891011 'port 22'`
    - `sudo nano /etc/snort/rules/local.rules`

- *The following line drops/blocks all the packets from port 22*
    - `drop tcp any 22 <> any any (msg:"SSH Brute Force"; sid:100001; rev:1;)`

2. What is the name of the service under attack?
    - *SSH*

3. What is the used protocol/port in the attack?
    - *TCP/22*


## **Scenario 2 | Reverse-Shell**
First of all, start Snort in sniffer mode and try to figure out the attack source, service and port.

Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!

Here are a few points to remember:

- Create the rule and test it with "-A console" mode. 
- Use "-A full" mode and the default log path to stop the attack.
- Write the correct rule and run the Snort in IPS "-A full" mode.
- Block the traffic at least for a minute and then the flag file will appear on your desktop.

1. Stop the attack and get the flag (which will appear on your Desktop)
    - *Generating the logs by capturing traffic*
    - `sudo snort -r dev -l ` 

    - *Processing the logs*
    - `sudo snort -r snort.log.1672697486 -X`
    - `sudo snort -r snort.log.1672697486 -X -n 10`

    - *Adding the rule to stop the attack*
    - `sudo nano /etc/snort/rules/local.rules`
    - `drop tcp 4444 <> any any (msg:"Reverse Shell Detected"; sid:100001; rev:1;)`

    - *Rule the rule against the live traffic* 
    - `sudo snort -c /ect/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 -A full`

2. What is the used protocol/port in the attack?
    - *tcp/4444*

3. Which tool is highly associated with this specific port number?
    - *Metasploit*