# ATO Executive Summary

**Authentication and Identity Management Assessment**
**Oracle IAM Identity Domains — OCI DoD Realm (IL4 / IL5)**

---

**System:** Oracle Cloud Infrastructure (OCI) DoD Realm
**Service Evaluated:** Oracle IAM Identity Domains
**Impact Levels:** IL4 (CUI), IL5 (CUI / NSS)
**Assessment Date:** January 2026
**Purpose:** Determine suitability of Oracle IAM Identity Domains to satisfy DoD authentication, MFA, and federation requirements for Authority to Operate (ATO)

---

## 1. Assessment Objective

This assessment evaluates whether **Oracle IAM Identity Domains**, when deployed within an **OCI DoD Realm tenancy**, can satisfy Department of Defense (DoD) authentication, multi-factor authentication (MFA), federation, and session management requirements for web applications operating at **IL4 and IL5**, in accordance with:

- DoDI 8520.03 (Identification and Authentication)
- DISA Cloud Computing SRG v1r5
- NIST SP 800-63-4 (AAL/FAL)
- NIST SP 800-53 Rev. 5 (IA, AC, AU families)
- DoD CIO MFA Policy (2024)

---

## 2. Authorization Boundary and Inheritance

Oracle IAM Identity Domains operates **within the OCI DoD Realm authorization boundary**, which holds:

- **FedRAMP High JAB P-ATO**
- **DISA Provisional Authorization at IL4, IL5, and IL6**

**Inherited from Oracle (CSP):**
- Infrastructure security controls
- Cryptographic protections
- Platform availability and integrity
- Native audit logging via OCI Audit Service

**Customer Responsibility:**
- Authentication policy configuration
- MFA enforcement and factor selection
- Federation trust establishment
- Session timeout enforcement
- Audit log forwarding and retention
- User lifecycle management

---

## 3. Authentication Architecture

### Primary Mechanism: Federated CAC/PIV Authentication

- Users authenticate using **DoD PKI (CAC/PIV)** at a DoD-managed IdP
- Oracle IAM Identity Domains consumes **SAML 2.0 assertions**
- Certificate validation, revocation checking (OCSP/CRL), and identity proofing are performed **outside OCI** by the authoritative DoD IdP
- CAC/PIV authentication satisfies MFA requirements through **possession (card)** and **knowledge (PIN)**

This model aligns with DoDI 8520.03 and DISA SRG requirements.

---

## 4. MFA and Assurance Level Compliance

| Area | Determination |
|------|---------------|
| **Minimum AAL (IL4)** | AAL2 — Met |
| **Minimum AAL (IL5)** | AAL2 (AAL3 for privileged/NSS) — Met |
| **Privileged Access** | Phishing-resistant MFA or CAC/PIV required |
| **Non-PKI MFA (Exception-based)** | FIDO2 hardware authenticators only |
| **Prohibited Factors** | SMS, email OTP, voice OTP — Disabled |

**Password-only authentication is disabled for all IL4/IL5 access.**

---

## 5. Federation and Session Security

| Control | Configuration |
|---------|---------------|
| Federation Assurance Level | FAL2 minimum (FAL3 for NSS at IL5) |
| Assertion Security | Signed and encrypted |
| Assertion Lifetime | ≤5 minutes |
| AuthnContext Enforcement | Certificate-based authentication required |
| Replay Protection | Single-use assertions, short-lived tokens |

**Session Controls (DISA-Compliant):**

| Parameter | IL4 | IL5 |
|-----------|-----|-----|
| Idle timeout | ≤15 minutes | ≤15 minutes |
| Absolute session duration | ≤12 hours | ≤8 hours |
| Privileged reauthentication | Required | Required |
| Remember device / trusted devices | Disabled | Disabled |

---

## 6. Auditing and Monitoring

Authentication and MFA events are automatically captured by **OCI Audit**, including:

- Authentication successes and failures
- MFA challenges and validation
- Session creation and termination
- Privilege and policy changes

Audit logs are forwarded to a **DoD-approved SIEM** via OCI Logging and Service Connector Hub to meet:

- AU-2, AU-3, AU-6, AU-12 requirements
- One-year online retention with long-term archival

---

## 7. ATO Determination

### Finding

**Oracle IAM Identity Domains is ATO-supportable for IL4 and IL5 DoD workloads** when configured as documented and operated within an OCI DoD Realm tenancy.

### Conditions for Approval

| # | Condition | Status |
|---|-----------|--------|
| 1 | Federation with DoD-approved IdP performing CAC/PIV authentication | Required |
| 2 | Phishing-resistant MFA enforced for all privileged access | Required |
| 3 | Non-compliant MFA factors disabled (SMS, email OTP) | Required |
| 4 | Session timeouts meeting DISA SRG thresholds | Required |
| 5 | Authentication audit logs forwarded to approved SIEM | Required |
| 6 | Password-only authentication disabled | Required |

When these conditions are met, Oracle IAM Identity Domains fully satisfies DoD authentication, MFA, federation, and auditing requirements for ATO at IL4 and IL5.

---

**Prepared for inclusion in System Security Plan (SSP) and ATO Review Package**

---

## Related Documents

- [DoD Authentication Standards](authentication-standards.md)
- [OCI IAM Assessment](oci-iam-assessment.md)
- [Implementation Guide](implementation-guide.md)
- [Control Mappings and Appendices](appendices.md)
