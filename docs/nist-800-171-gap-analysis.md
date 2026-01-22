# NIST SP 800-171 Rev 3 Gap Analysis — OCI IAM Identity Domains

**Applicable Environment:** OCI DoD Realm (IL4/IL5) and Nonfederal CUI Systems
**Purpose:** CUI Protection Compliance Assessment, CMMC Preparation, ATO Review Package
**Last Updated:** January 2026

---

<a id="1-overview"></a>

## 1. Overview

### 1.1 Document Purpose

This document provides a gap analysis of Oracle IAM Identity Domains against NIST SP 800-171 Revision 3 security requirements. NIST 800-171 establishes security requirements for protecting the confidentiality of Controlled Unclassified Information (CUI) in nonfederal systems and organizations.

### 1.2 NIST 800-171 Rev 3 Summary

| Attribute | Value |
|-----------|-------|
| Publication Date | May 14, 2024 |
| Total Security Requirements | 97 (reduced from 110 in Rev 2) |
| Security Requirement Families | 17 |
| Organization-Defined Parameters (ODPs) | 49 |
| DoD Contract Applicability | Required for DFARS 252.204-7012 compliance |
| CMMC Alignment | Foundation for CMMC Level 2 certification |

**Reference:** [NIST SP 800-171 Rev 3](https://csrc.nist.gov/pubs/sp/800/171/r3/final)

### 1.3 Relationship to Existing Documentation

NIST 800-171 requirements derive from NIST SP 800-53. This documentation builds upon the existing [NIST SP 800-53 Control Mapping](appendices.md#appendix-a-nist-sp-800-53-rev-5-control-mapping) and provides specific guidance for CUI protection scenarios.

---

<a id="2-applicability"></a>

## 2. Applicability

### 2.1 When NIST 800-171 Applies

NIST SP 800-171 applies to:

- Defense contractors handling CUI under DFARS 252.204-7012
- Organizations seeking CMMC Level 2 certification
- Nonfederal systems processing, storing, or transmitting CUI
- Subcontractors in the defense industrial base (DIB)

### 2.2 Relationship to DoD Impact Levels

| Impact Level | NIST 800-171 Applicability | Notes |
|--------------|---------------------------|-------|
| IL4 (CUI) | **Required** | Primary CUI protection standard |
| IL5 (CUI/NSS) | **Required + Additional** | 800-171 baseline plus NSS-specific controls |
| IL6 (Classified) | Not Applicable | Classified data governed by separate requirements |

### 2.3 OCI IAM Identity Domains Applicability

OCI IAM Identity Domains serves as the identity and access management component for CUI systems deployed in OCI. The controls addressed in this analysis focus on:

- Access Control (03.01)
- Audit and Accountability (03.03)
- Identification and Authentication (03.05)
- System and Communications Protection (03.13)

---

<a id="3-dod-organization-defined-parameters"></a>

## 3. DoD Organization-Defined Parameters (ODPs)

NIST 800-171 Rev 3 includes 49 ODPs that allow organizations to tailor security requirements. The DoD has published definitive values for all ODPs applicable to defense contractors.

**Reference:** [DoD ODP Values for NIST SP 800-171 Rev 3](https://dodcio.defense.gov/Portals/0/Documents/CMMC/OrgDefinedParmsNISTSP800-171.pdf)

### 3.1 IAM-Relevant DoD ODP Values

| Control | Parameter | DoD Value | OCI IAM Configuration |
|---------|-----------|-----------|----------------------|
| 03.01.01 | Inactive account disable period | **90 days** | SCIM deprovisioning sync |
| 03.01.10 | Device lock inactivity timeout | **≤15 minutes** | Session settings → Idle timeout |
| 03.01.01 | Session logout threshold | **≤24 hours** | Session settings → Session duration |
| 03.05.05 | Identifier reuse prevention period | **10 years** | User identifier management |
| 03.05.09 | Minimum password length | **16 characters** | Password policy configuration |
| 03.13.11 | Cryptographic protection | **FIPS Validated** | Inherited from OCI platform |

For complete ODP values, see [Appendix H: NIST 800-171 DoD ODP Reference](#appendix-h-nist-800-171-dod-odp-reference).

---

<a id="4-gap-analysis-access-control"></a>

## 4. Gap Analysis: Access Control (03.01)

### 4.1 Control Summary

| Control | Requirement | OCI IAM Capability | Gap Status |
|---------|-------------|-------------------|------------|
| 03.01.01 | System Account Management | User lifecycle, SCIM provisioning | **Supported** |
| 03.01.02 | Access Enforcement | IAM policies, group membership | **Supported** |
| 03.01.03 | Information Flow Enforcement | Network policies, compartments | **Partial — Customer Config** |
| 03.01.04 | Separation of Duties | Role-based access, admin separation | **Supported** |
| 03.01.05 | Least Privilege | Granular IAM policies | **Supported** |
| 03.01.07 | Privileged Functions | Admin group controls, audit logging | **Supported** |
| 03.01.10 | Device Lock | Session idle timeout | **Supported** |
| 03.01.12 | Remote Access | Federation, VPN integration | **Supported** |
| 03.01.16 | Wireless Access | Network-level control | **Out of Scope** |
| 03.01.18 | Mobile Devices | Conditional access policies | **Partial — Customer Config** |
| 03.01.20 | External Systems | Federation trust policies | **Supported** |
| 03.01.22 | Publicly Accessible Content | Access controls | **Customer Responsibility** |

### 4.2 Detailed Control Analysis

<a id="030101-system-account-management"></a>

#### 03.01.01 System Account Management

**Requirement:** Define account types, manage lifecycle, disable inactive accounts within 90 days, require logout after 24 hours of inactivity.

**OCI IAM Capability:**
- User account creation, modification, disablement, deletion
- Group and role membership management
- SCIM provisioning for automated lifecycle sync
- JIT provisioning via federation
- Session duration limits (configurable)

**Gap Assessment:** **No Gap** — Full capability available

**Configuration Required:**
- Configure SCIM connector to authoritative HR/identity source
- Set session duration ≤24 hours (DoD ODP requirement)
- Implement automated inactive account review process
- See [Account Provisioning](implementation-guide.md#42-account-provisioning)

<a id="030110-device-lock"></a>

#### 03.01.10 Device Lock

**Requirement:** Initiate device lock after ≤15 minutes of inactivity.

**OCI IAM Capability:**
- Configurable idle session timeout
- Sign-on policy enforcement

**Gap Assessment:** **No Gap** — Full capability available

**Configuration Required:**
- Set idle timeout to 15 minutes or less
- See [Session Timeout Configuration](implementation-guide.md#51-session-timeout-configuration)

---

<a id="5-gap-analysis-audit-accountability"></a>

## 5. Gap Analysis: Audit and Accountability (03.03)

### 5.1 Control Summary

| Control | Requirement | OCI IAM Capability | Gap Status |
|---------|-------------|-------------------|------------|
| 03.03.01 | Event Logging | OCI Audit service | **Supported** |
| 03.03.02 | Audit Record Content | Comprehensive event schema | **Supported** |
| 03.03.03 | Audit Record Generation | Automatic capture | **Supported** |
| 03.03.04 | Audit Record Storage | Object Storage retention | **Supported** |
| 03.03.05 | Audit Review and Analysis | SIEM integration | **Customer Responsibility** |
| 03.03.06 | Audit Reduction and Reporting | SIEM/logging tools | **Customer Responsibility** |
| 03.03.08 | Protection of Audit Information | OCI Audit tamper protection | **Inherited** |

### 5.2 Detailed Control Analysis

<a id="030301-event-logging"></a>

#### 03.03.01 Event Logging

**Requirement:** Log specified event types including authentication, account management, privilege escalation.

**DoD ODP — Required Event Categories:**
1. Authentication attempts (success/failure)
2. File and object access
3. User account management
4. Privilege escalation
5. System events
6. Application initialization

**OCI IAM Capability:**
- Automatic capture of all authentication events
- MFA challenge and validation logging
- Session lifecycle events
- Account management events
- Policy and group membership changes

**Gap Assessment:** **No Gap** — All required IAM events captured

**Reference:** [Authentication Event Types](appendices.md#appendix-c-authentication-event-types)

<a id="030305-audit-review"></a>

#### 03.03.05 Audit Review, Analysis, and Reporting

**Requirement:** Correlate audit records, analyze for suspicious activity, report findings.

**OCI IAM Capability:**
- OCI Audit provides raw event data
- Service Connector Hub enables forwarding
- No native SIEM/analysis capability

**Gap Assessment:** **Customer Responsibility** — SIEM integration required

**Configuration Required:**
- Forward audit logs to DoD-approved SIEM
- Configure alerting rules for authentication anomalies
- Establish review procedures
- See [SIEM Integration Architecture](implementation-guide.md#62-siem-integration-architecture)

---

<a id="6-gap-analysis-identification-authentication"></a>

## 6. Gap Analysis: Identification and Authentication (03.05)

### 6.1 Control Summary

| Control | Requirement | OCI IAM Capability | Gap Status |
|---------|-------------|-------------------|------------|
| 03.05.01 | User Identification | Unique user identifiers | **Supported** |
| 03.05.02 | Device Identification | API keys, instance principals | **Supported** |
| 03.05.03 | Multi-factor Authentication | FIDO2, TOTP, federation MFA | **Supported** |
| 03.05.04 | Replay-Resistant Authentication | SAML assertions, token expiration | **Supported** |
| 03.05.05 | Identifier Management | 10-year identifier reuse prevention | **Partial — Customer Process** |
| 03.05.06 | Authenticator Management | MFA enrollment, credential lifecycle | **Supported** |
| 03.05.07 | Authenticator Feedback | Masked password entry | **Supported** |
| 03.05.08 | Cryptographic Module Authentication | FIPS-validated modules | **Inherited** |
| 03.05.09 | Password-Based Authentication | Password policy configuration | **Supported** |

### 6.2 Detailed Control Analysis

<a id="030503-multifactor-authentication"></a>

#### 03.05.03 Multi-factor Authentication

**Requirement:** Use MFA for local and network access to privileged accounts and network access to non-privileged accounts.

**OCI IAM Capability:**
- FIDO2/WebAuthn hardware authenticators (phishing-resistant)
- TOTP via mobile app or hardware token
- CAC/PIV via federation with DoD IdP
- Sign-on policies enforcing MFA by user group/context

**Gap Assessment:** **No Gap** — Full capability available

**Configuration Required:**
- Enable phishing-resistant MFA (FIDO2 or CAC/PIV)
- Disable non-compliant factors (SMS, email, voice)
- Configure sign-on policies per [MFA Configuration](implementation-guide.md#2-multi-factor-authentication-mfa)

**DoD Compliance Note:** SMS, email, and voice OTP are prohibited for CUI systems at IL4+.

<a id="030505-identifier-management"></a>

#### 03.05.05 Identifier Management

**Requirement:** Prevent identifier reuse for minimum 10 years (DoD ODP).

**OCI IAM Capability:**
- Unique user identifier (OCID) assigned at creation
- Identifier uniqueness enforced within domain
- No automatic identifier recycling

**Gap Assessment:** **Partial — Customer Process Required**

**Customer Action:**
- Document identifier management policy
- Maintain records of deleted/disabled user identifiers
- Implement process to verify identifier non-reuse
- Consider using enterprise GUID from authoritative source

<a id="030509-password-authentication"></a>

#### 03.05.09 Password-Based Authentication

**Requirement:** Minimum 16 characters (DoD ODP), prohibit passwords on compromised lists, maintain password history.

**OCI IAM Capability:**
- Configurable password policies
- Minimum length enforcement
- Password history (reuse prevention)
- Complexity requirements

**Gap Assessment:** **Supported** — Configuration required to meet DoD ODP values

**Configuration Required:**
- Set minimum password length to 16 characters
- Enable password history (minimum 24 generations recommended)
- Update compromised password list quarterly (customer responsibility)

**Note:** For DoD environments, password-only authentication is prohibited. Passwords serve as one factor in MFA scenarios.

---

<a id="7-gap-analysis-system-communications"></a>

## 7. Gap Analysis: System and Communications Protection (03.13)

### 7.1 Control Summary

| Control | Requirement | OCI IAM Capability | Gap Status |
|---------|-------------|-------------------|------------|
| 03.13.01 | Boundary Protection | Network controls | **Platform Inherited** |
| 03.13.03 | Information at Rest | Encryption at rest | **Inherited** |
| 03.13.04 | Information in Transit | TLS encryption | **Inherited** |
| 03.13.05 | Session Termination | Session timeout, logout | **Supported** |
| 03.13.06 | Network Disconnect | Session termination on disconnect | **Supported** |
| 03.13.08 | Transmission Confidentiality | TLS 1.2+ | **Inherited** |
| 03.13.11 | Cryptographic Protection | FIPS-validated modules | **Inherited** |

### 7.2 Detailed Control Analysis

<a id="031305-session-termination"></a>

#### 03.13.05 Session Termination

**Requirement:** Terminate session after defined conditions (inactivity, explicit logout, administrative action).

**OCI IAM Capability:**
- Configurable idle timeout
- Configurable absolute session duration
- Single logout (SLO) support
- Administrative session termination

**Gap Assessment:** **No Gap** — Full capability available

**Configuration Required:**
- Configure per [Session Timeout Configuration](implementation-guide.md#51-session-timeout-configuration)
- Idle: ≤15 minutes
- Absolute: ≤24 hours (800-171), ≤12 hours IL4, ≤8 hours IL5 (DISA SRG)

<a id="031311-cryptographic-protection"></a>

#### 03.13.11 Cryptographic Protection

**Requirement:** Implement FIPS-validated cryptography for CUI protection.

**OCI IAM Capability:**
- OCI platform uses FIPS 140-2/3 validated cryptographic modules
- TLS 1.2+ for data in transit
- AES-256 encryption at rest

**Gap Assessment:** **Inherited** — No customer configuration required

**Inheritance Statement:**
Cryptographic protection is inherited from the OCI platform authorization boundary. OCI DoD Realm environments operate with FIPS-validated cryptographic modules.

---

<a id="8-compliance-summary"></a>

## 8. Compliance Summary

### 8.1 Overall Assessment

| Category | Total Controls | Supported | Customer Config | Partial/Process | Inherited | Out of Scope |
|----------|---------------|-----------|-----------------|-----------------|-----------|--------------|
| Access Control (03.01) | 12 | 7 | 2 | 1 | 0 | 2 |
| Audit (03.03) | 7 | 4 | 0 | 0 | 1 | 2 |
| Identification & Auth (03.05) | 9 | 6 | 1 | 1 | 1 | 0 |
| System & Comms (03.13) | 7 | 2 | 0 | 0 | 5 | 0 |
| **Total** | **35** | **19** | **3** | **2** | **7** | **4** |

### 8.2 Compliance Determination

**OCI IAM Identity Domains supports NIST SP 800-171 Rev 3 compliance** for identity and access management controls when:

1. **MFA is enforced** using phishing-resistant methods (FIDO2 or CAC/PIV)
2. **Session timeouts** are configured to DoD ODP values (≤15 min idle, ≤24 hr absolute)
3. **Prohibited factors** (SMS, email, voice OTP) are disabled
4. **Audit logs** are forwarded to DoD-approved SIEM
5. **Password policies** meet DoD ODP requirements (16+ characters)
6. **Account lifecycle** processes are implemented per DoD ODP values

### 8.3 Controls Requiring Customer Action

| Control | Action Required | Reference |
|---------|-----------------|-----------|
| 03.01.01 | Configure SCIM provisioning, session limits | [Account Provisioning](#030101-system-account-management) |
| 03.01.10 | Set idle timeout ≤15 minutes | [Device Lock](#030110-device-lock) |
| 03.03.05 | Implement SIEM integration | [Audit Review](#030305-audit-review) |
| 03.05.03 | Enable MFA, disable non-compliant factors | [MFA](#030503-multifactor-authentication) |
| 03.05.05 | Document identifier management process | [Identifier Management](#030505-identifier-management) |
| 03.05.09 | Configure 16+ character password policy | [Password Authentication](#030509-password-authentication) |
| 03.13.05 | Configure session termination thresholds | [Session Termination](#031305-session-termination) |

---

<a id="9-cmmc-alignment"></a>

## 9. CMMC Alignment

NIST SP 800-171 Rev 3 serves as the foundation for Cybersecurity Maturity Model Certification (CMMC) Level 2.

### 9.1 CMMC Level 2 Practice Mapping

| CMMC Practice | NIST 800-171 Control | OCI IAM Support |
|---------------|---------------------|-----------------|
| AC.L2-3.1.1 | 03.01.01 | Account Management |
| AC.L2-3.1.5 | 03.01.05 | Least Privilege |
| AC.L2-3.1.7 | 03.01.07 | Privileged Functions |
| AU.L2-3.3.1 | 03.03.01 | Event Logging |
| IA.L2-3.5.1 | 03.05.01 | User Identification |
| IA.L2-3.5.3 | 03.05.03 | MFA |
| SC.L2-3.13.8 | 03.13.08 | Transmission Confidentiality |

### 9.2 CMMC Assessment Preparation

Organizations preparing for CMMC Level 2 assessment should:

1. Document OCI IAM configurations per this guide
2. Collect evidence artifacts per [ATO Evidence Checklist](implementation-guide.md#64-ato-evidence-checklist)
3. Map configurations to CMMC practice statements
4. Prepare POA&M for any identified gaps

---

<a id="appendix-h-nist-800-171-dod-odp-reference"></a>

## Appendix H: NIST 800-171 DoD ODP Reference

### H.1 IAM-Relevant Organization-Defined Parameters

| Control | ODP Description | DoD Value | Notes |
|---------|-----------------|-----------|-------|
| 03.01.01 | Account types allowed | Individual, group, system, service, emergency | No shared/generic accounts for CUI access |
| 03.01.01 | Inactive account disable | **90 days** | Automated process recommended |
| 03.01.01 | Session logout threshold | **24 hours** | DISA SRG may require shorter for IL4/IL5 |
| 03.01.10 | Device lock timeout | **15 minutes** | Maximum inactivity before lock |
| 03.02.01 | Security training frequency | Initial + **annual** | — |
| 03.03.01 | Audit event review frequency | **Quarterly** minimum | — |
| 03.05.05 | Identifier reuse prevention | **10 years** | — |
| 03.05.09 | Minimum password length | **16 characters** | When passwords are used |
| 03.05.09 | Password history | Organization-defined | Recommend 24 generations |
| 03.11.02 | Vulnerability scan frequency | **Monthly** | — |
| 03.13.11 | Cryptographic standard | **FIPS Validated** | Inherited from OCI |

### H.2 ODP Source

DoD-defined ODP values are published at:
[DoD CIO CMMC ODP Document](https://dodcio.defense.gov/Portals/0/Documents/CMMC/OrgDefinedParmsNISTSP800-171.pdf)

---

## Related Documents

- [DoD Authentication Standards](authentication-standards.md) — Policy requirements and mandatory controls
- [OCI IAM Assessment](oci-iam-assessment.md) — Platform suitability evaluation
- [Implementation Guide](implementation-guide.md) — Step-by-step configuration guidance
- [Executive Summary](executive-summary.md) — One-page ATO determination
- [NIST 800-53 Control Mapping](appendices.md#appendix-a-nist-sp-800-53-rev-5-control-mapping) — Detailed control mapping
