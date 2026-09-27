# Changelog & Version History

All notable changes, architectural milestones, and compliance updates for the **Thunderbird Outlook Parity & Productivity Suite** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] - 2026-09-27

### Added
- Native Thunderbird `spacesToolbar` integration mounting the Outlook Suite directly into the Supernova left rail.
- Full-tab navigation routing for `<spaces-rail>`, `<task-board>`, `<notes-board>`, `<scheduling-assistant>`, and `<delegation-modal>`.
- Cold-start zero-account detection automatically launching the `<onboarding-wizard>` across new profiles.
- Field-Level Encryption (FLE) in `DeltaStore` (`src/storage/delta-store.ts`) for Task bodies and Note HTML at rest using Web Crypto AES-GCM 256-bit.
- Zero-knowledge offline unlock mechanism via PBKDF2 password-derived key.
- Living auditor compliance dossier `docs/compliance/AUDITOR-COMPLIANCE-REPORT.md` and CI compliance verifier `scripts/verify-compliance.js`.
- Executive Delegation modal with "Send on Behalf Of" RFC 5322 header builder and private appointment masking.
- Microsoft 365 Graph API v1.0 and CalDAV/CardDAV delta sync engine with 5-minute background alarms.
- 15 automated Vitest test suites with 43 passing tests across UI and unit modules.

### Changed
- Converted extension browser action from cramped 48px dropdown popup to full responsive tab router.
- Switched cryptographic master key storage from shared static passphrase to per-installation `crypto.getRandomValues()` device secret.
- Protected `auditLogs` table during ISO 27701 local cache wipe to maintain ISO 27001 A.8.15 integrity.
- Re-routed compiled `.xpi` distribution packages, versioned artifacts, and checksums into a dedicated `build/` directory.
- Configured public release pipeline (`scripts/publish-release.js` / `release.bat`) to automate auditor synchronization with strict exclusion of private internal blueprints (`docs/artifacts/`).

### Fixed
- Fixed background worker execution in Manifest V2 by introducing `src/background.html` with ES module support, resolving `SyntaxError: import declarations may only appear at top level of a module`.
- Fixed token refresh silent session expiration by adding `TokenRefreshManager`.
- Fixed tab creation URL resolution on fresh Thunderbird window initialization.
- Fixed UI bundle script imports to use reliable root-relative paths.
