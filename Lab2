# 🧪 Lab 2: Detecting Attacks Using Splunk (DoS, SQL Injection, Command Injection)

---

## 🧠 Objective
The objective of this lab is to use Splunk to detect common web-based attacks by analyzing logs and writing search queries.

---

## 📖 Scenario
You are a SOC analyst monitoring web server logs through Splunk.

Recent alerts suggest suspicious traffic patterns that may indicate:
- Denial of Service (DoS) attack
- SQL Injection (SQLi)
- OS Command Injection

Your task is to investigate and identify malicious activity using Splunk.

---

## 🎯 Goals
- Analyze logs using Splunk
- Detect DoS attack patterns
- Identify SQL Injection attempts
- Detect OS command injection attempts
- Extract attacker IP addresses

---

## 🛠️ Lab Environment

- **Tool:** Splunk
- **Data Source:** Web server logs (Apache / Nginx)
- **Log Type:** Access logs
- **Index:** `web_logs` (or your configured index)

---

## 📥 Sample Log Format

``` id="vndq1n"
192.168.1.10 - - [10/Oct/2025:13:55:36] "GET /index.php?id=1 HTTP/1.1" 200 2326
192.168.1.15 - - [10/Oct/2025:13:55:40] "GET /index.php?id=1' OR '1'='1 HTTP/1.1" 200 2326
192.168.1.20 - - [10/Oct/2025:13:55:45] "GET /index.php?cmd=ls;cat /etc/passwd HTTP/1.1" 200 2326


