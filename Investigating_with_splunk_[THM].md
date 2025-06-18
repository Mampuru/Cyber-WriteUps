# **TryHackMe**

## **Investigating With Splunk**

### Difficulty: Medium - 30 mins


SOC Analyst Johny has observed some anomalous behaviours in the logs of a few windows machines. It looks like the adversary has access to some of these machines and successfully created some backdoor. His manager has asked him to pull those logs from suspected hosts and ingest them into Splunk for quick investigation. Our task as SOC Analyst is to examine the logs and identify the anomalies.

To learn more about Splunk and how to investigate the logs, look at the rooms splunk101 and splunk201.

Answer the questions below
1. How many events were collected and Ingested in the index main?
    - `index="main"`
    - *12256*

2. On one of the infected hosts, the adversary was successful in creating a backdoor user. What is the new username?
    - `index="main" EventID="4720”` 
    - 4720 is logged when a user account is created.
    - Click on *SamAccountName* and search for the *New Accounts* 
    - *A1berto*

3. On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?
    - `index="main" EventID=13 A1berto`
    - 13 This would query to Sysmon events that logged modifications of a registry value.
    - Click on the “TargetObject” field to display the value of the object that was modified.
    - *HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto*

4. Examine the logs and identify the user that the adversary was trying to impersonate.
    -`index="main"`
    - The names of users are found in the “User” field. The newly created user “A1berto” is not the same as “Alberto”; therefore, “Alberto” is being impersonated
    - *Alberto*

5. What is the command used to add a backdoor user from a remote computer?
    - `index="main" EventID=1 OR EventID=4688 A1berto`
    - Filter events with ID of 4688 of Sysmon event ID of 1.
    - Select the “CommandLine” field. Of the values, the first set of commands is a command a remote user would used because the “wmic” is a command-line tool which can be leveraged for remote execution of commands.
    - *C:\windows\System32\Wbem\WMIC.exe” /node:WORKSTATION6 process call create “net user /add A1berto paw0rd1*

6. How many times was the login attempt from the backdoor user observed during the investigation?
    - `index="main" EventID="4625" OR EventID="4624" A1berto`
    - The query will filter events where successful and failed account logon attempts were made by the backdoor user.
    - 4625 - An account failed to log on | 4624 - An account was successfully logged on 
    - *0*

7. What is the name of the infected host on which suspicious Powershell commands were executed?
    - `index="main" EventID="4104" OR EventID="4103"`
    - 4104 - Powershall Script Block Logging | 4103 - module Logging
    - *James.browne*

8. PowerShell logging is enabled on this device. How many events were logged for the malicious PowerShell execution?
    - `index="main" EventID="4104" OR EventID="4103"`
    - *79*

9. An encoded Powershell script from the infected host initiated a web request. What is the full URL?
    - `index="main" EventID="4104" OR EventID="4103"` 
    - `|rex field=ContextInfo "Host Application = (?<Command>[^\r\n]+)"`
    - `| table Command`
    - `| dedup Command`
    - The modified query will extract the value of “Host Application” from the field “ContextInfo”, present it on a table without duplicate commands.
    - *hxxp[://]10[.]10[.]10[.]5/news[.]php*