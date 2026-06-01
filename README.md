# 🔒 Enterprise Zero Trust Architecture (ZTA)
### Identity-Aware Infrastructure & Context-Enforced Perimeter

[![Security Paradigm](https://img.shields.io/badge/Paradigm-Never%20Trust%2C%20Always%20Verify-FF5733?style=for-the-badge)](https://www.nist.gov/publications/zero-trust-architecture)
[![Architecture Standard](https://img.shields.io/badge/Standard-NIST%20SP%20800--207-0052CC?style=for-the-badge)]()

A production-grade implementation of a Zero Trust Architecture (ZTA) based on the core paradigm: **"Never Trust, Always Verify."** This deployment completely eliminates the legacy notion of a "trusted internal network" by hiding critical enterprise infrastructure behind an Identity-Aware Proxy (IAP) backed by modern cryptographic authentication layers.

---

## 🛠️ Security Engineering Stack

The defensive components and orchestrators used to enforce strict identity-based boundaries:

### ⚙️ Identity Proxy & Container Orchestration
| Component | Technology | Role & Enforcement |
| :--- | :--- | :--- |
| **Data Plane Proxy** | ![Pomerium](https://img.shields.io/badge/Pomerium-6C5CE7?style=flat-square&logo=pomerium&logoColor=white) | Enforces context-aware authorization policies, route validation, and token signing. |
| **Containerization** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) | Isolates core services within cryptographically secured, non-default bridge networks. |

### 🔑 Federation & Cryptographic Authorization Layer
* ![OIDC](https://img.shields.io/badge/OIDC-000000?style=flat-square&logo=openid&logoColor=white) **OpenID Connect (OIDC):** Handles centralized identity verification with upstream Identity Providers (IdP) supporting MFA.
* ![OAuth2](https://img.shields.io/badge/OAuth2-3C3C3D?style=flat-square&logo=jsonwebtokens&logoColor=white) **OAuth 2.0 / JWT:** Signs and passes user context assertions securely down to downstream microservices.

---

## 📊 Infrastructure Hiding & Workflow Design

The architectural topology below illustrates how backend microservices are decoupled from the open internet, enforcing strict context-aware perimeter verification:

```text
🌎 OPEN INTERNET / UNTRUSTED CLIENTS
──────────────────────────────────────────────────────────────────────
                  │
                  ▼   [ Public HTTPS: Port 443 Only ]
     ┌────────────────────────┐
     │  Identity-Aware Proxy  │ ◄───► [ Central Identity Provider (IdP) ]
     │      (Pomerium)        │       Authenticates User via OIDC + MFA
     └────────────────────────┘
                  │
                  ├── (Identity Checks Fail?) ──► ❌ 403 Forbidden / Drop Connection
                  │
                  ▼ [ Valid JWT Token Granted ]
       🔒 Isolated Internal Bridge (pomerium-net)
                  │
                  ▼ [ Internal Expose Only: Port 5678 ]
     ┌────────────────────────┐
     │ Internal Target Core   │
     │ (Isolated Microservice)│
     └────────────────────────┘
