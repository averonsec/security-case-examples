# Security-case-examples
Sample security case studies (educational purposes)


# Reflected XSS – Case Study

## Summary

A reflected cross-site scripting vulnerability was identified in a search endpoint.
User-controlled input was rendered without proper output encoding.

The issue was responsibly reported to the vendor.
This document contains a sanitized technical summary.

## Vulnerability Type
Reflected Cross-Site Scripting (XSS)

## Affected Component
Search functionality (query parameter)

## Technical Details

The application reflects user input directly into the HTML response.
No output encoding was applied before rendering.

As a result, arbitrary JavaScript execution was possible in the user’s browser context.

## Impact

- Session token exposure (if not protected by HttpOnly)
- Account takeover risk
- Client-side content manipulation
- Phishing or redirection scenarios

## Proof of Concept

Sanitized screenshot:

<img width="1313" height="444" alt="xss cookie2" src="https://github.com/user-attachments/assets/09515401-69ca-4b71-a18d-7d9c93e931d9" />

## Remediation

- Strict input validation
- Context-aware output encoding
- Apply HttpOnly and Secure flags to session cookies
- Consider Content Security Policy (CSP)

---

Independent Security Researcher  
Averon Averenkov
