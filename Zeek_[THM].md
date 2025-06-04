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

1. Investigate the http.pcap file. Create the  HTTP signature shown in the task and 2. investigate the pcap. What is the source IP of the first event?

2. What is the source port of the second event?

- Investigate the conn.log.
3. What is the total number of the sent and received packets from source port 38706?

- Create the global rule shown in the task and investigate the ftp.pcap file.

4. Investigate the notice.log. What is the number of unique events?

5. What is the number of ftp-brute signature matches?

## **Task 5: Zeek Scripts | Fundamentals**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-6

1. Investigate the smallFlows.pcap file. Investigate the dhcp.log file. What is the domain value of the "vinlap01" host?

2. Investigate the bigFlows.pcap file. Investigate the dhcp.log file. What is the number of identified unique hostnames?

3. Investigate the dhcp.log file. What is the identified domain value?

## **Task 6: Zeek Scripts | Scripts and Signatures**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-7

Go to folder TASK-7/101.
1. Investigate the sample.pcap file with 103.zeek script. Investigate the terminal output. What is the number of the detected new connections?

- Go to folder TASK-7/201.
2. Investigate the ftp.pcap file with ftp-admin.sig signature and  201.zeek script. Investigate the signatures.log file. What is the number of signature hits?

3. Investigate the signatures.log file. What is the total number of "administrator" username detections?

4. Investigate the ftp.pcap file with all local scripts, and investigate the loaded_scripts.log file. What is the total number of loaded scripts?

- Go to folder TASK-7/202.
5. Investigate the ftp-brute.pcap file with "/opt/zeek/share/zeek/policy/protocols/ftp/detect-bruteforcing.zeek" script. Investigate the notice.log file. What is the total number of brute-force detections?

## **Task 7: Zeek Scripts | Frameworks**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-8

1. Investigate the case1.pcap file with intelligence-demo.zeek script. Investigate the intel.log file. Look at the second finding, where was the intel info found? 

2. Investigate the http.log file. What is the name of the downloaded .exe file?

3. Investigate the case1.pcap file with hash-demo.zeek script. Investigate the files.log file. What is the MD5 hash of the downloaded .exe file?

4. Investigate the case1.pcap file with file-extract-demo.zeek script. Investigate the "extract_files" folder. Review the contents of the text file. What is written in the file?

## **Task 8: Zeek Scripts | Packages**
Each exercise has a folder. Ensure you are in the right directory to find the pcap file and accompanying files. Desktop/Exercise-Files/TASK-9

1. Investigate the http.pcap file with the zeek-sniffpass module. Investigate the notice.log file. Which username has more module hits?

2. Investigate the case2.pcap file with geoip-conn module. Investigate the conn.log file. What is the name of the identified City?

3. Which IP address is associated with the identified City?

4. Investigate the case2.pcap file with sumstats-counttable.zeek script. How many types of status codes are there in the given traffic capture?