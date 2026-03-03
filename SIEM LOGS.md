


# SIEM Introduction for SOC Analysts

Security Information and Event Management, also known as SIEM, is a security solution that collects and interprets data within an organization and detects potential threats. Thanks to SIEM, security threats can be monitored in real time.

For anyone starting in a SOC role, understanding how SIEM works is important. You do not need to go very deep into technical details at the beginning, but you should understand what is happening behind the scenes. In this note, we will go through how SIEM works, how it collects logs, how logs are stored, and how alerts are created.

<img width="358" height="546" alt="image" src="https://github.com/user-attachments/assets/a37b8508-fa4a-428e-b627-ea72cdce8830" />
## SIEM and the SOC Analyst

Potential threats detected with SIEM are reviewed by SOC analysts. If you have used LetsDefend, you can think of the alerts on the monitoring page as alerts created by a SIEM system. The SIEM collects data, analyzes it, and generates alerts. The SOC analyst investigates those alerts.

## Log Collection

The most important part of a SIEM architecture is log collection. Without logs, SIEM would be useless. The system needs data to detect threats.

In computing, a log file records events that occur in an operating system or other software. Logging is simply the act of keeping those records. A basic log contains time, source system, and a message. For example, when you check the /var/log/auth.log file on an Ubuntu server, you can see source, time, and message information.

The goal is to transfer logs from various systems such as hosts, firewalls, servers, and proxies to the SIEM. By collecting everything in one place, we can process all data and detect threats centrally.

Logs are generally collected in two ways, with log agents or without agents.
<img width="500" height="289" alt="image" src="https://github.com/user-attachments/assets/cada1ae9-ef13-4b34-b366-14bbe955ecc6" />

## Log Agents

In this method, a log agent software is installed on the system. Agents often include features such as parsing, log rotation, buffering, encryption, and log integrity. This means the agent can take action on logs before forwarding them to the SIEM.

For example, if a log contains “username: LetsDefend; account: Administrator”, the agent can divide it into separate parts before sending it.

The advantage of this method is that it is tested and developed by the vendor and has many useful features. The disadvantage is that additional features increase resource consumption. More CPU and RAM may be required, which increases cost.

## Syslog

Syslog is a very popular network protocol for log transfer. It works with UDP or TCP and can optionally be encrypted with TLS. Many devices such as switches, routers, IDS, firewalls, Linux, Mac, and Windows systems can support syslog.

The syslog format includes timestamp, source device, facility, severity, message number, and message text.

One important detail is that the maximum packet size for syslog over UDP is 1024 bytes, and for TCP it is 4096 bytes.

## Third Party Agents

Most SIEM products provide their own agent software. For example, Splunk uses Universal Forwarder and ArcSight uses ArcSight Connectors. There are also popular open source agents such as Beats and NXLog.

These agents are easy to integrate and provide parsing capabilities beyond basic syslog.

## Agentless Log Collection

Sometimes organizations prefer agentless methods because there is no installation or update cost. Logs are collected by connecting to the target system using SSH or WMI. However, this method requires usernames and passwords, which creates a risk if credentials are stolen. It is easier to manage but has limited capabilities.

## Log Aggregation and Parsing

Logs first go to a log aggregator. Here, logs can be processed before being sent to storage. This process can include parsing, filtering, and enrichment.

For example, if you only want status codes from web server logs, you can filter and send only that information.

### EPS

EPS stands for events per second. It is calculated as events divided by time in seconds. If a system receives 1,000 logs in five seconds, the EPS is 200. As EPS increases, aggregator and storage capacity must also increase.

To handle high log volume, multiple aggregators can be added so that incoming logs are distributed and do not overload a single system.

## Log Modification and Enrichment

Sometimes logs need modification. For example, if most logs use the format dd mm yyyy and one source uses mm dd yyyy, it should be converted for consistency. Time zones may also need adjustment.

Enrichment increases the efficiency of logs. One example is geolocation. The system can find the geographic location of an IP address and add it to the log. This saves time during investigation and helps analyze location based behavior.

Another example is DNS enrichment. The system can resolve domain names to IP addresses or perform reverse DNS lookups.

## Log Storage

After processing, logs are stored. A common mistake in SIEM design is focusing only on storage size. Storage speed is equally important. If it takes fifteen minutes to search logs, investigations become inefficient.

Unlike traditional databases that focus on adding and editing data, SIEM storage focuses on indexing data for fast search. Since logs are not meant to be edited later, write once read many technologies are more suitable.

## Alerting

After logs are collected, processed, and stored, the next step is detecting abnormal behavior and generating alerts.

Alert timing depends on search speed. If a suspicious log is created today, we want the alert immediately, not two days later. This is why proper storage and indexing are critical.

Alerts usually indicate suspicious activity and need investigation. They should be optimized and not trigger in large numbers unless necessary.

Alerts can be created by searching stored data or by analyzing logs in real time as they are received.

Examples include a new user added to global administrator or fifteen login failures in three minutes from the same IP address.

## Techniques for Better Alerting

To create quality alerts, you must understand your data. Common techniques include blacklisting, whitelisting, and long tail log analysis.

Blacklist is used to catch undesirable situations. For example, if a prohibited process name such as mimikatz.exe appears in logs, an alert can be generated. This method is easy to implement but easy to bypass if the attacker changes the file name.

Whitelist is the opposite. It defines what is normal and generates alerts for anything outside that list. This method is effective but difficult to maintain because the list must be updated regularly.

Long tail log analysis assumes that frequently occurring behaviors are normal. Rare events are treated as suspicious. For example, if successful login events constantly occur on a device, they may be normal. Less common events should be examined more closely.


note: Let's defend
