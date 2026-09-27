# Release Notes & Auditor Changelog
**Project:** Thunderbird Outlook Parity & Productivity Suite  
**Authority:** Department of Information Communications Technology (DICT), Republic of Seychelles  
**Classification:** RESTRICTED / AUDITOR & PUBLIC RELEASE  
**Target Host:** Mozilla Thunderbird 128+ ESR (Supernova / Nebula UI)  

---

## [1.0.0] - 2026-09-27

### 🏛️ Sovereign Infrastructure & Onboarding
- **`ad.gov.sc` Autodiscovery Engine**: Automated RFC 6186, MX, and Microsoft 365 hybrid endpoint discovery.
- **Dedicated `<onboarding-wizard>`**: Full-tab onboarding taking over fresh Thunderbird profile setup with one-click credential provisioning.
- **Native Spaces Rail Integration**: Direct `spacesToolbar` left-rail launcher and responsive Full Tab Suite manager.

### 👔 Executive Delegation ("Secretary" / Boss System)
- **Multi-Boss Profile Switching**: Executive assistants can view and manage multiple executive accounts simultaneously without shared master passwords.
- **"Send on Behalf Of" Compliant Headers**: Automated RFC 5322 MIME structuring (`Sender` vs `From`) with full non-repudiation audit trails.
- **Private Sensitivity Protection**: Automatic masking of Outlook `sensitivity: "private"` items for unauthorized delegates.

### ⚡ Enterprise Groupware Feature Parity
- **Microsoft To Do & CalDAV Task Synchronization**: Delta sync engine with subtasks, due dates, priority, and categories.
- **Sticky Notes / OneNote Sync**: Offline-first yellow note cards with pin/delete management and dual IMAP/Graph synchronization.
- **Meeting Scheduling Assistant**: Multi-attendee Free/Busy timeline grid with `/findMeetingTimes` conflict detection.
- **Email Follow-Up Flags & Categories**: Today, Tomorrow, This Week flags and Master Category palette in Compose window.
- **Contact Card Popover**: Real-time presence indicators, Out-of-Office (OOO) alerts, and GAL contact cards.

### 🔒 Cryptography, Privacy & ISO/IEC 27001 Compliance
- **Zero Cleartext Credentials**: Hardware-bound per-installation secret deriving AES-GCM 256-bit PBKDF2 keys.
- **Field-Level Encryption (FLE)**: Offline Task bodies and Note contents encrypted at rest in IndexedDB (Dexie.js).
- **Zero-Knowledge Offline Mode**: Full offline groupware access authenticated via password-derived key.
- **ISO 27701 Right to Erasure**: Privacy purge mechanism with tamper-evident SHA-256 audit log preservation.
- **100% Automated Test Evidence**: 15 test suites and 43 test assertions verified in CI across Windows and Linux.
