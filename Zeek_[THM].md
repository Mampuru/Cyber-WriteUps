# **TryHackMe**

## **Zeek**

### Difficulty: Medium - 120 mins

## **Task 2: Network Security Monitoring and Zeek**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-2

1. What is the installed Zeek instance version number?
    - `zeek -v`
    - *4.2.1*

2. What is the version of the ZeekControl module?
    - `zeek -v`
    - *2.4.0*

3. Investigate the "sample.pcap" file. What is the number of generated alert files?
    - `zeek -C -r Desktop/Exercise-Files/TASK-2/sample.pcap` then count the number of files generated 
    - *8*

## **Task 3: Zeek Logs**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-3

- `zeek -C -r sample.pcap` generate log files.
- `ls -l` verify that all files are generated.

1. Investigate the sample.pcap file. Investigate the dhcp.log file. What is the available hostname?
    - `cat dhcp.log`
    - `cat dhcp.log | zeek-cut host_name`
    - *Microknoppix*

2. Investigate the dns.log file. What is the number of unique DNS queries?
    - `cat dns.log`
    - `cat dns.log  | zeek-cut query`
    - *2*

3. Investigate the conn.log file. What is the longest connection duration?
    - `cat conn.log`
    - *332.319364*

## **Task 4: CLI Kung-Fu Recall: Processing Zeek Logs**
    - *No answer needed*

## **Task 5: Zeek Signatures**
While Zeek was known as Bro, it supported Snort rules with a script called snort2bro, which converted Snort rules to Bro signatures. However, after the rebranding, workflows between the two platforms have changed. The official Zeek document mentions that the script is no longer supported and is not a part of the Zeek distribution.
Answer the questions below

Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-5

- `nano http-password.sig`
- `signature http-password {ip-proto == tcp dst-port == 80 payload /.*password.*/ event "Cleartext Password Found!"}`

1. Investigate the http.pcap file. Create the  HTTP signature shown in the task and 2. investigate the pcap. What is the source IP of the first event?
    - `zeek -C -r http.pcap -s http.password.sig`
    - `cat signatures.log | zeek-cut src_addr | sort -r`
    - *10.10.57.178*

2. What is the source port of the second event?
    - `cat signatures.log | zeek-cut src_port | sort`
    - *38712*

- Investigate the conn.log.
3. What is the total number of the sent and received packets from source port 38706?
    - `cat conn.log`
    - `cat conn.log |zeek-cut id.orig_p orig_pkts resp_pkts | grep 38706`
    - *20*

- Create the global rule shown in the task and investigate the ftp.pcap file.
    - `nano ftp-bruteforce.sig`

    - `signature ftp-username {ip-proto == tcp ftp /.*USER.*/ event "FTP Username Input Found!"}`
    - `signature ftp-brute {ip-proto == tcp payload /.*530.*Event.*incorrect.*/ event "FTP Brute-force Attempt!"}`

4. Investigate the notice.log. What is the number of unique events?
    - `zeek -C -r ftp.pcap -s ftp-brutegorce.sig` 
    - *1413*

5. What is the number of ftp-brute signature matches?
    - `cat signatures.log | head -n 20`
    - `cat signatures.log | zeek-cut sig_id | grep "ftp-brute" | wc -l`
    - *1410*

## **Task 6: Zeek Scripts | Fundamentals**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-6

1. Investigate the smallFlows.pcap file. Investigate the dhcp.log file. What is the domain value of the "vinlap01" host?
    - `zeek -C -r smallFlows.pcap`
    - `cat dhcp.log`
    - `cat dhcp.log | zeek-cut domain`
    - *astaro_vineyard*

2. Investigate the bigFlows.pcap file. Investigate the dhcp.log file. What is the number of identified unique hostnames?
    - `zeek -C -r bigFlows.pcap`
    - `cat dhcp.log | zeek-cut host_name | sort | uniq`
    - `cat dhcp.log | zeek-cut host_name | sort | uniq | wc -l`
    - *17*

3. Investigate the dhcp.log file. What is the identified domain value?
    - `cat dhcp.log | zeek-cut domain | sort | uniq`
    - *jaalam.net*

4. Investigate the dns.log file. What is the number of unique queries?
    - `cat dns.log| head`
    - *1109*

## **Task 7: Zeek Scripts | Scripts and Signatures**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-7

Go to folder TASK-7/101.
1. Investigate the sample.pcap file with 103.zeek script. Investigate the terminal output. What is the number of the detected new connections?
    - `zeek -C -r sample.pcap 103.zeek`
    - `cat conn.log | zeek-cut uid | sort | uniq | wc -l`
    - *87*

- Go to folder TASK-7/201.
2. Investigate the ftp.pcap file with ftp-admin.sig signature and  201.zeek script. Investigate the signatures.log file. What is the number of signature hits?
    - `zeek -C -r ftp.pcap 201.zeek -s ftp-admin.sig | wc -l`
    - *1401*

3. Investigate the signatures.log file. What is the total number of "administrator" username detections?
    - `cat signatures.log | zeek-cut sub_msg | grep "USER administrator" | wc -l`
    - *731*

4. Investigate the ftp.pcap file with all local scripts, and investigate the loaded_scripts.log file. What is the total number of loaded scripts?
    - `zeek -C -r ftp.pcap 201.zeek local`
    - `cat loaded_scripts.log | zeek-cut name | wc -l`
    - *498*

- Go to folder TASK-7/202.
5. Investigate the ftp-brute.pcap file with "/opt/zeek/share/zeek/policy/protocols/ftp/detect-bruteforcing.zeek" script. Investigate the notice.log file. What is the total number of brute-force detections?
    - `zeek -C -r ftp-brute.pcap /opt/zeek/share/zeek/policy/protocols/ftp/detect-bruteforcing.zeek`
    - `cat notice.log  |zeek-cut note | grep "FTP::Bruteforcing" | wc -l`
    - *2*

## **Task 8: Zeek Scripts | Frameworks**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-8

1. Investigate the case1.pcap file with intelligence-demo.zeek script. Investigate the intel.log file. Look at the second finding, where was the intel info found? 
    - `zeek -C -r case1.pcap intelligence-demo.zeek`
    - `cat intel.log | head`
    - `cat intel.log | zeek-cut seen.where`
    - *IN_HOST_HEADER*

2. Investigate the http.log file. What is the name of the downloaded .exe file?
    - `cat intel.log | head`
    - `cat http.log | zeek-cut uri | grep '\.exe$'`
    - *knr.exe*

3. Investigate the case1.pcap file with hash-demo.zeek script. Investigate the files.log file. What is the MD5 hash of the downloaded .exe file?
    - `zeek -C -r case1.pcap hash-demo.zeek`
    - `cat files.log | head`
    - `cat files.log | zeek-cut mime_type md5`
    - `cat files.log | zeek-cut fuid conn_uids tx_hosts rx_hosts mime_type extracted | nl`
    - `grep -rin CVsnuagu2ZhLnXy91 * | column -t | nl | less -S`
    - *cc28e40b46237ab6d5282199ef78c464*

4. Investigate the case1.pcap file with file-extract-demo.zeek script. Investigate the "extract_files" folder. Review the contents of the text file. What is written in the file?
    - `zeek -C -r case1.pcap  file-extract-demo.zeek`
    - `file * | nl`
    - `cat extract-1561667874.743959-HTTP-Fpgan59p6uvNzLFja`
    - *Microsoft NCSI*

## **Task 9: Zeek Scripts | Packages**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-9

1. Investigate the http.pcap file with the zeek-sniffpass module. Investigate the notice.log file. Which username has more module hits?
    - `zeek -C -r http.pcap zeek-sniffpass`
    - `cat notice.log | head`
    - `cat notice.log | zeek-cut msg | uniq -c `
    - *BroZeek*

2. Investigate the case2.pcap file with geoip-conn module. Investigate the conn.log file. What is the name of the identified City?
    - `zeek -C -r case2.pcap  geoip-conn`
    - `cat conn.log |head`
    - `cat conn.log |zeek-cut id.resp_h geo.resp.city | grep -v -e "-" | uniq -c`
    - *Chicago*

3. Which IP address is associated with the identified City?
    - *23.77.86.54*

4. Investigate the case2.pcap file with sumstats-counttable.zeek script. How many types of status codes are there in the given traffic capture?
    - `zeek -C -r case2.pcap  sumstats-counttable.zeek`
    - `zeek -C -r case2.pcap sumstats-counttable.zeek | awk '{print $3}' | grep -v -e '^$' | sort | uniq | wc -l`
    - *4*