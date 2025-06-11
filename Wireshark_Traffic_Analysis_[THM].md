# **TryHackMe**

## **Wireshark: Traffic Analysis**

### Difficulty: Medium - 120 mins

## **Task 2:Nmap Scans**
Use the "Desktop/exercise-pcaps/nmap/Exercise.pcapng" file.
1. What is the total number of the "TCP Connect" scans?
    - `tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024`
    - *1000*

2. Which scan type is used to scan the TCP port 80?
    - `tcp.connect`

3. How many "UDP close port" messages are there?
    - `icmp.type==3 and icmp.code==3`
    - *1083*

4. Which UDP port in the 55-70 port range is open?
    - `udp.dstport in {55..70}`
    - *68*

## **Task 3: ARP Poisoning & Man In The Middle!**
Use the "Desktop/exercise-pcaps/arp/Exercise.pcapng" file.
1. What is the number of ARP requests crafted by the attacker?
    - `arp.opcode==1 and eth.src==00:0c:29:e2:18:b4`
    - *283*

2. What is the number of HTTP packets received by the attacker?
    - `eth.dst==00:0c:29:e2:18:b4 and http`
    - *90*

3. What is the number of sniffed username&password entries?
    - `eth.dst==00:0c:29:e2:18:b4`
    - `http.host==testphp.vulnweb.com and http.request.method==POST and urlencoded-form contains "uname"`
    - `http.request.full_uri=="http://test.phpvulweb.com/userinfo.php" and http.request.method == POST and urlencoded-form contains "uname"`
    - *6*

4. What is the password of the "Client986"?
    - *clientnothere!*

5. What is the comment provided by the "Client354"?
    - `http.host==testphp.vulweb.com and http.request.method==POST`
    - *Nice work!*

## **Task 4: Identifying Hosts: DHCP, NetBIOS and Kerberos**
Use the "Desktop/exercise-pcaps/dhcp-netbios-kerberos/dhcp-netbios.pcap" file.
1. What is the MAC address of the host "Galaxy A30"?
    - `dhcp.option.hostname contains "Galaxy and dhcp.option.hostname contains "A30"`
    - *9a:81:41:cb:96:6c*

2. How many NetBIOS registration requests does the "LIVALJM" workstation have?
    - `nbns.name contains LIVALJM`
    - `nbns.name contains "LIVALJM" and nbns.flags in {0x2810 0x2910}`
    - *16*

3. Which host requested the IP address "172.16.13.85"?
    - `dhcp.option.requested_ip_address==172.16.13.85`
    - *Galaxy-A12*

Use the "Desktop/exercise-pcaps/dhcp-netbios-kerberos/kerberos.pcap" file.
4. What is the IP address of the user "u5"? (Enter the address in defanged format.)
    - `kerberos.CNameString=="u5"`
    - *10[.]1[.]12[.]2*

5. What is the hostname of the available host in the Kerberos packets?
    - `kerberos.CNameString contains "$"`
    - *xp1$*

## **Task 5: Tunneling Traffic: DNS and ICMP**
Use the "Desktop/exercise-pcaps/dns-icmp/icmp-tunnel.pcap" file.
1. Investigate the anomalous packets. Which protocol is used in ICMP tunnelling?
    - *SSH*

Use the "Desktop/exercise-pcaps/dns-icmp/dns.pcap" file.

2. Investigate the anomalous packets. What is the suspicious main domain address that receives anomalous DNS queries? (Enter the address in defanged format.)
    - `dns.qry.name.len > 15 and !mdns`
    - *dataexfil[.]com*

## **Task 6: Cleartext Protocol Analysis: FTP**
Use the "Desktop/exercise-pcaps/ftp/ftp.pcap" file.
1. How many incorrect login attempts are there?
    - `ftp.response.code == 530`
    - *737*

2. What is the size of the file accessed by the "ftp" account?
    - `ftp.response.code == 213`
    - *39424*

3. The adversary uploaded a document to the FTP server. What is the filename?
    - `ftp.request.command == "STOR"` <= CORRECT command
    - `ftp.request.command == "RETER"` <= THM answer command
    - *resume.doc*

4. The adversary tried to assign special flags to change the executing permissions of the uploaded file. What is the command used by the adversary?
    - *CHMOD 777*

## **Task 7: Cleartext Protocol Analysis: HTTP**
Use the "Desktop/exercise-pcaps/http/user-agent.cap" file.
1. Investigate the user agents. What is the number of anomalous  "user-agent" types?
    - `http.user_agent`
    - *6*

2. What is the packet number with a subtle spelling difference in the user agent field?
    - *52*

Use the "Desktop/exercise-pcaps/http/http.pcapng" file.
3. Locate the "Log4j" attack starting phase. What is the packet number?
    - `http.request.method == POST`
    - *444*

4. Locate the "Log4j" attack starting phase and decode the base64 command. What is the IP address contacted by the adversary? (Enter the address in defanged format and exclude "{}".)
    - *62[.]210[.]130[.]250*

## **Task 8: Encrypted Protocol Analysis: Decrypting HTTPS**
Use the "Desktop/exercise-pcaps/https/Exercise.pcap" file.
1. What is the frame number of the "Client Hello" message sent to "accounts.google.com"?
    - `((http.request or tls.handshake.type==1)and !(ssdp)) && (tls.handshake.extensions_server_name == "accounts.google.com")`
    - *16*

2. Decrypt the traffic with the "KeysLogFile.txt" file. What is the number of HTTP2 packets?
    - *115*

3. Go to Frame 322. What is the authority header of the HTTP2 packet? (Enter the address in defanged format.)
    - `(http2) && (frame.number == 322)`
    - *safebrowsing[.]googleapis[.]com*

4. Investigate the decrypted packets and find the flag! What is the flag?
    - File -> Export Object -> "Http..." Select packet 1644 -> "Save"
    - *FLAG{THM-PACKETMASTER}*

## **Task 9: Bonus: Hunt Cleartext Credentials!**
Use the "Desktop/exercise-pcaps/bonus/Bonus-exercise.pcap" file.
1. What is the packet number of the credentials using "HTTP Basic Auth"?
    - Tools -> credentials
    - *237*

2. What is the packet number where "empty password" was submitted?
     - Tools -> credentials
     - *170*

## **Task 10: Bonus: Actionable Results!**
Use the "Desktop/exercise-pcaps/bonus/Bonus-exercise.pcap" file.
1. Select packet number 99. Create a rule for "IPFirewall (ipfw)". What is the rule for "denying source IPv4 address"?
    - Tools -> Firewall ACL Rules -> IPFirewall(ipfw)
    - `add deny ip from 10.121.70.151 to any in`

2. Select packet number 231. Create "IPFirewall" rules. What is the rule for "allowing destination MAC address"?
    - Select packet 231 -> Tools -> Firewall ACL Rules for -> Unselect "Deny" option 
    - `add allow MAC 00:d0:59:aa:af:80 any in`