# ISO/IEC 27001:2022 ISMS Compliance Policy
## Information Security Management System Policy & Control Mapping

**Organization:** Government of Seychelles — Department of Information Communications Technology (DICT)  
**System Name:** Thunderbird Outlook Parity & Productivity Suite  
**Classification:** RESTRICTED / GOVERNMENT IT INFRASTRUCTURE  
**Standard:** ISO/IEC 27001:2022 / ISO/IEC 27002:2022  

---

## 1. Scope & Objective

The Department of Information Communications Technology (DICT) in Seychelles achieved ISO/IEC 27001 certification in November 2022. This document establishes the formal Information Security Management System (ISMS) policy and control mapping governing the development, deployment, and operational execution of the Thunderbird Outlook Parity Extension across all Government of Seychelles (`ad.gov.sc` and associated MDAs) workstations.

---

## 2. ISO/IEC 27001:2022 Control Mapping Matrix

| ISO 27001 Control | Description | Extension Architecture Implementation |
| :--- | :--- | :--- |
| **A.5.15 Access Control** | Limiting access to information and other associated assets based on business and security requirements. | • Multi-Factor OAuth2 / PKCE authentication.<br/>• No hardcoded credentials or backdoor bypasses.<br/>• Role-based access control for Executive Delegation. |
| **A.8.12 Data Leakage Prevention** | Preventing unauthorized disclosure of confidential government data. | • Privacy filtering for sensitive/confidential calendar events.<br/>• Zero telemetry leakage to external third-party endpoints.<br/>• Isolated sandboxed WebExtension execution model. |
| **A.8.15 Logging & Monitoring** | Recording events and generating audit trails to detect cybersecurity incidents. | • Tamper-evident structured audit logging (`src/background/audit-logger.ts`).<br/>• Compatible with National CERT-SC SIEM ingestion standards. |
| **A.8.20 Network Security** | Protecting information in networks and its supporting information processing facilities. | • Strict TLS 1.3 encryption in transit for all Graph, IMAP, and CalDAV calls.<br/>• Certificate pinning and strict HTTPS transport policies. |
| **A.8.24 Use of Cryptography** | Ensuring proper and effective use of cryptography to protect data confidentiality and integrity. | • Web Crypto API utilizing AES-GCM 256-bit encryption for all stored tokens.<br/>• PBKDF2 key derivation with unique nonces per transaction. |
| **A.8.28 Secure Coding** | Applying secure coding principles to software development. | • TypeScript strict mode with zero `any` allocations.<br/>• Complete ban on `eval()`, `Function()`, or unsafe `innerHTML`.<br/>• Explicit Resource Management (RAII) to eliminate memory leaks. |
| **A.8.31 Separation of Dev, Test & Prod** | Isolating development, testing, and production environments. | • Full mock test harnesses (MSW) ensuring no production credentials are used during CI/CD. |

---

## 3. Cryptographic & Token Storage Standard (Control A.8.24)

1. **Token Protection**: OAuth access tokens and refresh tokens must never be persisted in plaintext inside `localStorage` or unencrypted JSON files.
2. **Encryption Specification**:
   - Algorithm: **AES-GCM (Galois/Counter Mode)** with a 256-bit key length.
   - Nonce (IV): 96-bit cryptographically secure random value generated via `crypto.getRandomValues()`. Nonces are unique per encryption operation and never reused.
   - Key Derivation: PBKDF2 with SHA-256 and a minimum of 100,000 iterations.
3. **Memory Safety**: Tokens held in memory are cleared from heap reference upon session termination or profile logout.

---

## 4. Executive Delegation & Access Control (Control A.5.15)

When an executive assistant ("Secretary") is granted delegated access to a senior official's ("Boss") profile:
1. **Explicit Permission Verification**: The extension verifies delegated permissions via Microsoft Graph API scopes (`Mail.ReadWrite.Shared`, `Calendars.ReadWrite.Shared`) or Exchange RBAC before mounting the mailbox tree.
2. **Privacy Enforcement**: Calendar events and tasks marked with `Sensitivity: Private` in Outlook/Exchange are automatically masked in the delegate's view unless explicit delegate private item visibility is confirmed.
3. **Audit Trail**: Every email or calendar modification executed by a delegate logs a structured event containing:
   - Initiator: `secretary@gov.sc`
   - Principal: `boss@gov.sc`
   - Action: `CREATE_EVENT_ON_BEHALF`
   - Timestamp (ISO 8601 UTC)
   - SHA-256 integrity hash

---

## 5. Incident Response & CERT-SC Reporting (Control A.8.16)

In the event of an authentication anomaly, repeated cryptographic decryption failure, or unauthorized scope escalation attempt:
1. The extension terminates the active session immediately.
2. A structured security notice is recorded in the audit log.
3. Instructions for alerting the **National CERT-SC** (`cert@ict.gov.sc`) are presented to the workstation administrator.
