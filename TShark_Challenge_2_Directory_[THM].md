# **TryHackMe**

## **TShark Challenge II: Directory**

### Difficulty: Easy - 60 mins

An alert has been triggered: "A user came across a poor file index, and their curiosity led to problems".

The case was assigned to you. Inspect the provided directory-curiosity.pcap located in ~/Desktop/exercise-files and retrieve the artefacts to confirm that this alert is a true positive.

Your tools: TShark, VirusTotal.
Answer the questions below

Investigate the DNS queries.
Investigate the domains by using VirusTotal.
According to VirusTotal, there is a domain marked as malicious/suspicious.

1. What is the name of the malicious/suspicious domain? Enter your answer in a defanged format.
    - `tshark -r directory-curiosity.pcap -T fields -e dns.qry.name | awk NF | uniq -c`
    - *jx2-bavuong[.]com*

2. What is the total number of HTTP requests sent to the malicious domain?
    - `tshark -r directory-curiosity.pcap -T fields -e http.request.full_uri | awk NF | uniq -c | grep "jx2-bavuong.com" | wc -l`
    - *14*

3. What is the IP address associated with the malicious domain? Enter your answer in a defanged format.
    - `tshark -r directory-curiosity.pcap -T fields -e dns.qry.name dns.a | awk NF | uniq -c | grep "jx2-bavuong.com" `
    - *141[.]164[.]41[.]174*

4. What is the server info of the suspicious domain?
    - `tshark -r directory-curiosity.pcap -T fields -e http.server | awk NF | uniq -c |`
    - *Apache/2.2.11 (Win32) DAV/2 mod_ssl/2.2.11 OpenSSL/0.9.8i PHP/5.2.9*

Follow the "first TCP stream" in "ASCII".Investigate the output carefully.

5. What is the number of listed files?
    - `tshark -r directory-curiosity.pcap -z follow,tcp,ascii,0 -q `
    - *3*

6. What is the filename of the first file? Enter your answer in a defanged format.
    - `tshark -r directory-curiosity.pcap -T fields -export-objects http, /home/ubuntu/Desktop exercise-files -q`
    - *123[.]php*

Export all HTTP traffic objects.
7. What is the name of the downloaded executable file? Enter your answer in a defanged format.
    - *vlauto[.]exe*

8. What is the SHA256 value of the malicious file?
    - `sha256sum vlauto.exe`
    - *b4851333efaf399889456f78eac0fd532e9d8791b23a86a19402c1164aed20de*

Search the SHA256 value of the file on VirtusTotal.

9. What is the "PEiD packer" value?
    - *.NET executable*

Search the SHA256 value of the file on VirtusTotal.

10. What does the "Lastline Sandbox" flag this as?
    - *Malware Trojan*