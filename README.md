# 🏛️ DPI Trust — Universal Verifiable Credential Trust Framework & Registry

> **Part of the MOSIP Asia Digital Public Infrastructure (DPI) Suite**  
> **Agency B:** National Trust Authority (`*.trust.<domain>`)  
> **Lead Architect:** Sila (`@silanm`)  
> **Target Delivery:** Activity 1.5 under the Bill & Melinda Gates Foundation Ecosystem Grant

---

## 📌 Executive Summary

**DPI Trust** provides a modular, cloud-native **Verifiable Credential Trust Framework and Verifiable Data Registry (VDR)** for national digital identity ecosystems. It anchors decentralized trust, resolves cryptographic identities (`did:web`), hosts machine-readable JSON-LD schemas, manages cryptographic revocation lists (W3C `StatusList2021`), and serves the accredited **Trusted Issuers List (TIL/TIR)** API for verifiers.

Built on open W3C and OpenID Foundation standards, DPI Trust decouples the **universal trust infrastructure** (hosting, DID resolution, byte array revocation) from **country-specific governance taxonomies** (schemas, accredited authorities, regulatory policies).

---

## 🌐 Pluggable Country Trust Profile Architecture

A core design principle of DPI Trust is the strict **separation of governance from infrastructure**:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        DPI TRUST ARCHITECTURAL SEPARATION                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                        │
│   ⚙️ UNIVERSAL TRUST INFRASTRUCTURE ENGINE (core/ & helm/)                              │
│   • In-Cluster VDR Daemon (Serving did:web, did.json, JSON-LD context caches)          │
│   • Trusted Issuers List (TIL/TIR) REST API (/api/v1/issuers)                          │
│   • W3C StatusList2021 Bitstring Revocation Engine & Caching Daemon                    │
│   • Lightweight Trust Governance Web Console (console.trust.<domain>)                  │
│                                                                                        │
│                                  ▲                                                     │
│                                  │ (Loaded via values.yaml: profile: "thailand")       │
│                                                                                        │
│   📁 PLUGGABLE COUNTRY GOVERNANCE PROFILES (profiles/)                                 │
│   ┌───────────────────────────────────┬───────────────────────────────────┐            │
│   │ 🇹🇭 profiles/thailand/             │ 🌐 profiles/generic/              │            │
│   │ • ETDA / DGA JSON-LD Schemas      │ • Clean W3C Default Baseline      │            │
│   │ • Student PID & Developer Passport│ • Self-contained Reference Schemas│            │
│   │ • National Trust Anchors          │ • Open Testbed Profiles           │            │
│   └───────────────────────────────────┴───────────────────────────────────┘            │
│                                                                                        │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🎯 4-Month Strategic Focus: The Thailand National Model (Activity 1.5)

For the active **4-month development window (September 2026 – January 2027)** under the Gates Foundation Grant, our primary engineering focus is **100% committed to the Thailand National Model**:

* **Regulatory Anchor:** Electronic Transactions Development Agency (**ETDA**) Recommendation on Verifiable Credentials & Digital Government Development Agency (**DGA**) standards.
* **Flagship Profile:** [`profiles/thailand/`](profiles/thailand/)
* **Core Schemas:**
  1. `NationalStudentPID.jsonld` — University citizen identity proofing.
  2. `DeveloperPassport.jsonld` — Virtual hackathon team credential holding OIDC API tokens and namespace permissions.
  3. `HackathonAchievement.jsonld` — Tamper-evident cryptographic award certificate.
* **Revocation Standard:** W3C `StatusList2021` bitstring byte array anchored on `vdr.trust.<domain>`.

In subsequent phases (Years 2+), the pluggable profile architecture allows seamless regional adaptation (e.g., `profiles/philippines/` for PhilSys/DICT, or `profiles/generic/` for international standard testbeds) without changing the core engine.

---

## 🏛️ Sovereign Agency B Domain Topology

DPI Trust parameterizes all endpoints around the root domain (`${var.sandbox_domain}`, e.g., `dpi.ait.ac.th`):

| Endpoint | Ingress FQDN | Protocol / Path | Purpose & Target Consumers |
|---|---|---|---|
| **VDR DID Document** | `https://vdr.trust.<domain>/.well-known/did.json` | JSON (`did:web`) | Public keys (`assertionMethod`) used by Inji Certify and Inji Verify |
| **Schema Registry** | `https://vdr.trust.<domain>/schemas/<profile>/` | W3C JSON-LD Context | Machine-readable credential taxonomies resolved by wallets and verifiers |
| **Trusted Issuers List** | `https://registry.trust.<domain>/api/v1/issuers` | REST (TIR/TIL API) | Whitelist of accredited government and academic issuers |
| **Trust Admin Console** | `https://console.trust.<domain>/` | Web Admin UI | Browser console for ecosystem administrators to inspect schemas and trust anchors |

---

## 📦 OCI Helm Chart & GHCR Distribution

Track 3 packages its entire runtime into a hermetic, self-contained OCI Helm chart:

```bash
# Pull the verified OCI chart directly from GitHub Container Registry
helm pull oci://ghcr.io/mosip-asia/charts/ait-trust-framework --version 0.1.0
```

### Deployment Configuration (`values.yaml`)

```yaml
global:
  domain: "dpi.ait.ac.th"

trustFramework:
  profile: "thailand"        # Selects profiles/thailand/ governance pack

vdr:
  host: "vdr.trust.dpi.ait.ac.th"
  cacheDuration: "1h"

registry:
  host: "registry.trust.dpi.ait.ac.th"
  authRequired: false        # Publicly queryable by verifiers

console:
  host: "console.trust.dpi.ait.ac.th"
```

---

## 🗺️ Milestone Roadmap & Active Sprint Deliverables

All Track 3 engineering tasks are tracked on [**Project #14: dpi-sandbox**](https://github.com/orgs/mosip-asia/projects/14):

| Milestone | Target ETA | Issue | Title | Status | Priority |
|---|:---:|:---:|:---|:---:|:---:|
| **Milestone 1.5.1** | **30 Sep 2026** | **[#1](https://github.com/mosip-asia/dpi-trust/issues/1)** | Package Thailand Trust Framework (VDR `did:web` & TIL) as OCI Helm Chart v0.1.0 | `Ready` | `P0 - Blocker` |
| **Milestone 1.5.2** | **31 Oct 2026** | **[#2](https://github.com/mosip-asia/dpi-trust/issues/2)** | VCGA Trusted Issuers List (`registry.trust`) & Trust Admin Console (`console.trust`) | `Backlog` | `P0 - Blocker` |
| **Milestone 1.5.3** | **31 Dec 2026** | **[#3](https://github.com/mosip-asia/dpi-trust/issues/3)** | ETDA & DGA Standards Alignment & Verifier Handbook Documentation | `Backlog` | `P0 - Blocker` |
| **Milestone 1.5.4** | **30 Jan 2027** | **[dpi-sandbox#19](https://github.com/mosip-asia/dpi-sandbox/issues/19)** | Cryptographic Audit Logs & End-to-End VC/VP Interoperability Verification | `Backlog` | `P0 - Blocker` |

---

## 🛠️ Local Verification & Smoke Testing

To verify the local VDR and schema endpoints, run the bundled verification test:

```bash
# Verify all endpoints against local cluster or remote domain
./helm/ait-trust-framework/smoke.sh dpi.ait.ac.th
```

A successful smoke test confirms:
1. `/.well-known/did.json` returns HTTP 200 with valid Ed25519 public keys.
2. `/schemas/thailand/student-pid.jsonld` returns HTTP 200 with valid JSON-LD `@context`.
3. `/api/v1/issuers` returns HTTP 200 with accredited trust anchors.

---

## 📜 Standards Compliance

* **W3C:** [Decentralized Identifiers (`did:web`) v1.0](https://w3c-ccg.github.io/did-method-web/)
* **W3C:** [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/)
* **W3C:** [StatusList2021 Revocation](https://w3c-ccg.github.io/vc-status-list-2021/)
* **ETDA / DGA:** Thailand Recommendation on Electronic Verifiable Credentials & Digital Identity
* **OpenID Foundation:** OpenID for Verifiable Credential Issuance (OID4VCI) & Verifiable Presentations (OID4VP)

---

## 📄 License

Licensed under the **Apache License 2.0** — see the [LICENSE](LICENSE) file for details.
