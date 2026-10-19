# Bug Bounty Reports

A collection of security vulnerabilities I discovered and reported through public bug bounty / vulnerability disclosure programs. Each report includes a technical write-up and a proof-of-concept (PoC), published in line with each platform's disclosure policy.

## ⚠️ Disclaimer

All vulnerabilities in this repository were discovered and reported through **official bug bounty or vulnerability disclosure programs** (HackerOne, OpenBugBounty), following each program's responsible disclosure policy. Proof-of-concept code/screenshots are provided for **educational and research purposes only**. Do not use any of this against systems you do not own or do not have explicit authorization to test. I am not responsible for any misuse of this information.

## 📋 Summary

| # | Vulnerability | Platform | Target | Severity | Status | Report |
|---|---------------|----------|--------|----------|--------|--------|
| 1 | Reflected DOM-Based XSS | HackerOne | Henkel VDP | High | Duplicate | [View](./01-reflected-dom-xss-henkel) |
| 2 | 2FA Bypass | OpenBugBounty | Facturaz | High | Fixed | [View](./02-2fa-bypass-facturaz) |
| 3 | Business Logic Flaw | OpenBugBounty | Facturaz | Medium | Fixed | [View](./03-business-logic-facturaz) |
| 4 | Information Disclosure | HackerOne | Mars VDP | Informative | Duplicate | [View](./04-information-disclosure-mars) |

## 📁 Structure

Each folder contains:
- `README.md` — full technical write-up (summary, root cause, steps to reproduce, impact, status)
- `poc/` — proof-of-concept script, request samples, or screenshots demonstrating the issue

## 🔎 About

I'm a security researcher participating in public bug bounty and VDP programs on HackerOne and OpenBugBounty.

- GitHub: [@a7meda1s1dd1g](https://github.com/a7meda1s1dd1g)
- LinkedIn: [Ahmed Alsiddig](https://www.linkedin.com/in/ahmed-alsiddig-6a0801287/)

## 📜 License

This repository is licensed under the [MIT License](./LICENSE) — applies to original write-up content and PoC material I authored. It does not grant rights to any proprietary software referenced in the reports.

## 🙏 Acknowledgements

Thanks to the security teams at Henkel, Facturaz, and Mars for their collaboration through their respective disclosure programs.
