# Business Logic Vulnerability in POS PIN Verification — PIN Substitution Between Accounts (Facturaz)

## Summary
A **business logic vulnerability** exists in the POS PIN protection mechanism of Facturaz that allows a user to replace the stored, encrypted POS PIN of one account with the encrypted POS PIN value from another account. As a result, an attacker can unlock the POS-protected area of the first account using the PIN associated with the second account — fully bypassing the intended account-specific PIN verification.

- **Program / Platform:** Facturaz — OpenBugBounty
- **Vulnerability Type:** Business Logic Error / Improper PIN Validation
- **Severity:** Medium–High (CVSS 6.5 — Medium, see below)
- **Status:** Fixed
- **Submission ID:** 4611551
- **Reported:** 2026-06-19

## Affected URL
```
https://facturaz.es/en/app/dashboard/pos
```

## Description
The application uses a POS PIN mechanism to restrict access to protected functionality. When a PIN is configured, the application stores an **encrypted value** representing that PIN.

The encrypted PIN value is not cryptographically bound to the account it belongs to. As a result, an attacker can take the encrypted representation of a PIN from one account they control and submit it to replace the encrypted PIN value stored on a different account. After the replacement, the first account can then be unlocked using the PIN value associated with the second account.

This shows the application **trusts the submitted encrypted PIN value as authoritative**, without verifying server-side that it was originally generated for the account making the request.

## Prerequisites
- Two user accounts under the attacker's control
- An intercepting proxy such as Burp Suite
- Ability to modify requests before they reach the server

## Steps to Reproduce
1. Log in to **Account A**.
2. Go to Settings → Security and configure a POS PIN for Account A.
3. Navigate to the POS section: `https://facturaz.es/en/app/dashboard/pos`.
4. Unlock the POS.
5. Log in to **Account B**.
6. Go to Settings → Security and configure a POS PIN for Account B — set it to a known value, e.g. `0000`.
7. Intercept the request and copy the **encrypted** `pos_pin` value corresponding to PIN `0000`:
   ```
   PATCH /rest/v1/profiles?select=*&id=[ACCOUNT_B_ID]
   { "pos_pin": "[ENCRYPTED VALUE OF POS PIN]" }
   ```
8. Return to **Account A**. Refresh the page to capture the relevant endpoints, then find in Burp's HTTP history:
   ```
   GET /rest/v1/profiles?select=*&id=[ACCOUNT_A_ID]
   ```
9. Change the method to `PATCH` and replace Account A's `pos_pin` value with the encrypted `pos_pin` copied from Account B:
   ```
   PATCH /rest/v1/profiles?select=*&id=[ACCOUNT_A_ID]
   { "pos_pin": "[ENCRYPTED VALUE OF POS PIN OF ACCOUNT B]" }
   ```
10. Forward the modified request.
11. Attempt to unlock Account A using PIN `0000`.
12. Observe that the application accepts the PIN from Account B and unlocks the protected functionality of Account A.

### Expected Result
A POS PIN should be uniquely and verifiably bound to the account for which it was created. Encrypted PIN values from one account should never be accepted for another account.

### Actual Result
The application accepted an encrypted POS PIN value originating from a different account, allowing the PIN from Account B to unlock Account A's protected functionality.

## Impact
This vulnerability breaks the integrity of the POS PIN security mechanism and demonstrates that PIN validation is not properly tied to account identity.

Potential impacts include:
- Circumvention of intended POS PIN protections
- Unauthorized access to PIN-protected functionality
- Weakening of internal security controls
- Possibility of broader authorization issues if similar logic is reused elsewhere in the application

## Severity — CVSS v3.1
**Base Score: 6.5 (Medium)**
```
Vector: AV/AC/PR/UI/S/C/I/A
```
The vulnerability allows manipulation of a security control protecting account functionality and may enable unauthorized access to protected features.

## Proof of Concept
A video PoC demonstrating the complete attack flow is included in [`poc/`](./poc).

## Suggested Remediation
- Cryptographically bind stored PIN values to the account identifier
- Verify server-side that a submitted encrypted PIN belongs to the authenticated account
- Avoid accepting client-supplied encrypted security credentials as authoritative values
- Use account-specific salts or keys when generating stored PIN representations
- Perform integrity checks to prevent PIN substitution attacks

## Timeline
| Date | Event |
|------|-------|
| 2026-06-19 | Reported via OpenBugBounty (Submission ID 4611551) |

## Disclosure Status
Status: **Fixed**. Included here for portfolio/research documentation, in line with OpenBugBounty's disclosure policy.

## Credit
Reported by [Ahmed Alsiddig](https://www.linkedin.com/in/ahmed-alsiddig-6a0801287/) via OpenBugBounty.
