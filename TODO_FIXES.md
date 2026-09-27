# Bug Tracker, Quirks & Compliance Fixes (TODO_FIXES.md)

**Project:** Thunderbird Outlook Parity & Productivity Suite  
**Tracking Scope:** Cross-platform OS edge cases, Thunderbird API quirks, ISO compliance remediations, and performance fixes.

> **See also:** [`docs/artifacts/01.07_swot_analysis_and_structural_review.md`](docs/artifacts/01.07_swot_analysis_and_structural_review.md) — Independent security review  
> **See also:** [`docs/artifacts/01.08_gap_analysis_supplemental.md`](docs/artifacts/01.08_gap_analysis_supplemental.md) — Outlook parity gap analysis

---

## 🔍 Active & Monitored Issues

### 1. Platform & OS Quirks (Windows 11 vs Ubuntu 24.04 LTS)
- [x] **Quirk-001: Web Crypto Storage Persistence on Linux**
  - *Issue*: On some headless or non-GNOME Ubuntu Linux setups, `window.crypto.subtle` keys are held in memory but lost across restarts if not backed by an encrypted master key in IndexedDB.
  - *Fix*: Implement a PBKDF2 salt + AES-GCM encrypted master key wrapper backed by Dexie.js so keys persist deterministically on both Windows (DPAPI-friendly) and Ubuntu 24.04.
  - *Status*: **Resolved in `src/common/crypto-vault.ts`**.

- [ ] **Quirk-002: Thunderbird 128+ Supernova CSS Token Changes**
  - *Issue*: Thunderbird 128 (Nebula/Supernova) replaced legacy XUL color variables with standard CSS Custom Properties (`--color-text-primary`, `--color-background-secondary`, `--color-accent`).
  - *Fix*: Web Components must use native fallback tokens (e.g. `var(--color-text-primary, #1e1e1e)`) to guarantee visual fidelity across custom user themes.
  - *Status*: **Monitored in Web Components styling**.

---

### 2. Protocol & Synchronization Edge Cases
- [ ] **Fix-101: Microsoft Graph API 429 Rate-Limiting & Jitter**
  - *Issue*: Rapid delta polling across multiple delegated mailboxes can trigger HTTP 429 (`Too Many Requests`) with `Retry-After` header.
  - *Fix*: Exponential backoff with randomized jitter implemented in `src/api/graph/client.ts`. Centralized request queuing still needed for multi-account scenarios.
  - *Status*: **Partially resolved. Queuing: Scheduled Phase 2**.

- [ ] **Fix-102: CalDAV `sync-collection` Missing Support on Legacy Servers**
  - *Issue*: Older on-premise CalDAV servers may not implement RFC 6578 (`sync-collection`) and fail when receiving sync-token PROPFIND requests.
  - *Fix*: Implement fallback to `getetag` / `getctag` change detection when server does not advertise `calendar-query` / `sync-collection` support in `DAV:` header.
  - *Status*: **Scheduled in Phase 3**.

- [x] **Fix-103: Token Refresh — Silent Session Expiry (SHOWSTOPPER)**
  - *Issue*: OAuth2 access tokens expire after ~60 minutes. No refresh code existed — users would be silently unauthenticated after 1 hour.
  - *Fix*: Created `src/background/token-refresh-manager.ts` + wired `messenger.alarms` scheduler in `background.ts`.
  - *Status*: **RESOLVED in 01.07/01.08 review session**.

- [x] **Fix-104: Background Sync Never Ran After Initial Provision**
  - *Issue*: `alarms` permission declared in manifest but `messenger.alarms.create()` was never called. Extension synced once then stopped.
  - *Fix*: `background.ts` now creates `delta-sync-and-refresh` alarm at 5-minute intervals.
  - *Status*: **RESOLVED in 01.07/01.08 review session**.

- [ ] **Fix-105: Delta Sync Orchestrator Missing**
  - *Issue*: Individual API clients exist but no orchestrator calls them on each alarm tick.
  - *Fix*: Implement `src/background/delta-sync-engine.ts` that coordinates Graph + CalDAV sync per account.
  - *Status*: **Scheduled Phase 2 hardening**.

- [ ] **Fix-106: Token Expiry Not Checked in GraphClient.getBearerToken()**
  - *Issue*: GraphClient decrypts the credential envelope but does not parse the token payload to check if it has expired before making the API call.
  - *Fix*: Parse `StoredTokenPayload.expiresAt` and throw `TokenExpiredError` before any API call.
  - *Status*: **Scheduled Phase 2**.

---

### 3. Executive Delegation & Privacy Edge Cases
- [ ] **Fix-201: Outlook "Private" Calendar Event Masking**
  - *Issue*: When an executive assistant queries a boss's calendar, items marked `sensitivity: "private"` in Outlook must not expose body, attendees, or subject to delegates without explicit delegate private item permissions.
  - *Fix*: In `src/background/delegation-manager.ts`, sanitize event payloads for delegated profiles: display "Private Appointment" and obscure description unless `canViewPrivateItems` scope is explicitly granted.
  - *Status*: **Sanitization implemented. UI enforcement Scheduled Phase 4**.

- [ ] **Fix-202: "Send on Behalf Of" vs "Send As" Header Compliance**
  - *Issue*: Exchange / M365 rejects messages where `From` is modified unless the user has "Send As", whereas "Send on Behalf" requires keeping the secretary's address in `Sender` and the boss's address in `From`.
  - *Fix*: Automate RFC 5322 header generation based on discovered delegation rights (`From: boss@gov.sc`, `Sender: secretary@gov.sc`). Header builder exists in `delegation-manager.ts`.
  - *Status*: **Scheduled in Phase 4**.

---

### 4. ISO/IEC 27001 & Seychelles DPA 2023 Compliance Fixes
- [x] **Comp-301: Zero Cleartext Password Retention**
  - *Requirement*: ISO 27001 Control A.8.24 forbids storing unencrypted passwords in plaintext in extension storage.
  - *Fix*: Enforce OAuth2 / PKCE for M365 and AES-GCM 256-bit encrypted credential envelopes for legacy basic auth.
  - *Status*: **Enforced in `src/common/crypto-vault.ts`**.

- [x] **Comp-302: Hardcoded Passphrase Security Violation**
  - *Requirement*: ISO 27001 Control A.8.24 — all users shared the same `'session-device-key'` passphrase, meaning any attacker could decrypt all credentials.
  - *Fix*: Created `src/common/device-secret.ts` — per-installation `crypto.getRandomValues()` secret stored in sandboxed `browser.storage.local`.
  - *Status*: **RESOLVED in 01.07 review session**.

- [x] **Comp-303: Audit Log Wipe Violation (ISO 27001 A.8.15)**
  - *Requirement*: Audit logs must be retained for incident response. `purgeLocalCache()` previously deleted them.
  - *Fix*: `purgeLocalCache()` now excludes `auditLogs` table. Separate privileged `purgeAuditLogsWithExport()` method added.
  - *Status*: **RESOLVED in 01.07 review session**.

- [ ] **Comp-304: Automated Local Data Erasure ("Right to Erasure")**
  - *Requirement*: Seychelles Data Protection Act 2023 requires user-accessible data purging.
  - *Fix*: "Purge Local Profile Cache" button implemented in `<settings-modal>`.
  - *Status*: **UI implemented. End-to-end flow validation Scheduled Phase 2**.

---

### 5. RAII & Resource Management Guardrails
- [x] **RAII-401: Event Listener Leak Prevention in Web Components**
  - *Issue*: Dynamic Web Components attached to the DOM accumulate event listeners if disconnected without explicit removeEventListener calls.
  - *Fix*: Use TypeScript 5.5+ `using` with `EventSubscriptionScope` to bind listener cleanup directly to DOM disconnect lifecycles.
  - *Status*: **Implemented in `src/common/raii.ts`**.

- [ ] **RAII-402: Error Boundary Pattern Missing in Web Components**
  - *Issue*: If a component throws in `render()` or `connectedCallback()`, it fails silently — the panel goes blank with no user feedback.
  - *Fix*: Wrap all component `render()` calls in try/catch that shows a friendly error state with a link to the diagnostic export.
  - *Status*: **Pattern defined in 01.08. Apply to all 8 components in Phase 2**.

---

### 6. Missing Outlook Parity Features (Phase 3+)
- [ ] **Parity-501: Email Follow-Up Flags** — Scheduled Phase 3
- [ ] **Parity-502: Email Categories (not just tasks/events)** — Scheduled Phase 3
- [ ] **Parity-503: Shared Mailbox Support** — Scheduled Phase 4
- [ ] **Parity-504: Room & Resource Booking** — Scheduled Phase 4
- [ ] **Parity-505: Meeting Response Tracking UI** — Scheduled Phase 3
- [ ] **Parity-506: Profile Photos in delegation/compose** — Scheduled Phase 3
- [ ] **Parity-507: Contact Card Popover (hover)** — Scheduled Phase 3
- [ ] **Parity-508: Mailtip Warnings in Compose** — Scheduled Phase 3
- [ ] **Parity-509: Distribution List Expansion** — Scheduled Phase 3

---

### 7. Infrastructure & Developer Experience
- [ ] **Infra-601: Sync Status Indicator in Spaces Rail** — Scheduled Phase 2
- [ ] **Infra-602: Offline Retry Queue for Dirty Records** — Scheduled Phase 2
- [ ] **Infra-603: Admin Runbook (`docs/ADMIN_RUNBOOK.md`)** — Scheduled Phase 5
- [x] **Infra-604: `.env.example` for Azure App Registration** — **CREATED this session**
- [x] **Infra-605: AI Diagnostic Export System** — **CREATED this session** (`src/common/diagnostic-exporter.ts`)


---

### 2. Protocol & Synchronization Edge Cases
- [ ] **Fix-101: Microsoft Graph API 429 Rate-Limiting & Jitter**
  - *Issue*: Rapid delta polling across multiple delegated mailboxes can trigger HTTP 429 (`Too Many Requests`) with `Retry-After` header.
  - *Fix*: Implement exponential backoff with randomized jitter and centralized request queuing in `src/api/graph/client.ts`.
  - *Status*: **Scheduled in Phase 2**.

- [ ] **Fix-102: CalDAV `sync-collection` Missing Support on Legacy Servers**
  - *Issue*: Older on-premise CalDAV servers may not implement RFC 6578 (`sync-collection`) and fail when receiving sync-token PROPFIND requests.
  - *Fix*: Implement fallback to `getetag` / `getctag` change detection when server does not advertise `calendar-query` / `sync-collection` support in `DAV:` header.
  - *Status*: **Scheduled in Phase 3**.

---

### 3. Executive Delegation & Privacy Edge Cases
- [ ] **Fix-201: Outlook "Private" Calendar Event Masking**
  - *Issue*: When an executive assistant queries a boss's calendar, items marked `sensitivity: "private"` in Outlook must not expose body, attendees, or subject to delegates without explicit delegate private item permissions.
  - *Fix*: In `src/background/delegation-manager.ts`, sanitize event payloads for delegated profiles: display "Private Appointment" and obscure description unless `canViewPrivateItems` scope is explicitly granted.
  - *Status*: **Scheduled in Phase 4**.

- [ ] **Fix-202: "Send on Behalf Of" vs "Send As" Header Compliance**
  - *Issue*: Exchange / M365 rejects messages where `From` is modified unless the user has "Send As", whereas "Send on Behalf" requires keeping the secretary's address in `Sender` and the boss's address in `From`.
  - *Fix*: Automate RFC 5322 header generation based on discovered delegation rights (`From: boss@gov.sc`, `Sender: secretary@gov.sc`).
  - *Status*: **Scheduled in Phase 4**.

---

### 4. ISO/IEC 27001 & Seychelles DPA 2023 Compliance Fixes
- [x] **Comp-301: Zero Cleartext Password Retention**
  - *Requirement*: ISO 27001 Control A.8.24 forbids storing unencrypted passwords in plaintext in extension storage.
  - *Fix*: Enforce OAuth2 / PKCE for M365 and AES-GCM 256-bit encrypted credential envelopes for legacy basic auth.
  - *Status*: **Enforced in `src/common/crypto-vault.ts`**.

- [ ] **Comp-302: Automated Local Data Erasure ("Right to Erasure")**
  - *Requirement*: Seychelles Data Protection Act 2023 requires user-accessible data purging.
  - *Fix*: Implement a one-click "Purge Local Profile Cache" button in `<settings-modal>` that truncates all IndexedDB tables and wipes encryption keys.
  - *Status*: **Scheduled in Phase 2**.

---

### 5. RAII & Resource Management Guardrails
- [x] **RAII-401: Event Listener Leak Prevention in Web Components**
  - *Issue*: Dynamic Web Components attached to the DOM accumulate event listeners if disconnected without explicit removeEventListener calls.
  - *Fix*: Use TypeScript 5.5+ `using` with `EventSubscriptionScope` to bind listener cleanup directly to DOM disconnect lifecycles.
  - *Status*: **Implemented in `src/common/raii.ts`**.
