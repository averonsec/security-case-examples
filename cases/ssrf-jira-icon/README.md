# 🔥 Jira OAuth SSRF → Full Internal Infrastructure Disclosure

> [!IMPORTANT]
> During a sanctioned security audit of the legacy infrastructure (Atlassian Jira v5.0.1), I identified a Critical Server-Side Request Forgery (SSRF) vulnerability. 
> By exploiting an unvalidated parameter in the OAuth IconUriServlet, I achieved full proxy-like access to internal resources and the local network.

## 🎯 Vulnerability Type
Full In-band SSRF (Server-Side Request Forgery) in `Atlassian Icon Picker` plugin.

## 🚀 Impact
This vulnerability allows a remote unauthenticated attacker to bypass network perimeters and:
- **Internal Network Scanning:** Map and interact with internal services (SVN, Tomcat, Databases) hidden from the public internet.
- **Data Exfiltration:** Retrieve sensitive response bodies from internal microservices (Full Reflection).
- **Cloud Metadata Exposure:** Potential theft of IAM roles/security credentials (AWS/GCP/Azure).
- **Lateral Movement:** Pivot attack to legacy internal systems (e.g., CollabNet SVN Edge on port 3343).

This significantly increases the attack surface, leading to a potential full compromise of the internal контур.

## 🧪 Proof of Concept (OOB & In-band)
The exploit chain starts with a specially crafted request to the `IconUriServlet` endpoint. 

### 1. Out-of-Band (OOB) Confirmation
By injecting a Burp Collaborator URL into the `consumerUri` parameter, I triggered an immediate back-connection from the target server's IP.

```http
GET /plugins/servlet/oauth/users/icon-uri?consumerUri=http://YOUR_COLLABORATOR_ID.oast.fun HTTP/1.1
Host: <TARGET_IP>:<PORT>
Connection: close
```
<img width="1891" height="279" alt="image" src="https://github.com/user-attachments/assets/f51b8e35-e013-4b2b-95f6-c83f6fcff154" />

### 2. In-band Reflection (Full Response Body)
The vulnerability is not "blind"—the server reflects the full response body of the requested resource. By using the server as an open proxy to fetch `google.com`, I confirmed that an attacker can exfiltrate sensitive data from any internal HTTP service.

```http
GET /plugins/servlet/oauth/users/icon-uri?consumerUri=http://google.com HTTP/1.1
Host: <TARGET_IP>:<PORT>
Connection: close
```
<img width="1873" height="680" alt="image" src="https://github.com/user-attachments/assets/b45ba25a-5be6-4718-9f72-ca1a2b4e2ab0" />


**Result:** The Jira server fetched the external content and displayed it in the response, verifying the Full SSRF vector.

## 🛠 Internal Reconnaissance & Pivot
By scanning the loopback interface (`127.0.0.1`), several high-value internal assets were identified:
- **SVN Edge Admin Console (Port 3343):** Exposed internal source control management.
- **Legacy Apache Tomcat (Port 28080):** Outdated server environment (v6.0.32) vulnerable to additional legacy exploits.
- **Internal API Endpoints:** Exposure of sensitive system metrics and server status.

## 🕳 Root Cause Analysis
The issue stems from a lack of input validation and protocol filtering in the `IconUriServlet`. The application does not enforce a whitelist for the `consumerUri` parameter, allowing it to reach internal network segments (SSRF) and proxy full responses (Reflection).

## 🚨 Severity: CRITICAL (9.8 / 10.0)
- **Vector:** Network / Unauthenticated
- **Complexity:** Low
- **Impact:** Full Internal Infrastructure Disclosure / Lateral Movement / Data Exfiltration.

---
*Disclaimer: This research was conducted under an authorized security audit. All sensitive information has been redacted.*

