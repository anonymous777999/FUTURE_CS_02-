# 🛡️ SOC Log Analysis with Splunk (All-in-One)

## 📌 Overview
This project simulates a real-world **Security Operations Center (SOC)** log analysis using **Splunk**.  
The task was to ingest logs, extract important fields, run queries, and identify **malware infections**, **failed login attempts**, and suspicious activity.

---

## 📂 Contents in This File
1. **Sample Logs**  
2. **Step-by-step Instructions for Splunk**  
3. **Field Extraction Regex**  
4. **Splunk Queries**  
5. **Findings**  
6. **Recommendations**  
7. **Incident Report Summary**  
8. **Screenshot Placeholders**

---

## 1️⃣ Sample Logs
```txt
2025-07-03 06:13:14 | user=charlie | ip=10.0.0.5 | action=connection attempt
2025-07-03 08:20:14 | user=charlie | ip=192.168.1.101 | action=connection attempt
2025-07-03 05:04:14 | user=bob | ip=192.168.1.101 | action=login success
2025-07-03 06:01:14 | user=bob | ip=172.16.0.3 | action=file accessed
2025-07-03 05:18:14 | user=charlie | ip=172.16.0.3 | action=login success
2025-07-03 04:27:14 | user=david | ip=172.16.0.3 | action=connection attempt
2025-07-03 05:48:14 | user=bob | ip=10.0.0.5 | action=malware detected | threat=Trojan Detected
2025-07-03 08:30:14 | user=eve | ip=172.16.0.3 | action=login success
2025-07-03 08:21:14 | user=david | ip=172.16.0.3 | action=connection attempt
2025-07-03 05:45:14 | user=david | ip=172.16.0.3 | action=malware detected | threat=Trojan Detected
2025-07-03 08:00:14 | user=alice | ip=198.51.100.42 | action=login success
2025-07-03 04:19:14 | user=alice | ip=198.51.100.42 | action=malware detected | threat=Rootkit Signature
2025-07-03 05:30:14 | user=eve | ip=192.168.1.101 | action=malware detected | threat=Trojan Detected
2025-07-03 06:10:14 | user=david | ip=203.0.113.77 | action=file accessed
2025-07-03 05:42:14 | user=eve | ip=203.0.113.77 | action=malware detected | threat=Trojan Detected
2025-07-03 07:02:14 | user=alice | ip=203.0.113.77 | action=login failed
2025-07-03 04:18:14 | user=bob | ip=198.51.100.42 | action=login success
2025-07-03 09:02:14 | user=david | ip=203.0.113.77 | action=login failed
2025-07-03 09:07:14 | user=eve | ip=203.0.113.77 | action=login success
2025-07-03 04:47:14 | user=bob | ip=10.0.0.5 | action=login failed
2025-07-03 07:38:14 | user=charlie | ip=172.16.0.3 | action=connection attempt
2025-07-03 07:57:14 | user=david | ip=10.0.0.5 | action=file accessed
2025-07-03 07:44:14 | user=bob | ip=203.0.113.77 | action=connection attempt
2025-07-03 05:33:14 | user=david | ip=198.51.100.42 | action=file accessed
2025-07-03 04:19:14 | user=david | ip=10.0.0.5 | action=connection attempt
2025-07-03 04:29:14 | user=alice | ip=192.168.1.101 | action=malware detected | threat=Trojan Detected
2025-07-03 07:51:14 | user=eve | ip=10.0.0.5 | action=malware detected | threat=Rootkit Signature
2025-07-03 04:53:14 | user=david | ip=203.0.113.77 | action=login success
2025-07-03 04:23:14 | user=charlie | ip=198.51.100.42 | action=login failed
2025-07-03 05:27:14 | user=david | ip=203.0.113.77 | action=connection attempt
2025-07-03 07:46:14 | user=bob | ip=10.0.0.5 | action=login success
2025-07-03 04:41:14 | user=alice | ip=172.16.0.3 | action=malware detected | threat=Spyware Alert
2025-07-03 09:10:14 | user=bob | ip=198.51.100.42 | action=file accessed
2025-07-03 07:36:14 | user=david | ip=10.0.0.5 | action=connection attempt
2025-07-03 08:31:14 | user=eve | ip=203.0.113.77 | action=file accessed
2025-07-03 05:49:14 | user=charlie | ip=192.168.1.101 | action=connection attempt
2025-07-03 06:21:14 | user=alice | ip=203.0.113.77 | action=login success
2025-07-03 07:44:14 | user=bob | ip=192.168.1.101 | action=connection attempt
2025-07-03 04:23:14 | user=bob | ip=172.16.0.3 | action=login failed
2025-07-03 07:18:14 | user=bob | ip=203.0.113.77 | action=file accessed
2025-07-03 05:12:14 | user=alice | ip=198.51.100.42 | action=login success
2025-07-03 05:06:14 | user=bob | ip=203.0.113.77 | action=malware detected | threat=Worm Infection Attempt
2025-07-03 08:42:14 | user=charlie | ip=203.0.113.77 | action=file accessed
2025-07-03 09:10:14 | user=bob | ip=172.16.0.3 | action=malware detected | threat=Ransomware Behavior
2025-07-03 04:46:14 | user=david | ip=203.0.113.77 | action=login success
2025-07-03 08:42:14 | user=eve | ip=172.16.0.3 | action=file accessed
2025-07-03 07:22:14 | user=charlie | ip=192.168.1.101 | action=connection attempt
2025-07-03 04:53:14 | user=alice | ip=203.0.113.77 | action=file accessed
2025-07-03 07:45:14 | user=charlie | ip=172.16.0.3 | action=malware detected | threat=Trojan Detected
2025-07-03 05:44:14 | user=bob | ip=198.51.100.42 | action=file accessed
```
---------------------------------------------------------------------------------------
2️⃣ Splunk Upload Instructions
Go to Settings → Add Data → Upload File.

Select the above log file.
Set:
Source type: soc_task2
Host: soc_task2
Index: main
Click Start Searching.
---------------------------------------------------------------------------------------

3️⃣ Splunk Queries
Events by IP:
index="main" sourcetype="soc_task2"
| stats count by ip
| sort -count
---------------------------------------------------------------------------------------

Events by User:
index="main" sourcetype="soc_task2"
| stats count by user
| sort -count
---------------------------------------------------------------------------------------

Failed logon:
index="main" sourcetype="soc_task2" "FAILED" | sort - count | head 10
---------------------------------------------------------------------------------------

Successfull logon:
index="main" sourcetype="soc_task2" "success" | sort - count | head 10
---------------------------------------------------------------------------------------

Malware Detection:
index="main" sourcetype="soc_task2" action="malware detected"
| stats count by user, ip, threat
---------------------------------------------------------------------------------------

4️⃣ Findings
Malware Detected: Multiple types (Trojan, Rootkit, Spyware, Ransomware, Worm).
Repeated Failed Logins: Possible brute force attempts.
Unusual File Access: Possible insider threat or data exfiltration.
Frequent Connection Attempts: Scanning or unauthorized probing.
---------------------------------------------------------------------------------------

5️⃣ Recommendations
Isolate infected hosts immediately.
Reset passwords for all affected accounts.
Enable brute force lockout policies.
Review firewall & IDS/IPS logs for matching activity.
Monitor for recurring infection attempts.
---------------------------------------------------------------------------------------

