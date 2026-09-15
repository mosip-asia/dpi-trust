# 🇹🇭 Thailand National Trust Profile (`profiles/thailand`)

> **Primary National Governance Pack for Activity 1.5**  
> Aligned with Electronic Transactions Development Agency (**ETDA**) and Digital Government Development Agency (**DGA**) standards.

---

## 🏛️ Governance Components

This profile provides the official national taxonomies, DID parameters, and seed issuer accreditations for Thailand's digital ecosystem:

### 1. JSON-LD Schemas (`schemas/`)
* **`student-pid.jsonld`**: National Student Personal Identification Data (PID) credential issued by accredited universities.
* **`developer-passport.jsonld`**: Closed-Loop Virtual Hackathon Developer Passport VC holding dynamic team credentials, OIDC scopes, and namespace authorization.
* **`hackathon-achievement.jsonld`**: Cryptographic W3C Achievement & Award credential issued by the VCGA.

### 2. Root DID Document (`did.json`)
* Anchors `did:web:vdr.trust.<domain>` with Ed25519 public keys used for ecosystem-wide verification.

### 3. Seed Trusted Issuers (`seed-issuers.json`)
* Whitelist of accredited government and academic issuers recognized by the VCGA:
  * `did:web:issuer.egov.<domain>` (e-Government Agency / Inji Certify)
  * `did:web:ait.ac.th` (Asian Institute of Technology)
