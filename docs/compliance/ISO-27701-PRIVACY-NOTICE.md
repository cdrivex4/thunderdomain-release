# ISO/IEC 27701 & Seychelles Data Protection Act 2023 Compliance Notice
## Privacy Information Management System (PIMS) Specification

**Organization:** Government of Seychelles — Department of Information Communications Technology (DICT)  
**Applicable Legislation:** Seychelles Data Protection Act 2023 / Electronic Transactions Act  
**Applicable Standard:** ISO/IEC 27701:2019 (PIMS Extension to ISO/IEC 27001)  

---

## 1. Purpose & Principles

This notice outlines the privacy protections, Personally Identifiable Information (PII) handling rules, and data minimization mechanisms implemented within the Thunderbird Outlook Parity Suite to ensure compliance with the **Seychelles Data Protection Act 2023** and **ISO/IEC 27701:2019**.

The extension operates under four core privacy principles:
1. **Data Minimization**: Only mail headers, calendar metadata, tasks, notes, and contacts required for active client operations are cached locally.
2. **Purpose Limitation**: No personal or organizational data is processed for any purpose other than providing client-side email and productivity workflows.
3. **Local-First & Zero External Telemetry**: No user data, analytics, or behavioral telemetry is transmitted to external servers or third-party tracking services. All network communication is restricted exclusively to the authorized mail/directory servers configured by the user or domain admin.
4. **User Control & Right to Erasure**: Users have immediate access to purge all locally cached messages, tokens, contacts, and logs.

---

## 2. Categories of PII Processed

| Category | Specific Data Elements | Storage Location | Retention & Lifecycle |
| :--- | :--- | :--- | :--- |
| **Identity & Authentication** | Email address, User Principal Name (UPN), Display Name, OAuth2 Access/Refresh Tokens | Encrypted IndexedDB (`src/common/crypto-vault.ts`) | Maintained until sign-out or account deletion. |
| **Communications & Messaging** | Message subjects, sender/recipient addresses, timestamps, body excerpts | Local IndexedDB (`Dexie.js`) cache | Synced dynamically; purged upon cache reset. |
| **Calendar & Scheduling** | Meeting titles, start/end times, attendee lists, room locations, free/busy status | Local IndexedDB cache & Thunderbird Lightning DB | Synced dynamically; purged upon cache reset. |
| **Tasks & Notes** | To-Do titles, due dates, categories, note contents | Local IndexedDB cache | Synced dynamically; purged upon cache reset. |
| **Contacts & GAL** | Contact names, email addresses, department, phone numbers | Local memory & IndexedDB cache | Retained during active session; invalidated on TTL. |

---

## 3. Data Protection Rights & Technical Implementation

### 3.1 Right to Access & Portability (Section 18, DPA 2023)
Users can export their synchronized tasks, notes, and calendar items to standard interoperable formats (RFC 5545 `.ics` and JSON) at any time via the extension settings.

### 3.2 Right to Erasure / "Data Wipe" (Section 21, DPA 2023)
The extension provides a dedicated **"Purge Local Profile Cache"** function in `<settings-modal>`. When triggered:
1. All records in IndexedDB (`OutlookSuiteDatabase`) are permanently truncated.
2. The cryptographic key vault is destroyed from memory and storage.
3. All active network subscriptions and tokens are revoked.
4. The client returns to a clean, unconfigured state.

---

## 4. Cross-Border Data Transfers

In compliance with the Seychelles Data Protection Act 2023 provisions on cross-border data flows:
- When operating in government hybrid environments, data transit is strictly confined to DICT-approved sovereign infrastructure or authorized cloud tenants under verified data protection agreements.
- All network channels mandate TLS 1.3 encryption with strong cipher suites (`TLS_AES_256_GCM_SHA384` / `TLS_CHACHA20_POLY1305_SHA256`).
