# Scanner Comparison

This document compares results from the three security scanners used in CI:

- **SAST (CodeQL)**
- **SCA (Dependency-Check)**
- **DAST (OWASP ZAP)**

---

## Only CodeQL (SAST) Found

**1 High-severity SAST issue:**

- Flask application is running in **debug mode**
- Risk: exposes Werkzeug debugger → remote code execution

**Why SAST catches this:**  
It analyzes source code behavior, not runtime behavior.  
DAST cannot detect debug mode unless an error page is exposed.

---

## Only ZAP (DAST) Found

ZAP identified **7 configuration issues** not visible in code:

- Missing Content-Security-Policy
- Missing X-Frame-Options
- Missing X-Content-Type-Options
- Missing Permissions-Policy
- Server header leaks version
- HTTP-only site (no HTTPS)
- Insufficient Spectre isolation

**Why DAST catches this:**  
It observes the application **as it runs**, inspecting headers, HTTP behavior, and runtime configuration.

---

## Only Dependency-Check (SCA) Found

- **No vulnerabilities found**
- Dependencies appear clean.

**Why SCA is unique:**  
Only SCA checks third-party library CVEs using NVD databases.

---

## Overlap Between Tools

- **SAST + DAST overlap:**  
  Neither found the same issue — SAST detects code-level flaws, DAST detects runtime issues.

- **DAST + SCA overlap:**  
  None — ZAP does not analyze library CVEs.

- **SAST + SCA overlap:**  
  None — CodeQL looks at logic and patterns, not CVE data.

---

## Summary

| Scanner                | What It Caught                 | Why It Matters                          |
| ---------------------- | ------------------------------ | --------------------------------------- |
| CodeQL (SAST)          | Flask debug mode (High)        | Prevents remote code execution          |
| ZAP (DAST)             | 7 runtime configuration issues | Strengthens HTTP-level security posture |
| Dependency-Check (SCA) | No CVEs                        | Dependencies are up-to-date             |

**Conclusion:**  
All three scanners cover different layers—together providing a full security view.
