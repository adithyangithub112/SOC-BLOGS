# SOC176 - RDP Brute Force Detected

## **Introduction**

1. A brute force attack is **a cybersecurity method where attackers use automated software to systematically guess login credentials (usernames and passwords) or encryption keys by trying all possible combinations**. Relying on trial-and-error, this approach aims to find the correct key through sheer computational speed rather than exploiting software vulnerabilities.
2. Port 3389 is the default, well-known port used for Remote Desktop Protocol(RDP), allowing users to remotely access and control Windows computers. It facilitates GUI access to servers or workstations over TCP/UDP, enabling remote work and administrative tasks.

## Alert Details

| Field | Value |
| --- | --- |
| **Event ID** | 234 |
| **Event Time** | Mar 07, 2024, 11:44 AM |
| **Detection Rule** | SOC176 – RDP Brute Force Detected |
| **Alert Level** | Security Analyst |
| **Source IP Address** | 218.92.0.56 |
| **Destination IP Address** | 172.16.17.148 |
| **Destination Hostname** | Matthew |
| **Protocol** | RDP |
| **Firewall Action** | Allowed |
| **Alert Trigger Reason** | Login failure attempts from a single source IP using multiple non-existing accounts |

<img width="1556" height="435" alt="1" src="https://github.com/user-attachments/assets/d8800911-b985-472c-9b4d-ddb7d54f95c5" />


## Log analysis

In the log analysis I found that there are multiple failure login attempts  originating from the source IP address **218.92.0.56**  targeting the RDP service on the destination host **Matthew (172.16.17.148)**. 

The authentication log shows that the attacker attempted to login using several different usernames and password, which resulted in repeated login failures. This clearly show the possibility of brute force attack.

Also these failure login attempts happened in a very short period of time. Here there is about 30 login attempts occurred  at 11:44 AM on Mar 07. This points out that the attacker used automation tools or scripts to perform the brute force attack.

Based on the log analysis, the activity appears to be an unauthorized  external attempt to access the RDP service, which triggered the security rule **OC176 – RDP Brute Force Detected**. Also after several failed attempts the attacker was able to login with the username **Matthew.** successful login was observed in the captures logs. It means that the attack was successful.

<img width="1308" height="487" alt="2" src="https://github.com/user-attachments/assets/dbad2a19-ed68-4849-84bd-2dd0655fe848" />

## Threat Intelligence Analysis

The source IP address **218.92.0.56** was investigated using virus total to determine its reputation and potential malicious activity and found out that multiple security vendors marks the IP as malicious.
The IP address **218.92.0.56** belongs to the network **218.92.0.0/16** and is associated with **Autonomous System (AS4134)** operated by **Chinanet**, a large internet service provider. Threat intelligence data suggests that this IP has previously been reported for suspicious or malicious activity.

<img width="1573" height="800" alt="3" src="https://github.com/user-attachments/assets/14db2bcd-926e-4cee-b905-955431799312" />

Let’s Defend’s threat intel field also marks the IP malicious.

<img width="1556" height="832" alt="4" src="https://github.com/user-attachments/assets/68ac88d8-501c-4cfd-9e67-07591cabab81" />

## Process Activity Analysis

After reviewing the provided process logs, no suspicious activity is observed. The processes shown are part of the normal Windows operating system boot and logon sequence.

## Conclusion

The investigation confirmed that the alert SOC176 – RDP Brute Force Detected is a true positive. The external IP address 218.92.0.56 performed multiple failed login attempts against the RDP service on host 172.16.17.148. After several failed attempts (Event ID 4625), a successful login (Event ID 4624, Logon Type 10) was recorded for the user Matthew from the same IP address.

Threat intelligence sources also identify the attacker IP as malicious, supporting that this activity was likely performed by a threat actor. This indicates that the RDP brute force attack was successful and resulted in unauthorized remote access to the system.
