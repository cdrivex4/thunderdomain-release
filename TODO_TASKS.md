# Engineering Task Tracker (TODO_TASKS.md)

**Project:** Thunderbird Outlook Parity & Productivity Suite  
**Target Environment:** Windows 11/Server & Ubuntu 24.04 LTS (Thunderbird 128+ ESR)  
**Security Standard:** Seychelles DICT ISO/IEC 27001:2022 & DPA 2023  

---

## Phase 1: Foundation, Compliance Suite, Build Toolchain & CI Matrix
- [x] **Documentation & Architecture**
  - [x] Create comprehensive [`README.md`](README.md) with system wiring diagrams.
  - [x] Create [`TODO_TASKS.md`](TODO_TASKS.md) and [`TODO_FIXES.md`](TODO_FIXES.md).
  - [x] Create [`docs/DECISION_TREE_TRACKER.md`](docs/DECISION_TREE_TRACKER.md) (`01.01`).
  - [x] Create [`docs/OUT_OF_SCOPE_AND_DEFERRED.md`](docs/OUT_OF_SCOPE_AND_DEFERRED.md) (`01.01`).
  - [x] Create [`docs/DOCUMENTATION_DEPENDENCY_GRAPH.md`](docs/DOCUMENTATION_DEPENDENCY_GRAPH.md) (`01.01`).
  - [x] Draft architectural specification in [`docs/architecture.md`](docs/architecture.md).
- [x] **Interdependent Compliance Notices**
  - [x] [`docs/compliance/ISO-27001-ISMS-POLICY.md`](docs/compliance/ISO-27001-ISMS-POLICY.md): Full DICT ISO 27001:2022 control mapping.
  - [x] [`docs/compliance/ISO-27701-PRIVACY-NOTICE.md`](docs/compliance/ISO-27701-PRIVACY-NOTICE.md): Seychelles Data Protection Act 2023 & ISO 27701 PII handling.
  - [x] [`docs/compliance/DICT-SECURITY-BASELINE.md`](docs/compliance/DICT-SECURITY-BASELINE.md): Seychelles Government IT & `ad.gov.sc` active directory baseline.
  - [x] [`docs/compliance/CERT-SC-INCIDENT-RESPONSE.md`](docs/compliance/CERT-SC-INCIDENT-RESPONSE.md): National CERT-SC audit log schema & incident protocol.
  - [x] [`docs/compliance/CRYPTOGRAPHIC-CONTROLS.md`](docs/compliance/CRYPTOGRAPHIC-CONTROLS.md): Web Crypto AES-GCM 256-bit token vault spec.
  - [x] [`docs/compliance/THIRD-PARTY-NOTICES.md`](docs/compliance/THIRD-PARTY-NOTICES.md): Open source licensing & SBOM audit.
- [x] **Toolchain & Build Infrastructure**
  - [x] Configure `package.json`, `tsconfig.json`, `vite.config.ts`, `vitest.config.ts`, `playwright.config.ts`.
  - [x] Implement multi-platform GitHub Actions CI matrix (`.github/workflows/ci-matrix.yml`) for Windows and Ubuntu 24.04 LTS.

---

## Phase 2: `ad.gov.sc` Autodiscovery, Identity & Cryptographic Vault
- [x] **Domain Autodiscovery & Provisioning**
  - [x] Implement `src/background/account-detector.ts` for RFC 6186 / MX / M365 endpoint discovery.
  - [x] Implement `<onboarding-wizard>` for zero-config `ad.gov.sc` onboarding.
- [x] **ISO 27001 Cryptographic Token Vault & FLE**
  - [x] Implement `src/common/crypto-vault.ts` using Web Crypto API (AES-GCM 256-bit, PBKDF2 100,000 rounds).
  - [x] Implement `src/common/device-secret.ts` for per-installation hardware-bound secret.
  - [x] Implement Field-Level Encryption in `src/storage/delta-store.ts` for offline task/note data at rest.
  - [x] Implement `src/background/token-refresh-manager.ts` for background OAuth2 token lifecycle.
- [x] **Tamper-Evident Audit Logging & AI Diagnostics**
  - [x] Implement `src/background/audit-logger.ts` for CERT-SC compliant event logging.
  - [x] Implement `src/common/diagnostic-exporter.ts` for AI crash log export.
- [x] **Resilience & Synchronization Core**
  - [x] Implement `src/ui/base-component.ts` (`BaseWebComponent`) with error boundary & RAII.
  - [x] Implement `src/storage/sync-status-store.ts` for observable sync & network state.
  - [x] Implement `src/background/delta-sync-engine.ts` for scheduled & offline flush delta syncing.
- [x] **Automated Testing**
  - [x] Unit tests for `crypto-vault.ts` (`tests/unit/crypto-vault.test.ts`).
  - [x] Unit tests for Field-Level Encryption (`tests/unit/fle-encryption.test.ts`).
  - [x] Button interaction tests for `<onboarding-wizard>` (`tests/ui/onboarding.test.ts`).
  - [x] UI tests for sync status badge (`tests/ui/sync-status.test.ts`).

---

## Phase 3: Core Parity Subsystems (Tasks, Notes, Flags & Categories)
- [x] **Microsoft To Do & CalDAV Task Synchronization**
  - [x] Implement `src/api/graph/todo.ts` for `/v1.0/me/todo/lists` delta synchronization.
  - [x] Build `<task-board>` Vanilla TypeScript Web Component with subtasks and category filtering.
- [x] **Notes & OneNote Synchronization**
  - [x] Implement `src/api/graph/notes.ts` for OneNote / Sticky Notes Graph sync.
  - [x] Implement `src/api/caldav/imap-notes.ts` for on-premise IMAP Notes MIME encoding.
  - [x] Build `<notes-board>` Web Component with sticky yellow card design and pin/delete actions.
- [x] **Email Follow-Up Flags, Categories & Compose Tools**
  - [x] Implement Follow-Up Flagging in `<compose-sidebar>` (Today, Tomorrow, This Week).
  - [x] Implement Master Category tagging in `<compose-sidebar>`.
  - [x] Implement Mailtip security warnings for external non-gov.sc recipients.
  - [x] Implement Distribution List (DL) expansion indicators in GAL search.
- [x] **Outlook Contact Card Popover**
  - [x] Build `<contact-card-popover>` with presence indicator, OOO alert banner, and actions.
- [x] **Automated Testing**
  - [x] Vitest component interaction tests for `<task-board>` and `<notes-board>`.
  - [x] Vitest component tests for `<contact-card-popover>` (`tests/ui/contact-card.test.ts`).

---

## Phase 4: Executive Delegation ("Secretary" / Boss) & Scheduling Assistant
- [x] **Multi-Profile Delegation Engine**
  - [x] Implement `src/background/delegation-manager.ts` and `src/api/graph/delegation.ts`.
  - [x] Build `<delegation-modal>` for fast profile switching and boss profile addition.
- [x] **Meeting Scheduling Assistant**
  - [x] Implement `src/api/graph/events.ts` with `/findMeetingTimes` integration.
  - [x] Build `<scheduling-assistant>` Web Component with multi-attendee Free/Busy grid.
- [x] **Automated Testing**
  - [x] Vitest tests for delegation modal (`tests/ui/delegation.test.ts`) and scheduler (`tests/ui/scheduler.test.ts`).

---

## Phase 5: Calendar Bridge, GAL Autocomplete & Out of Office (OOF)
- [x] **WebExtension Experiment Bridges**
  - [x] `src/experiments/spaces-bridge/`: Spaces toolbar deep injection.
  - [x] `src/experiments/calendar-bridge/`: Lightning calendar hook.
  - [x] `src/experiments/addressbook-bridge/`: Global Address List provider.
- [x] **Compose Window & Out of Office (OOF)**
  - [x] Build `<compose-sidebar>` with real-time GAL search and "Send on Behalf" headers.
  - [x] Build `<oof-modal>` for automatic replies configuration.
  - [x] Build `<settings-modal>` with ISO 27701 "Purge Local Profile Cache" button.
- [x] **Automated Testing**
  - [x] Vitest tests for `<compose-sidebar>` and `<settings-modal>`.
  - [x] Protocol unit tests for RFC 5545 iCalendar & RFC 6350 vCard parsers (`tests/unit/ical-parser.test.ts`).

---

## Phase 6: Hardening, Stress Testing, DICT Compliance Audit & Release
- [x] **Automated Test Suite Complete (100% Button & Control Coverage)**
  - [x] Tier 1 Vitest interaction tests for all 7 Web Components.
  - [x] Tier 2 unit tests for RAII, Cryptographic Vault, and Protocol Parsers.
  - [x] Tier 3 Playwright E2E journey test.
- [x] **Packaging & Distribution**
  - [x] Packaging script `scripts/package-xpi.js`.
