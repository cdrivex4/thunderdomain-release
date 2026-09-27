# Government of Seychelles DICT Security Baseline
## Infrastructure & `ad.gov.sc` Active Directory Integration Guidelines

**Authority:** Department of Information Communications Technology (DICT), Republic of Seychelles  
**Domain Environment:** `ad.gov.sc` / `gov.sc`  
**Classification:** GOVERNMENT RESTRICTED  

---

## 1. Executive Context & Infrastructure Topology

The Department of Information Communications Technology (DICT) provides centralized ICT infrastructure, identity management, and messaging services for Government of Seychelles Ministries, Departments, and Agencies (MDAs).

The identity and messaging ecosystem consists of:
1. **Active Directory Domain Services**: Primary domain `ad.gov.sc`.
2. **Messaging & Groupware**: Hybrid deployment of Microsoft Exchange / M365 Exchange Online alongside sovereign government mail gateways.
3. **Endpoint Operating Systems**: Standardized enterprise desktop fleet consisting of **Windows 11 Enterprise** and **Ubuntu 24.04 LTS (Noble Numbat)** workstations.

---

## 2. Onboarding & Auto-Configuration Protocol (`ad.gov.sc`)

To provide a seamless, zero-friction setup for government personnel without compromising security:

```
[ User Input: user@ad.gov.sc ]
               │
               ▼
   [ DNS SRV / MX Query ] ──────────────────────────┐
               │                                    │
               ▼                                    ▼
 [ Probe M365 Autodiscover ]             [ Probe On-Premise SRV ]
 (autodiscover.outlook.com)               (_autodiscover._tcp.ad.gov.sc)
               │                                    │
               ▼                                    ▼
 [ OAuth 2.0 PKCE Authorization ]        [ Exchange / CalDAV Auth ]
 (Entra ID / DICT SSO Portal)            (TLS 1.3 Negotiated Endpoint)
               │                                    │
               └──────────────────┬─────────────────┘
                                  │
                                  ▼
                [ Auto-Mount Thunderbird Spaces ]
                - Inbox & Shared Mailboxes
                - Sovereign & Departmental Calendars
                - Government Global Address List (GAL)
                - Microsoft To Do / CalDAV Tasks
                - OneNote / Government Notes Board
```

---

## 3. Workstation Security Requirements

1. **Host Integrity**: The extension must execute within the sandboxed MailExtension environment without requiring root / administrator escalation on workstation machines.
2. **Zero Insecure Protocols**: Legacy unencrypted protocols (POP3/IMAP port 110/143 without STARTTLS, unencrypted HTTP, TLS < 1.2) are permanently disabled in code.
3. **Multi-User Isolation**: On shared government workstations, each Thunderbird user profile maintains a completely isolated cryptographic vault and local database.

---

## 4. Multi-Profile & Executive Delegation Rules

Executive secretaries and administrative officers managing senior government official profiles must adhere to:
1. **Author-Only Default**: New delegated accounts default to "Author" permissions (can read and draft, but cannot permanently delete or alter security policies).
2. **Audit Logging**: All communications sent on behalf of a Minister, Principal Secretary, or Director are logged with cryptographic non-repudiation timestamps.
3. **Separation of Duties**: Delegated access does not grant access to the principal's personal password or cryptographic master key.
