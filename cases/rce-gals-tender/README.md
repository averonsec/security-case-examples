# 🛡️ Infrastructure Audit: Major Enterprise Real Estate Developer

> [!CAUTION]
> **Security Research Discovery:** I identified a combined critical vulnerability chain on the official tender platform of a **top-tier Russian developer**. The flaws allow for **Unauthenticated Remote Code Execution (RCE)** and a **Full Source Code Leak** via an exposed `.git` directory.

---

## 🔍 Target Overview
*   **Organization:** Confidential (Large Real Estate Holding.
*   **Asset:** `tenders.target-portal.ru` (Tender & Procurement Platform).
*   **Severity:** 🔴 **CRITICAL (10.0/10.0)**
*   **Vulnerability Types:** CVE-2022-27228 (RCE) & Sensitive Data Exposure (.git).

## 💥 Potential Business Impact
The identified vulnerabilities pose a significant risk to the procurement infrastructure:
*   **Arbitrary Code Execution:** An unauthenticated attacker can execute PHP logic on the server, leading to potential full system compromise.
*   **Source Code Disclosure:** An exposed `.git` directory leads to the leak of application logic and internal repository structure.
*   **Data Breach Risk:** High risk of unauthorized access to multi-billion ruble tender documentation and sensitive partner data.

---

## 🛠️ Technical Details & Proof of Concept (PoC)

### 1. Unauthenticated RCE (CVE-2022-27228)
The vulnerability is located in the **Bitrix Voting module**. It allows for the manipulation of the `CFileUploader` component to invoke internal agents without authentication.

#### Personalized Verification (Burp Suite):
To confirm control over the server's file-processing logic, I injected a custom filename: **`averonsec.txt`**. The server responded with `"executeStatus":"executed"`, confirming that the input was successfully processed.

<a href="https://github.com/user-attachments/assets/247f0734-42a3-4d48-8e49-701508ed51b7" target="_blank">
  <img src="https://github.com/user-attachments/assets/247f0734-42a3-4d48-8e49-701508ed51b7" width="100%" alt="RCE Proof with custom filename - Click to enlarge" />
</a>

### 2. Critical Source Code Leak (.git Exposure)
The web server configuration fails to restrict access to the hidden **`.git`** directory. This allows for the complete reconstruction of the platform's source code, exposing internal repository URLs and developer history.

#### Evidence of .git/config Access:
By accessing the hidden `.git/config` file, I confirmed the exposure of the project's internal repository structure (`internal repository structure`).

<a href="https://github.com/user-attachments/assets/dbfb8d9c-3d1f-4f95-8042-f32e19a8121a" target="_blank">
  <img src="https://github.com/user-attachments/assets/dbfb8d9c-3d1f-4f95-8042-f32e19a8121a" width="100%" alt="Git Config Leak Proof - Click to enlarge" />
</a>

---

## 📊 Findings Summary


| Severity | Vulnerability | Description |
| :--- | :--- | :--- |
| 🔴 **CRITICAL** | **Remote Code Execution** | Confirmed CVE-2022-27228 bypass of authentication. |
| 🔴 **CRITICAL** | **Source Code Leak** | Public access to the `.git` directory. |
| 🟠 **HIGH** | **Insecure Configuration** | Verbose error disclosure and use of outdated software modules. |

## 🛡️ Remediation
1. **Patching:** Immediately update Bitrix CMS and all associated modules to the latest versions.
2. **Access Control:** Configure the web server (Nginx/Apache) to deny all requests to the `.git` directory and other hidden files.

---
*Responsible disclosure followed. All testing was non-destructive and performed to strengthen the security of the infrastructure.*
