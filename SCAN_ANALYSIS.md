# Security Scan Analysis – 2025-12-02

This document summarizes the latest pipeline security scans:

- **DAST (ZAP Fullscan & Baseline)**
- **SCA (Dependency-Check)**
- **SAST (CodeQL)**

---

## DAST Results (OWASP ZAP)

- Total URLs scanned: 1 (http://localhost:5000)
- PASS: 0
- WARN-NEW: 7 (Medium/Low)
- FAIL-NEW: 0

### Warning Summary

| Code  | Issue                                 | Risk   | Affected URL          | Why It Matters                                     |
| ----- | ------------------------------------- | ------ | --------------------- | -------------------------------------------------- |
| 10038 | Missing Content Security Policy (CSP) | Medium | http://localhost:5000 | CSP prevents XSS by restricting scripts.           |
| 10106 | HTTP Only Site                        | Medium | http://localhost:5000 | No HTTPS enables MITM attacks.                     |
| 10020 | Missing Anti-clickjacking Header      | Medium | http://localhost:5000 | Allows clickjacking via hidden iframes.            |
| 10021 | X-Content-Type-Options Missing        | Low    | http://localhost:5000 | Prevents browsers from MIME-sniffing.              |
| 10063 | Permissions-Policy Header Not Set     | Low    | http://localhost:5000 | Browser features (camera/mic/etc.) not restricted. |
| 10036 | Server Leaks Version Information      | Low    | http://localhost:5000 | Helps attackers fingerprint your server.           |
| 90004 | Insufficient Site Isolation (Spectre) | Low    | http://localhost:5000 | Weakens protection vs side-channel attacks.        |

---

## SCA Results (Dependency-Check)

**Based on `sca-reports.zip`**

- High severity: 0
- Medium severity: 0
- Low severity: 0
- Total vulnerabilities: **0**
- Status: **No vulnerable dependencies found** ✔

### Vulnerable Packages

_No CVEs detected in your Python dependencies._

---

## SAST Results (CodeQL)

- Total issues: **1**
- High severity: **1**
- Medium: 0
- Low: 0

### Issues by Type

| Issue Type                     | Severity | Description                                                                                       | File               |
| ------------------------------ | -------- | ------------------------------------------------------------------------------------------------- | ------------------ |
| Flask app is run in debug mode | High     | Debug mode exposes Werkzeug debugger, which allows remote code execution if attacker accesses it. | `app.py`, line ~40 |

---

## Combined Severity Breakdown

| Severity | Count | Source                             |
| -------- | ----- | ---------------------------------- |
| Critical | 0     | –                                  |
| High     | 1     | CodeQL (Flask debug mode)          |
| Medium   | 3     | ZAP (CSP, clickjacking, HTTP-only) |
| Low      | 4     | ZAP misc headers                   |
| Info     | 0     | –                                  |
| Total    | 8     |                                    |

---

## Recommendations

### 🔴 High Priority (Fix Immediately)

1. **Disable Flask Debug Mode**

   - Replace:
     ```python
     app.run(debug=True)
     ```
     with:
     ```python
     app.run(debug=False)
     ```
   - Never use debug mode in production.

2. **Enable HTTPS**
   - Use reverse proxy or configure SSL certificates.

---

### 🟡 Medium Priority

3. **Add Security Headers**

   - `Content-Security-Policy`
   - `X-Frame-Options`
   - `X-Content-Type-Options`
   - `Permissions-Policy`

4. **Configure security-aware response settings**
   - Prevent caching of sensitive data
   - Avoid exposing server version

---

### 🟢 Low Priority

5. **Re-run scans after fixes**  
   Ensure WARN-NEW count decreases.
