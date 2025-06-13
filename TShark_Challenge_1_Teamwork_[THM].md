# **TryHackMe**

## **TShark Challenge I: Teamwork**

### Difficulty: Easy - 60 mins

An alert has been triggered: "The threat research team discovered a suspicious domain that could be a potential threat to the organisation."

The case was assigned to you. Inspect the provided teamwork.pcap located in ~/Desktop/exercise-files and create artefacts for detection tooling.

Your tools: TShark, VirusTotal.
Answer the questions below

Investigate the contacted domains.
Investigate the domains by using VirusTotal.
According to VirusTotal, there is a domain marked as malicious/suspicious.

1. What is the full URL of the malicious/suspicious domain address? Enter your answer in defanged format.
    - `tshark -r teamwork.pcap -T fields -e http.host | sort -r | uniq`
    - *hxxp[://]www[.]paypal[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com/*

2. When was the URL of the malicious/suspicious domain address first submitted to VirusTotal?
    - Copy link into VriusTotal -> deatails
    - *2017-04-17 22:52:53 UTC*

3. Which known service was the domain trying to impersonate?
    - *PayPal*

4. What is the IP address of the malicious domain? Enter your answer in defanged format.
    - `tshark -r teamwork.pcap -T fields -e dns.qry.name -e dns.a | sort -u`
    - *184[.]154[.]127[.]226*

5. What is the email address that was used? Enter your answer in defanged format. (format: aaa[at]bbb[.]ccc)
    - `tshark -r teamwork.pcap -V  | grep -Eo '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,3}'`
    - *johnny5alive[at]gmail[.]com*