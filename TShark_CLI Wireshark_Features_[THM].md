# **TryHackMe**

## **TShark: CLI Wireshark Features**

### Difficulty: Medium - 120 mins

## **Task 2: Command-Line Wireshark Features I | Statistics I**
Use the "write-demo.pcap" to answer the questions.

1. What is the byte value of the TCP protocol?
    - `tshark -r write-demo.pcap -z io,phs -q`
    - *62*

2. In which packet lengths row is our packet listed?
    - `tshark -r write-demo.pcap -z plen,tree -q`
    - *40-79*

3. What is the summary of the expert info?
    - `tshark -r write-demo.pcap -z expert -q`
    - *Connection establish request (SYN): server port 80* 

Use the "demo.pcapng" to answer the question.

4. List the communications. What is the IP address that exists in all IPv4 conversations Enter your answer in defanged format.
    - `tshark -r demo.pcapng -z conv,ip -q`
    - *145[.]254[.]160[.]237*

## **Task 3: Command-Line Wireshark Features II | Statistics II**
Use the "demo.pcapng" to answer the questions.
1. Which IP address has 7 appearances? Enter your answer in defanged format.
    - `tshark -r demo.pcapng -z ip_host,tree -q`
    - *216[.]239[.]59[.]99*

2. What is the "destination address percentage" of the previous IP address?
    - `tshark -r demo.pcapng -z ip_secdst,tree -q`
    - *6.98%*

3. Which IP address constitutes "2.33% of the destination addresses"? Enter your answer in defanged format.
    - `tshark -r demo.pcapng -z dest, tree -q`
    - *145[.]253[.]2[.]203*

4. What is the average "Qname Len" value?
    - `tshark -r demo.pcapng -z dns, tree -q`
    - *29.00*

## ** Task 4: Command-Line Wireshark Features III | Streams, Objects and Credentials**
Use the "demo.pcapng" to answer the questions.
Follow the "UDP stream 0".

1. What is the "Node 0" value? Enter your answer in defanged format.
    - `tshark -r demo.pcapng -z follow,udp,ascii, 0 -q`
    - *145[.]254[.]160[.]237:3009*

Follow the "HTTP stream 1".
    
2. What is the "Referer" value? Enter your answer in defanged format.
    - `tshark -r demo.pcapng -z follow,http,ascii, 1 -q`
    - *hxxp[://]www[.]ethereal[.]com/download[.]html*

Use the "credentials.pcap" to answer the question.

3. What is the total number of detected credentials?
    - `tshark -r credentials.pcap -z credentials -q | wc -l`
    - `tshark -r credentials.pcap -z credentials -q | ul`
    - *79 - 4 = 75*

## **Task 5: Advanced Filtering Options | Contains, Matches and Fields**
Use the "demo.pcapng" to answer questions.

1. What is the HTTP packet number that contains the keyword "CAFE"?
    - `tshark -r demo.pcapng -Y 'http contains "CAFE"'`
    - *27*

Filter the packets with "GET" and "POST" requests and extract the packet frame time.
    
2. What is the first time value found?
    - `tshark -r demo.pcapng -Y 'http.request.method == "GET" || http.request.method == "POST"' -T fields -e frame.time`
    - *May 13, 2004 10:17:08.222534000 UTC*

## **Task 6: Use Cases | Extract Information**
Use the "hostnames.pcapng" to answer the questions.

1. What is the total number of unique hostnames?
    - `tshark -r hostnames.pcapng -T fields -e dhcp.option.hostname | awk NF | sort -r | uniq -c |sort -r | wc -l`
    - *30*

2. What is the total appearance count of the "prus-pc" hostname?
    - `tshark -r hostnames.pcapng -T fields -e dhcp.option.hostname | grep -c "prus-pc"`
    - *12*

Use the "dns-queries.pcap" to answer the question.

3. What is the total number of queries of the most common DNS query?
    - `tshark -r dns-queries.pcap -T fields -e dhcp.option.hostname |sort|uniq -c |sort -ur|head -n 1`
     - *472*

Use the "user-agents.pcap" to answer questions.

4. What is the total number of the detected "Wfuzz user agents"?
    - `tshark -r user-agent.pcap -Y 'http.user_agent contains "Wfuzz"' | wc -l`
    - *12*

5. What is the "HTTP hostname" of the nmap scans? Enter your answer in defanged format.
    - `tshark -r user-agent.pcap -T fields -e http.host`
    - *172[.]16[.]172[.]129*