# 🛡️ Security Case Examples

Welcome to my security research repository. This project serves as a curated collection of real-world vulnerability case studies identified during security assessments. Each case includes a technical summary, proof of concept (PoC), and remediation steps.

---

## 📂 Vulnerability Case Studies


| Case Study | Vulnerability Type | Severity | Link |
| :--- | :--- | :--- | :--- |
| **SOAP DataService LFI** | Arbitrary File Read | 🔴 Critical | [View Case](./cases/unauthenticated-lfi-soap/) |
| **Eaton IPM RCE** | Remote Code Execution | 🔴 Critical | [View Case](./cases/rce-eaton-ipm/) |
| **AJ-Report RCE** | Remote Code Execution | 🔴 Critical | [View Case](./cases/rce-aj-report/) |
| **Weaver e-cology RCE** | Remote Code Execution | 🔴 Critical | [View Case](./cases/rce-weaver-ecology/) |
| **Host Header SSRF** | Server-Side Request Forgery | 🟠 High | [View Case](./cases/ssrf-host-header/) |
| Full SSRF Jira | Server-Side Request Forgery | 🔴 Critical | [View Case](./cases/ssrf-jira-icon) |

---

## 🎯 Research Focus
My research primarily focuses on identifying critical flaws in enterprise software, data visualization platforms, and power management systems.

*   **Remote Exploitation:** Identifying unauthenticated entry points in web applications.
*   **Infrastructure Security:** Assessing the security posture of critical management interfaces.
*   **Data Integrity:** Analyzing input sanitization and output encoding mechanisms.

---
*Disclaimer: All research is conducted for educational purposes and follows responsible disclosure principles. Unauthorized access is strictly prohibited.*
