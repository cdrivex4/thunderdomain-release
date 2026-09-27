# Third-Party Notices & Software Bill of Materials (SBOM)
## Open Source Licensing and Provenance Record

**Project:** Thunderbird Outlook Parity & Productivity Suite  
**Organization:** Department of Information Communications Technology (DICT), Seychelles  
**Compliance Standard:** ISO/IEC 5230:2020 (OpenChain Open Source License Compliance) / ISO 27001 Control A.8.30  

---

## 1. Overview & Provenance

This project incorporates third-party open-source software libraries. All dependencies have been audited for security vulnerabilities (`npm audit`), licensing compatibility (MIT, Apache-2.0, BSD-3-Clause), and supply-chain integrity.

---

## 2. Dependency Inventory & License Matrix

| Package Name | Version Target | License | Purpose / Architectural Role |
| :--- | :--- | :--- | :--- |
| **dexie** | `^4.0.0` | Apache-2.0 | High-performance IndexedDB wrapper for local-first storage. |
| **typescript** | `^5.5.0` | Apache-2.0 | TypeScript compiler supporting strict mode & explicit resource management (`using`). |
| **vite** | `^5.4.0` | MIT | Deterministic bundling & module resolution. |
| **vitest** | `^2.1.0` | MIT | Next-generation fast unit & component testing engine. |
| **jsdom** | `^25.0.0` | MIT | DOM environment simulation for Web Component interaction tests in CI. |
| **msw** | `^2.4.0` | MIT | Mock Service Worker for network protocol and API simulation. |
| **playwright** | `^1.47.0` | Apache-2.0 | Cross-browser and extension end-to-end automation. |

---

## 3. License Texts

### Apache License 2.0
Licensed under the Apache License, Version 2.0 (the "License"); you may not use these files except in compliance with the License. You may obtain a copy of the License at:
http://www.apache.org/licenses/LICENSE-2.0

### MIT License
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
