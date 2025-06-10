# **TryHackMe**

## **Wireshark: Traffic Analysis**

### Difficulty: Medium - 120 mins

## **Task 2:Nmap Scans**
Use the "Desktop/exercise-pcaps/nmap/Exercise.pcapng" file.
1. What is the total number of the "TCP Connect" scans?

2. Which scan type is used to scan the TCP port 80?

3. How many "UDP close port" messages are there?

4. Which UDP port in the 55-70 port range is open?

## **Task 3: ARP Poisoning & Man In The Middle!**
Use the "Desktop/exercise-pcaps/arp/Exercise.pcapng" file.
1. What is the number of ARP requests crafted by the attacker?

2. What is the number of HTTP packets received by the attacker?

3. What is the number of sniffed username&password entries?

4. What is the password of the "Client986"?

5. What is the comment provided by the "Client354"?

## **Task 4: Identifying Hosts: DHCP, NetBIOS and Kerberos**
Use the "Desktop/exercise-pcaps/dhcp-netbios-kerberos/dhcp-netbios.pcap" file.
1. What is the MAC address of the host "Galaxy A30"?

2. How many NetBIOS registration requests does the "LIVALJM" workstation have?

3. Which host requested the IP address "172.16.13.85"?

Use the "Desktop/exercise-pcaps/dhcp-netbios-kerberos/kerberos.pcap" file.
4. What is the IP address of the user "u5"? (Enter the address in defanged format.)

5. What is the hostname of the available host in the Kerberos packets?

## **Task 5: Tunneling Traffic: DNS and ICMP**
Use the "Desktop/exercise-pcaps/dns-icmp/icmp-tunnel.pcap" file.
1. Investigate the anomalous packets. Which protocol is used in ICMP tunnelling?

Use the "Desktop/exercise-pcaps/dns-icmp/dns.pcap" file.
2. Investigate the anomalous packets. What is the suspicious main domain address that receives anomalous DNS queries? (Enter the address in defanged format.)

## **Task 6: Cleartext Protocol Analysis: FTP**
Use the "Desktop/exercise-pcaps/ftp/ftp.pcap" file.
1. How many incorrect login attempts are there?

2. What is the size of the file accessed by the "ftp" account?

3. The adversary uploaded a document to the FTP server. What is the filename?

4. The adversary tried to assign special flags to change the executing permissions of the uploaded file. What is the command used by the adversary?

## **Task 7: Cleartext Protocol Analysis: HTTP**
Use the "Desktop/exercise-pcaps/http/user-agent.cap" file.
1. Investigate the user agents. What is the number of anomalous  "user-agent" types?

2. What is the packet number with a subtle spelling difference in the user agent field?

Use the "Desktop/exercise-pcaps/http/http.pcapng" file.
3. Locate the "Log4j" attack starting phase. What is the packet number?

4. Locate the "Log4j" attack starting phase and decode the base64 command. What is the IP address contacted by the adversary? (Enter the address in defanged format and exclude "{}".)

## **Task 8: Encrypted Protocol Analysis: Decrypting HTTPS**
Use the "Desktop/exercise-pcaps/https/Exercise.pcap" file.
1. What is the frame number of the "Client Hello" message sent to "accounts.google.com"?

2. Decrypt the traffic with the "KeysLogFile.txt" file. What is the number of HTTP2 packets?

3. Go to Frame 322. What is the authority header of the HTTP2 packet? (Enter the address in defanged format.)

4. Investigate the decrypted packets and find the flag! What is the flag?

## **Task 9: Bonus: Hunt Cleartext Credentials!**
Use the "Desktop/exercise-pcaps/bonus/Bonus-exercise.pcap" file.
1. What is the packet number of the credentials using "HTTP Basic Auth"?

2. What is the packet number where "empty password" was submitted?

## **Task 10: Bonus: Actionable Results!**
Use the "Desktop/exercise-pcaps/bonus/Bonus-exercise.pcap" file.
1. Select packet number 99. Create a rule for "IPFirewall (ipfw)". What is the rule for "denying source IPv4 address"?

2. Select packet number 231. Create "IPFirewall" rules. What is the rule for "allowing destination MAC address"?