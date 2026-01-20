# DoD-Approved Authentication Standards for Web Applications

**Applicable Environment:** OCI DoD Realm (IL4/IL5/IL6)
**Purpose:** SSP, Security Control Implementation Statements, ATO Review Package
**Last Updated:** January 2026

---

## 1. Governing Policy and Standards Framework

| Document | Applicability |
|----------|---------------|
| **DoDI 8520.02** | PKI and Public Key Enabling policies |
| **DoDI 8520.03** | Primary authentication instruction for DoD information systems |
| **NIST SP 800-63-4** | Digital identity guidelines (IAL, AAL, FAL) |
| **NIST SP 800-53 Rev 5** | Security controls baseline (IA family) |
| **DISA Cloud Computing SRG v1r5** | Cloud-specific authentication requirements by Impact Level |
| **CNSSI 1253** | Security categorization for National Security Systems |
| **FIPS 201-3** | PIV credential and authentication requirements |
| **DoD CIO MFA Policy (2024)** | Non-PKI MFA approval and implementation requirements |
| **DoD ICAM Federation Framework** | Federation practices and trust relationships |

---

## 2. Authentication Assurance Level (AAL) Requirements

Per NIST SP 800-63B-4 and DoDI 8520.03, DoD web applications must meet the following minimum assurance levels:

| Impact Level | Minimum AAL | Primary Method | Acceptable Alternatives |
|--------------|-------------|----------------|------------------------|
| **IL4** (CUI) | AAL2 | DoD PKI (CAC/PIV) | DoD-approved non-PKI MFA with compensating controls |
| **IL5** (CUI/NSS) | AAL2/AAL3* | DoD PKI (CAC/PIV) | DoD-approved non-PKI MFA (restricted scenarios) |
| **IL6** (Classified) | AAL3 | DoD PKI (CAC/PIV) | Hardware token MFA only |

*NSS data at IL5 typically requires AAL3. **Privileged administrative access in IL5 environments SHALL meet AAL3** through CAC/PIV or hardware-backed phishing-resistant MFA.

---

## 3. Mandatory Requirements

### 3.1 All DoD Web Applications

The following NIST SP 800-53 Rev 5 controls are mandatory:

- **IA-2: Identification and Authentication (Organizational Users)** — Uniquely identify and authenticate organizational users
- **IA-2(1): MFA to Privileged Accounts** — Multi-factor authentication required for all privileged access
- **IA-2(2): MFA to Non-Privileged Accounts** — MFA required for network access to non-privileged accounts
- **IA-2(6): Access to Accounts - Separate Device** — One authentication factor provided by device separate from system gaining access
- **IA-2(8): Access to Accounts - Replay Resistant** — Replay-resistant authentication mechanisms
- **IA-2(12): Acceptance of PIV Credentials** — Accept and verify PIV credentials

### 3.2 Authentication Strength (DoDI 8520.03 Credential Strength D)

In order of preference:

1. PKI certificate-based authentication (CAC/PIV) — **Required default**
2. Hardware token + memorized secret
3. Phishing-resistant MFA (FIDO2/WebAuthn) where PKI unavailable

**Password-only authentication is prohibited for all IL4/IL5/IL6 environments.**

### 3.3 CAC/PIV Authentication

CAC/PIV authentication satisfies MFA requirements through:
- **Possession factor:** Physical smart card
- **Knowledge factor:** PIN entry

---

## 4. Conditional Requirements

### 4.1 When PKI Is Not Available

Per DoD CIO MFA Policy (2024), non-PKI MFA is permitted when:

1. Cloud Service Offering (CSO) holds appropriate DISA Provisional Authorization or FedRAMP certification
2. Non-PKI MFA capability integrated with DoD-approved ICAM service provider
3. MFA solution appears on DoD-approved non-PKI MFA list
4. Documented waiver/exception per DoDI 8520.03 paragraph 3.3.b

### 4.2 Acceptable Non-PKI MFA Methods (IL4/IL5 Non-Privileged Access)

| Method | IL4 | IL5 | Notes |
|--------|-----|-----|-------|
| FIDO2/WebAuthn hardware authenticators | Approved | Approved | Phishing-resistant, preferred |
| TOTP hardware tokens (OATH-compliant) | Conditional | Conditional | Requires hardware binding |
| Mobile push with cryptographic binding | Limited | Not Recommended | Restricted scenarios only |

### 4.3 Prohibited Methods

The following are **prohibited at IL4 and above**:

- SMS-based OTP
- Email-based OTP
- Voice call OTP
- Software-only tokens without hardware binding (IL5+)

---

## 5. Federation Requirements

Per NIST SP 800-63C and DoDI 8520.03:

| FAL | Requirement | DoD Application |
|-----|-------------|-----------------|
| **FAL1** | Bearer assertion, signed | Minimum for IL2 only |
| **FAL2** | Encrypted assertion OR back-channel presentation | **Required for IL4+** |
| **FAL3** | Holder-of-key assertion with cryptographic authenticator proof | **Required for NSS at IL5+** |

### 5.1 Federation Protocol Requirements

- SAML 2.0 or OpenID Connect (OIDC)
- Assertions must be signed using DoD-approved PKI
- Assertions must be encrypted (FAL2 compliance)
- Authentication Context Class Reference enforcement
- Assertion lifetime ≤5 minutes
- Single-use assertion enforcement
- Back-channel token resolution preferred

---

## 6. Identity Assurance Level (IAL) Requirements

Per NIST SP 800-63A:

| Impact Level | Minimum IAL | Identity Proofing Method |
|--------------|-------------|-------------------------|
| **IL4** | IAL2 | Remote or in-person proofing with evidence validation |
| **IL5** | IAL2/IAL3* | In-person proofing for NSS |
| **IL6** | IAL3 | In-person proofing required |

*IAL3 conditional for IL5 systems with national security implications.

For DoD personnel, IAL3 is typically inherited through the PIV issuance process per FIPS 201-3.

---

## 7. Session Management Requirements

Per NIST SP 800-63B-4 and DISA SRG:

| Control | IL4 Requirement | IL5 Requirement |
|---------|-----------------|-----------------|
| Session timeout (idle) | ≤15 minutes | ≤15 minutes |
| Session timeout (absolute) | ≤12 hours | ≤8 hours |
| Reauthentication | Every 12 hours | Every 12 hours (or per session for privileged) |
| Token binding | Recommended | Required for privileged access |
| Replay protection | Required | Required |
| Continuous/adaptive authentication | Optional | Conditional for high-risk sessions |

### 7.1 Token Binding Clarification

Token binding requirements are satisfied through:
- Proof-of-possession authenticators (FIDO2)
- Short-lived tokens with rotation
- Cryptographic session binding

Note: TLS Token Binding (RFC 8471) is not widely implemented. Equivalent protections are achieved through the mechanisms listed above.

---

## 8. ATO Assessor Expectations

Assessors will verify:

1. **Evidence of PKI integration** or documented exception with approved non-PKI MFA
2. **Sign-on policy configurations** enforcing MFA for all users
3. **Federation trust documentation** including SAML metadata exchange records
4. **Session timeout configurations** meeting DISA SRG thresholds
5. **Audit log samples** demonstrating authentication event capture
6. **Authentication flow diagrams** showing credential validation path
7. **Inheritance statements** clearly delineating CSP vs. customer responsibility
8. **Explicit prohibition** of password-only authentication in all policies
9. **Disabled non-compliant factors** (SMS, email, voice OTP)

---

## Related Documents

- [OCI IAM Identity Domains Assessment](oci-iam-assessment.md)
- [Implementation Guide](implementation-guide.md)
- [Executive Summary](executive-summary.md)
- [Control Mappings and Appendices](appendices.md)
