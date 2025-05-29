# **TryHackMe**

## **Zeek Exercises**

### Difficulty: Medium - 60 mins

## **Anomalous DNS**
An alert triggered: "Anomalous DNS Activity".

The case was assigned to you. Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive. 

1. Investigate the dns-tunneling.pcap file. Investigate the dns.log file. What is the number of DNS records linked to the IPv6 address?

2. Investigate the conn.log file. What is the longest connection duration?

3. Investigate the dns.log file. Filter all unique DNS queries. What is the number of unique domain queries?

4. There are a massive amount of DNS queries sent to the same domain. This is abnormal. Let's find out which hosts are involved in this activity. Investigate the conn.log file. What is the IP address of the source host?

## **Phishing**
An alert triggered: "Phishing Attempt".

The case was assigned to you. Inspect the PCAP and retrieve the artefacts to confirm this 

Answer the questions below

1. Investigate the logs. What is the suspicious source address? Enter your answer in defanged format.

2. Investigate the http.log file. Which domain address were the malicious files downloaded from? Enter your answer in defanged format.

3. Investigate the malicious document in VirusTotal. What kind of file is associated with the malicious document?

4. Investigate the extracted malicious .exe file. What is the given file name in Virustotal?

5. Investigate the malicious .exe file in VirusTotal. What is the contacted domain name? Enter your answer in defanged format.

Investigate the http.log file. What is the request name of the downloaded malicious .exe file?

## **Log4J**
An alert triggered: "Log4J Exploitation Attempt".

The case was assigned to you. Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive. 

1. Investigate the log4shell.pcapng file with detection-log4j.zeek script. Investigate the signature.log file. What is the number of signature hits?

2. Investigate the http.log file. Which tool is used for scanning?

3. Investigate the http.log file. What is the extension of the exploit file?

4. Investigate the log4j.log file. Decode the base64 commands. What is the name of the created file?