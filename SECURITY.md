# Security Scanning

This project uses a layered security pipeline consisting of:

- **SAST (CodeQL)**
- **SCA (Dependency-Check)**
- **DAST (OWASP ZAP)**

---

## SAST – CodeQL

- **Runs on:** Push to `main`, and Pull Requests
- **Finds:**
  - Logic flaws
  - Input sanitization issues
  - Insecure patterns (e.g., debug mode)
- **Latest results:**
  - 1 High severity issue: Flask debug mode enabled
- **Artifacts:** GitHub Security → Code scanning alerts
- **Workflow file:** `.github/workflows/codeql.yml`

---

## SCA – Dependency-Check

- **Runs on:** Push to `main`
- **Finds:**
  - Vulnerable libraries
  - Known CVEs
- **Latest results:**
  - 0 vulnerabilities found
- **Artifacts:**
  - `sca-reports/dependency-check-report.html`
- **Workflow file:** `.github/workflows/sca.yml`

---

## DAST – OWASP ZAP

- **Runs on:** Push to `main`
- **Tests:**
  - Live application running on CI
  - Missing headers
  - HTTP security issues
- **Artifacts:**
  - `zap-fullscan-reports/*.html`
  - `zap-baseline-reports/*.json`

**Latest results:**

- 7 warnings (missing headers + no HTTPS)

---

## How to Interpret Results

See:

- `SCAN_ANALYSIS.md` for detailed results
- `SCANNER_COMPARISON.md` for tool comparison
- GitHub Security tab for CodeQL issues

---

## Reporting Vulnerabilities

Please email S10258606@connect.np.edu.sg
