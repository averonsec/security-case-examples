# 🔥 Host Header Injection → Internal Apache Resource Exposure

> [!IMPORTANT]
> During manual testing, I identified a Host header manipulation vulnerability that allowed access to internal resources hosted on 127.0.0.1.
>
> By modifying the Host header in Burp Repeater, I retrieved the /server-status endpoint, exposing sensitive Apache server information.

## 🧠 Vulnerability Type
Host Header Injection → SSRF-like internal resource access

## 🎯 Impact
- Apache server version disclosure  
- Server uptime exposure  
- Active worker process visibility  
- Internal request handling details  
- Potential internal IP leakage  

This significantly increases the attack surface and enables reconnaissance.

## 🧪 Proof of Concept
```http
GET /server-status HTTP/1.1
Host: 127.0.0.1
```
<img width="1860" height="653" alt="ssrf" src="https://github.com/user-attachments/assets/676a48e3-3849-423f-97ac-af6fdb5655c5" />

The server responded with the internal Apache Server Status page.  
*(Sensitive information redacted)*

## 🕵️ Root Cause
Improper validation of the Host header allowed internal routing to localhost.

## 📊 Severity
High

## 🛡️ Remediation
- Validate/whitelist Host headers server-side  
- Restrict /server-status access  
- Disable mod_status in production  
- Enforce strict internal routing controls  

Responsible disclosure. Educational purposes only.
