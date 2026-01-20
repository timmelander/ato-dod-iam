# Oracle IAM Identity Domains — DoD ATO Suitability Assessment

**Applicable Environment:** OCI DoD Realm (IL4/IL5/IL6)
**Purpose:** SSP, Security Control Implementation Statements, ATO Review Package
**Last Updated:** January 2026

---

## 1. Platform Authorization Status

| Attribute | Status |
|-----------|--------|
| **OCI US Government Cloud** | FedRAMP High JAB P-ATO |
| **OCI US Defense Cloud** | DISA IL4, IL5, IL6 Provisional Authorization |
| **IAM Identity Domains availability** | Available in DoD Realm regions |

Oracle IAM Identity Domains operates within OCI's authorized boundary for DoD workloads at IL4/IL5/IL6.

---

## 2. Control Inheritance Narrative

Oracle IAM Identity Domains inherits the following from the OCI DoD Realm authorization boundary:

- **Cryptographic protections** — TLS, encryption at rest
- **Infrastructure security** — Physical, network, platform controls
- **Audit log integrity** — OCI Audit service tamper protection
- **Service availability** — Oracle-managed SLAs and redundancy

**Customer responsibility includes:**

- Identity policy enforcement
- MFA configuration and factor selection
- Federation trust establishment
- Session timeout configuration
- Log forwarding and extended retention
- User lifecycle management

---

## 3. Compliance Capability Determination

### 3.1 Assessment Question

Does Oracle IAM Identity Domains provide the required technical capabilities to satisfy DoD authentication standards for an OCI DoD Realm?

### 3.2 Assessment Answer

**YES** — Oracle IAM Identity Domains can satisfy DoD authentication standards when properly configured with required external integrations.

---

## 4. Native Capabilities Supporting Compliance

| Capability | DoD Requirement Addressed | Implementation Notes |
|------------|---------------------------|----------------------|
| SAML 2.0 federation | Federation with DoD IdPs (FAL2/FAL3) | Full SP and IdP support |
| OIDC support | Alternative federation protocol | Native support |
| X.509 certificate authentication | CAC/PIV direct authentication | Via X.509 IdP configuration |
| FIDO2/WebAuthn | Phishing-resistant MFA (AAL2/AAL3) | Supported in Premium domain types |
| Sign-on policies | Context-based access control (IA-2) | Granular rule configuration |
| MFA enforcement | IA-2(1), IA-2(2) | Multiple compliant factor types |
| Session management | AC-11, AC-12 | Configurable timeouts |
| Audit logging | AU-2, AU-3, AU-12 | OCI Audit service integration |
| Adaptive security | Risk-based authentication | Device, network, location evaluation |
| SCIM provisioning | Account lifecycle management | Automated sync with authoritative sources |

---

## 5. Required External Integrations

| Integration | Purpose | Configuration Path |
|-------------|---------|-------------------|
| **DoD Enterprise IdP (ICAM)** | PKI authentication source, CAC/PIV validation | SAML 2.0 federation |
| **Active Directory Federation Services** | CAC/PIV authentication | Federation with DoD AD FS |
| **DoD PKI Infrastructure** | Certificate chain validation, CRL/OCSP | Performed at DoD IdP |
| **SIEM Platform** | Centralized security monitoring | OCI Audit → Streaming → SIEM |

---

## 6. Limitations and Compensating Controls

| Limitation | Impact | Compensating Control |
|------------|--------|---------------------|
| Native CAC/PIV not directly terminated | Cannot directly validate DoD PKI chain within OCI IAM | Federate with DoD-managed IdP that performs certificate validation |
| SMS OTP available but non-compliant | Risk of enabling prohibited factor | Disable SMS factor in domain MFA settings; enforce compliant factors only via sign-on policy |
| Email OTP available but non-compliant | Risk of enabling prohibited factor | Disable email factor; enforce compliant factors only |
| Session timeout defaults may exceed SRG | Non-compliance if unchanged | Configure sign-on policy with ≤15 min idle, ≤12 hr (IL4) / ≤8 hr (IL5) absolute |
| No RFC 8471 Token Binding | Token binding compliance question | Achieved via FIDO2 proof-of-possession and short-lived tokens |

---

## 7. ATO Supportability Statement

**Oracle IAM Identity Domains IS ATO-supportable for OCI DoD Realm deployments at IL4 and IL5**, provided the following conditions are met:

### 7.1 Mandatory Conditions

1. **Federation with DoD-approved IdP** — Authentication must be federated through a DoD-approved Identity Provider that performs CAC/PIV validation and certificate revocation checking

2. **Phishing-resistant MFA enforcement** — Sign-on policies must enforce phishing-resistant MFA (FIDO2 or CAC/PIV) for all privileged access

3. **Non-compliant factors disabled** — SMS OTP, email OTP, and voice call OTP must be disabled in domain MFA settings

4. **Session management compliance** — Session timeouts must be configured to meet DISA SRG thresholds:
   - Idle: ≤15 minutes
   - Absolute: ≤12 hours (IL4), ≤8 hours (IL5)

5. **Audit log forwarding** — Authentication events must be forwarded to a DoD-approved SIEM with required retention periods

6. **Password-only authentication disabled** — MFA must be enforced for all users; password-only access must be prohibited

### 7.2 Domain Type Requirement

Identity Domain must be **Premium** or **Oracle Apps Premium** type to access the full MFA feature set including FIDO2/WebAuthn support.

---

## 8. IL4 vs IL5 Distinctions

| Aspect | IL4 | IL5 |
|--------|-----|-----|
| Minimum AAL | AAL2 | AAL2 (AAL3 for privileged/NSS) |
| Session absolute timeout | ≤12 hours | ≤8 hours |
| Federation assurance | FAL2 | FAL2 (FAL3 for NSS) |
| Non-PKI MFA | Permitted with exception | Restricted; hardware-only |
| Mobile push MFA | Conditional | Not Recommended |

---

## 9. Customer vs Oracle Responsibility Matrix

| Control Area | Oracle (CSP) | Customer |
|--------------|:------------:|:--------:|
| Infrastructure security | ✓ | |
| IAM service availability | ✓ | |
| Platform cryptographic controls | ✓ | |
| Native audit log capture | ✓ | |
| Sign-on policy configuration | | ✓ |
| MFA factor enablement/disablement | | ✓ |
| Federation trust establishment | | ✓ |
| Session timeout configuration | | ✓ |
| Audit log forwarding to SIEM | | ✓ |
| Extended log retention | | ✓ |
| User provisioning/deprovisioning | | ✓ |
| Privileged access review | | ✓ |
| Identity proofing (inherited from IdP) | | ✓ |

---

## 10. Assumptions

1. Customer operates within an OCI DoD Realm tenancy with appropriate DISA authorization
2. DoD enterprise IdP is available and configured for CAC/PIV certificate authentication
3. Customer has documented exception if using non-PKI MFA for any access scenarios
4. SIEM platform is DoD-approved for log aggregation at appropriate classification level
5. Identity domain type is Premium (or Oracle Apps Premium) for full MFA feature set
6. Certificate revocation checking (OCSP/CRL) is performed by the federated DoD IdP

---

## Related Documents

- [DoD Authentication Standards](authentication-standards.md)
- [Implementation Guide](implementation-guide.md)
- [Executive Summary](executive-summary.md)
- [Control Mappings and Appendices](appendices.md)
