# Thunderbird Outlook Parity & Productivity Suite
## Official Auditor & Binary Release Repository

[![ISO 27001 Compliant](https://img.shields.io/badge/Security-ISO%2FIEC%2027001%3A2022-brightgreen)](docs/compliance/ISO-27001-ISMS-POLICY.md)
[![Seychelles DPA 2023](https://img.shields.io/badge/Privacy-Seychelles%20DPA%202023-blue)](docs/compliance/ISO-27701-PRIVACY-NOTICE.md)
[![Thunderbird 128+ ESR](https://img.shields.io/badge/Host-Thunderbird%20128%2B%20ESR-orange)](https://www.thunderbird.net/)
[![Build Version](https://img.shields.io/badge/Release-v1.0.0--b10-blueviolet)](build-info.json)

An enterprise-grade Mozilla Thunderbird extension providing complete feature parity with Microsoft Outlook for the **Government of Seychelles (DICT)** and enterprise groupware environments.

> [!NOTE]
> **Auditor & Distribution Notice:**  
> This public repository contains official production binaries (`.xpi`), SHA-256 cryptographic checksums, automated CI test verification logs, and the complete ISO/IEC 27001 / Seychelles DPA 2023 compliance dossier. Proprietary core source code is retained in private sovereign infrastructure.

---

## 📦 Distribution Packages

| Package File | Target Platform | Description |
| :--- | :--- | :--- |
| [`thunderbird-outlook-suite.xpi`](thunderbird-outlook-suite.xpi) | Thunderbird 128+ ESR / 156+ (Win/Linux) | Latest Production MailExtension Package |
| [`thunderbird-outlook-suite-v1.0.0-b10.xpi`](thunderbird-outlook-suite-v1.0.0-b10.xpi) | Versioned Release | Immutable Build Artifact (#10) |
| [`SHA256SUMS.txt`](SHA256SUMS.txt) | Integrity Verification | Cryptographic Hash Manifest |
| [`build-info.json`](build-info.json) | Build Provenance | Metadata, Git Commit Hash & Timestamp |

### 🔒 Cryptographic Checksums (SHA-256)
```text
c16d9d5b30cea7fa837c0db073449d0f0a1d6fcf96cd0e536b551b9ba911eaa5  thunderbird-outlook-suite.xpi
c16d9d5b30cea7fa837c0db073449d0f0a1d6fcf96cd0e536b551b9ba911eaa5  thunderbird-outlook-suite-v1.0.0-b10.xpi
```

To verify integrity on Windows:
```powershell
certutil -hashfile thunderbird-outlook-suite.xpi SHA256
```

To verify integrity on Linux:
```bash
sha256sum -c SHA256SUMS.txt
```

---

## 🏛️ Key Capabilities & Outlook Feature Parity

1. **`ad.gov.sc` Zero-Config Onboarding**: Autodetects endpoints, takes over Thunderbird profile setup, and provisions Mail, Calendar, Microsoft To Do, Notes, and GAL.
2. **Executive Delegation ("Secretary" / Boss Profiles)**: Manage multiple boss accounts simultaneously, overlay schedules with color coding, and send emails/invites with *"Send on Behalf Of"* RFC 5322 headers while respecting private appointment flags.
3. **Microsoft To Do & CalDAV Task Synchronization**: Full subtask tracking, categories, due dates, and priority queues.
4. **Sticky Notes / OneNote Synchronization**: Dual Graph/IMAP notes cards with pin and delete capabilities.
5. **Meeting Scheduling Assistant**: Multi-attendee Free/Busy timeline grid with conflict detection.
6. **Compose Flags & Categories**: Follow-up flags (Today, Tomorrow, This Week), Master Categories, Mailtip domain alerts, and Distribution List expansion.
7. **Contact Card Popovers**: Real-time presence badges, Out-of-Office (OOO) alerts, and GAL contact lookup.

---

## 🛡️ Security, Privacy & Cryptographic Standards

- **Zero Cleartext Credentials**: No unencrypted tokens or passwords ever touch storage or disk.
- **Hardware-Bound Encryption Key**: Per-installation `crypto.getRandomValues()` secret stored in sandboxed extension storage, deriving AES-GCM 256-bit PBKDF2 keys.
- **Field-Level Encryption (FLE)**: Offline task bodies and note contents encrypted at rest in IndexedDB (Dexie.js).
- **Zero-Knowledge Offline Unlock**: Unlocks offline groupware data using the user's password-derived key.
- **Zero Telemetry (0%)**: Strict prohibition on external trackers, CDNs, or foreign analytics endpoints.

---

## 📋 Auditor Dossier & Compliance Reports

- 📑 **Master Compliance Report**: [`docs/compliance/AUDITOR-COMPLIANCE-REPORT.md`](docs/compliance/AUDITOR-COMPLIANCE-REPORT.md)
- 🔒 **ISO/IEC 27001:2022 ISMS Policy**: [`docs/compliance/ISO-27001-ISMS-POLICY.md`](docs/compliance/ISO-27001-ISMS-POLICY.md)
- 👤 **Seychelles DPA 2023 & ISO 27701 Privacy Notice**: [`docs/compliance/ISO-27701-PRIVACY-NOTICE.md`](docs/compliance/ISO-27701-PRIVACY-NOTICE.md)
- 🏛️ **DICT Security Baseline**: [`docs/compliance/DICT-SECURITY-BASELINE.md`](docs/compliance/DICT-SECURITY-BASELINE.md)
- 🚨 **National CERT-SC Incident Response**: [`docs/compliance/CERT-SC-INCIDENT-RESPONSE.md`](docs/compliance/CERT-SC-INCIDENT-RESPONSE.md)
- 🔑 **Cryptographic Controls Specification**: [`docs/compliance/CRYPTOGRAPHIC-CONTROLS.md`](docs/compliance/CRYPTOGRAPHIC-CONTROLS.md)
- 💾 **Portable Media & Flash Drive Security**: [`docs/compliance/PORTABLE-MEDIA-SECURITY.md`](docs/compliance/PORTABLE-MEDIA-SECURITY.md)
- 🌳 **Architectural Decision Tree & Dead-End Postmortems**: [`docs/DECISION_TREE_TRACKER.md`](docs/DECISION_TREE_TRACKER.md)
- 🎨 **UI Design System & Supernova Tokens**: [`docs/STYLE_GUIDE.md`](docs/STYLE_GUIDE.md)
- 📜 **Release Notes & Version History**: [`RELEASE_NOTES.md`](RELEASE_NOTES.md) | [`CHANGELOG.md`](CHANGELOG.md)

---

## 🧪 Automated Test Evidence & Verification Proofs

- 📋 **Vitest 100% Interaction Test Suite**: [`test-evidence/test-evidence.log`](test-evidence/test-evidence.log)
- 📋 **CI Compliance Policy Verification**: [`test-evidence/compliance-audit.log`](test-evidence/compliance-audit.log)
- 📋 **Active Bug & Task Trackers**: [`TODO_TASKS.md`](TODO_TASKS.md) | [`TODO_FIXES.md`](TODO_FIXES.md)

---

## 💻 Installation Instructions

1. Download [`thunderbird-outlook-suite.xpi`](thunderbird-outlook-suite.xpi).
2. Open **Mozilla Thunderbird** (v128+ ESR or v156+).
3. Navigate to **Tools** > **Add-ons and Themes** (or press `Ctrl+Shift+A`).
4. Click the gear icon (⚙️) in the top-right corner of the Add-ons Manager and select **"Install Add-on From File…"**.
5. Select the downloaded `thunderbird-outlook-suite.xpi` file and click **"Add"**.
6. The extension will automatically open the **Onboarding Wizard** to configure your account.
