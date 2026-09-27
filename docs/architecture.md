# Architecture Design Document
## Thunderbird Outlook Parity & Productivity Suite

---

## 1. Executive Technical Overview

The Thunderbird Outlook Parity Suite is designed as a hybrid MailExtension and WebExtension Experiment for Mozilla Thunderbird 128+ ESR (Nebula and Supernova UI). It bridges the operational gaps between Microsoft Outlook and Thunderbird for the Government of Seychelles (`ad.gov.sc`) infrastructure.

```
+----------------------------------------------------------------------------------------------------+
|                                      Thunderbird 128+ UI Host                                      |
|                                                                                                    |
|  +----------------------------------------------------------------------------------------------+  |
|  |                             <spaces-rail> Web Component                                      |  |
|  |     [ Mail ]       [ Calendar ]       [ Tasks / To Do ]       [ Notes ]       [ Secretary ]  |  |
|  +----------------------------------------------------------------------------------------------+  |
|  |                                                                                              |  |
|  |   Active Workspace Views:                                                                    |  |
|  |   - <onboarding-wizard>: Zero-config ad.gov.sc autodiscovery & modern authentication         |  |
|  |   - <task-board>: Microsoft To Do & CalDAV VTODO task management with subtasks & categories   |  |
|  |   - <notes-board>: Rich-text OneNote & IMAP sticky notes board                                |  |
|  |   - <scheduling-assistant>: Free/Busy attendee availability grid                              |  |
|  |   - <delegation-modal>: Secretary/Boss multi-profile manager with "Send on Behalf"           |  |
|  |                                                                                              |  |
|  +----------------------------------------------------------------------------------------------+  |
+--------------------------------------------------+-------------------------------------------------+
                                                   │
                     ┌─────────────────────────────┴─────────────────────────────┐
                     ▼                                                           ▼
+──────────────────────────────────────────────────+  +──────────────────────────────────────────────+
|          Standard MailExtension APIs             |  |        WebExtension Experiment Bridge        |
|  • messenger.accounts (read/watch accounts)      |  |  • spacesBridge: Deep native window hooks    |
|  • messenger.messages (read/tag messages)        |  |  • calendarBridge: Lightning store hook      |
|  • messenger.tabs / messenger.windows            |  |  • addressBookBridge: Global Address List    |
+──────────────────────────────────────────────────+  +──────────────────────────────────────────────+
                     │                                                           │
                     └─────────────────────────────┬─────────────────────────────┘
                                                   ▼
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                                    Core RAII Sync & Storage Engine                                 |
|  • ScopeGuards (Symbol.dispose / Symbol.asyncDispose) for deterministic listener & memory cleanup  |
|  • IndexedDB Offline-First Transactional Store (Dexie.js)                                          |
|  • Web Crypto AES-GCM 256-bit Secure Token Vault                                                    |
|  • Delta Change Engine (@odata.deltaLink & CalDAV sync-token tracking)                             |
+──────────────────────────────────────────────────+─────────────────────────────────────────────────+
                                                   │
                     ┌─────────────────────────────┴─────────────────────────────┐
                     ▼                                                           ▼
+──────────────────────────────────────────────────+  +──────────────────────────────────────────────+
|              Microsoft 365 Graph v1.0            |  |             Open Standards Backend           |
|  • OAuth2 / MSAL PKCE Authentication             |  |  • RFC 4791 CalDAV (Calendar & Tasks)       |
|  • Events, To-Do Lists, OneNote, Categories      |  |  • RFC 6352 CardDAV (Address Books)          |
|  • Delegated Access (/users/{boss_id}/...)       |  |  • RFC 6186 DNS Autodiscovery                |
+──────────────────────────────────────────────────+  +──────────────────────────────────────────────+
```

---

## 2. Component Design & Interdependency

### 2.1 The Web Components Layer
All UI views are built with **Vanilla TypeScript Web Components** extending `HTMLElement` and registered with `customElements.define()`.
- **CSS Architecture**: No external CSS libraries are imported at runtime. The components consume Thunderbird Supernova CSS tokens directly (`var(--color-text-primary)`, `var(--color-background-secondary)`, `var(--color-accent)`), guaranteeing 100% theme fidelity across custom user themes and native Dark/Light mode switches.
- **Resource Lifecycle**: Every component implements the `Disposable` interface via TypeScript 5.5+ `[Symbol.dispose]()`. When a component is disconnected from the DOM (`disconnectedCallback`), all active event listeners, resize observers, and background abort signals are deterministically disposed.

### 2.2 Delta Synchronization Engine
1. **Initial Full Sync**: On account creation, the engine issues an initial query to Graph API `/v1.0/me/events/delta` or CalDAV `PROPFIND`.
2. **Delta Token Storage**: The resulting `@odata.deltaLink` or `sync-token` is persisted in the `deltaTokens` IndexedDB table.
3. **Incremental Polling & Push**: Subsequent sync operations query the delta link directly, fetching only added, modified, or deleted items in sub-second roundtrips.

---

## 3. Executive Delegation Model ("Secretary" / Boss Profiles)

The extension allows executive assistants to manage multiple boss profiles simultaneously within a single Thunderbird application:
1. **Delegation Discovery**: The extension queries `/v1.0/users/{boss_email}/calendars` and `/mailFolders` using delegated OAuth scopes (`Mail.ReadWrite.Shared`, `Calendars.ReadWrite.Shared`).
2. **Calendar Overlay**: Boss calendars are mounted as secondary color-coded layers on the assistant's calendar view.
3. **MIME Construction**: In outgoing emails, the extension structures RFC 5322 headers:
   ```text
   From: Minister Name <minister.office@ad.gov.sc>
   Sender: Executive Assistant <jane.doe@ad.gov.sc>
   Reply-To: minister.office@ad.gov.sc
   ```
4. **Privacy Shield**: Items flagged `sensitivity: "private"` are masked in the UI unless explicit private item clearance is granted.
