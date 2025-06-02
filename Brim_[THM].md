# **TryHackMe**

## **Brim**

### Difficulty: Medium - 120 mins

## **Task 3:The Basics**
1. Process the "sample.pcap" file and look at the details of the first DNS log that appear on the dashboard. What is the "qclass_name"?
- `-path=='dns' | cut qclass_name | uniq`

2. Look at the details of the first NTP log that appear on the dashboard. What is the "duration" value?
- `-path=='ntp' | sort -r`    

3. Look at the details of the STATS packet log that is visible on the dashboard. What is the "reassem_tcp_size"?
- `-path=="status" | cut reassem_tcp_size`

## **Task 4: Default Queries**
1. Investigate the files. What is the name of the detected GIF file?
- `-path=="files" | cut filename | uniq`

2. Investigate the conn logfile. What is the number of the identified city names?
- `-path=="conn" | cut geo.resp.city | sort | uniq -c`

3. Investigate the Suricata alerts. What is the Signature id of the alert category "Potential Corporate Privacy Violation"?
- `event_type=="alert" | alert.signature, alert.category, alert.signature_id | uniq`

## **Task 6: Exercise: Threat Hunting with Brim | Malware C2 Detection**
1. What is the name of the file downloaded from the CobaltStrike C2 connection?
- `-path=="http" | cut host, uri | uniq -c | 104.168.44.45`

2. What is the number of CobaltStrike connections using port 443?
- `-path=="conn" | id.resp_h==104.168.44.45 id.resp_p==443 | count() by id.resp_p`

3. There is an additional C2 channel in used the given case. What is the name of the secondary C2 channel?
- *Select Sucicata alerts by category*


## **Task 7: Exercise: Threat Hunting with Brim | Crypto Mining**
1. How many connections used port 19999?
- `-path=="conn" id.resp_p==19999 | count() by id.resp_p`

2. What is the name of the service used by port 6666?
- `-path=="conn" idresp_p==6666 | cut service | uniq`

3. What is the amount of transferred total bytes to "101.201.172.235:8888"?
- `-path=="conn" | put total_bytes:= orig_byte + resp_bytes | 101.201.172.235 | 8888| cut uid, id, orig_bytes, resp_bytes, total_bytes`

4. What is the detected MITRE tactic id?
- `event_type=="alert"`