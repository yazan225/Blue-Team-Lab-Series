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
```

## 🔍 Investigation Steps

1. Detect DoS Attack (High Traffic from Single IP)

Search for high number of requests per IP:
```bash
index=web_logs
| stats count by clientip
| sort - count
```

👉 Look for:

- One IP generating unusually high traffic
- Sudden spikes in request count

2. Detect SQL Injection Attempts

Search for common SQLi patterns:

```bash
index=web_logs ("' OR" OR "'='" OR "1=1" OR "UNION" OR "SELECT")
```

More refined search:

```bash
index=web_logs
| regex _raw="(?i)(union|select|or 1=1|--|')"
```

👉 Indicators:

- ' OR '1'='1
- UNION SELECT
- SQL comments (--)

3. Detect OS Command Injection

Search for command execution patterns:

```bash
index=web_logs (";ls" OR ";cat" OR "&&" OR "|bash" OR "cmd=")
```

Regex version:

```bash
index=web_logs
| regex _raw="(;|\|\||&&).*(ls|cat|bash|sh)"
```

👉 Indicators:

- ; ls
- && whoami
- | bash
  
4. Identify Attacker IPs

Extract suspicious IPs:
```bash
index=web_logs ("' OR" OR ";ls" OR "&&")
| stats count by clientip
| sort - count
```
5. Analyze HTTP Status Codes

Check for anomalies:
```bash
index=web_logs
| stats count by status
```

👉 Look for:

- High number of 500 errors
- Unusual spikes in responses
  
# 📊 Findings (Example)
🔴 IP 192.168.1.15 → SQL Injection attempts detected
🔴 IP 192.168.1.20 → Command Injection activity
🔴 IP 192.168.1.10 → High request volume (possible DoS)

# 🛡️ Mitigation & Recommendations
- Block malicious IPs at firewall level
- Implement Web Application Firewall (WAF)
- Sanitize user inputs (prevent SQLi & command injection)
- Enable rate limiting (prevent DoS attacks)
- Monitor logs continuously in Splunk

