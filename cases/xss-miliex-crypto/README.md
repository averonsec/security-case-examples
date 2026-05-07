# 🛡️ Reflected XSS + Session Hijacking — miliex.online

> [!CAUTION]
> **Security Research Discovery:** I identified a Reflected XSS vulnerability on the official platform of a **currency exchange service (miliex.online)**. The flaw allows for **Session Hijacking** via exposed `PHPSESSID` cookie accessible to JavaScript.

---

## 🔍 Target Overview

- **Organization:** Confidential (Currency Exchange Service)
- **Asset:** `miliex.online` (Exchange Platform - listed on BestChange trusted exchangers directory)
- **Severity:** 🔴 **HIGH (8.1 / 10.0)**
- **Vulnerability Types:** Reflected XSS + Session Hijacking (Missing HttpOnly flag)

---

## 🔥 Potential Business Impact

The identified vulnerability poses a significant risk to the platform and its users:

- **Session Hijacking:** An attacker can steal `PHPSESSID` of any authenticated user, gaining full access to their account.
- **Cookie Exposure:** The `HttpOnly` flag is absent on session cookies — `PHPSESSID` is fully accessible via JavaScript.
- **Financial Risk:** High risk of unauthorized access to user accounts containing funds and transaction history.

---

## ⚙️ Technical Details & Proof of Concept (PoC)

### 1. Reflected XSS via 404 Template

The URL path is reflected in the 404 error page template **without sanitization**, allowing script injection.

#### Payload:
https://miliex.online/wp-content/themes/miliex/"><img src/onerror=alert(document.cookie)>

#### Proof — Cookie Extraction via alert():

<img width="1538" height="640" alt="XSS" src="https://github.com/user-attachments/assets/05c38191-99a8-4cec-b4e8-9cc2c635d45f" />


The browser dialog confirms extraction of:
- `ref_id=8...` *(value redacted)*
- `PHPSESSID=dbs...` *(value redacted)*

---

## 🛠️ Remediation

1. Sanitize the URL path in the 404 template via `esc_html()` / `htmlspecialchars()`
2. Set `HttpOnly` and `Secure` flags on session cookies
3. Implement a `Content-Security-Policy` header
