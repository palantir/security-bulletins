# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-02

**CVE:** CVE-2022-27889

**Affected Products / Versions:** Multipass versions before 3.647.0

**Publication Date:** June 9, 2022

## Summary

The ***Multipass*** service was found to have code paths that could be abused to cause a denial of service for authentication or authorization operations. This vulnerability is resolved in Multipass 3.647.0, which has been automatically deployed to all Apollo-managed Foundry instances. As part of maintaining good security hygiene, it is highly recommended that all customers upgrade to the latest version of Multipass.

## Background

Palantir Foundry is a platform that enables auditable, arbitrary-scale transformations on data while preserving fine-grained access controls. Multipass is the backend service provider which manages access to Palantir software products.

## Details

On March 28th, 2022, it was discovered that the Foundry Multipass service was unusually susceptible to performance degradation or denial of service when saturated with specific requests. A malicious attacker could perform an application-level denial of service attack, potentially causing authentication and/or authorization operations to fail for the duration of the attack. This could lead to performance degradation or login failures for customer Palantir Foundry environments.

An investigation was opened and subsequently determined that this vulnerability resulted from inefficient code paths that presented abusable primitives.

## Remediation

For Foundry infrastructure without Apollo, customers will need to update Foundry Multipass to 3.647.0 or greater.

On Palantir-managed Foundry enrollments, the Foundry Multipass service has been automatically upgraded to the fully-patched version.

### Palantir Foundry - Customer hosted (without Apollo)

**Versions:** Foundry Multipass [< 3.647.0]

**Impact:** Denial of service caused by high load of unauthenticated requests.

**Remediation:**

* Upgrade to Foundry Multipass version 3.647.0 or greater.
* Please coordinate with your forward deployed engineering team or Palantir POC for further information on remediation and response.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions:** Foundry Multipass [< 3.647.0]

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.

## Timeline

2022-03-28: Performance degradation is observed for a small number of hosted Palantir Cloud environments. Palantir's Computer Incident Response Team (CIRT) acknowledges the incident and initiates response procedures.

2022-03-28: Investigation reveals that Multipass may be susceptible to denial of service caused by a high load of unauthenticated requests. Product Development teams are engaged to investigate and deploy patches.

2022-03-28: Web application firewall (WAF) rules are implemented to mitigate denial of service conditions. Additional mitigations are implemented at the cloud hosting provider infrastructure layer.

2022-03-28: Palantir CIRT confirms the activity was a result of good-faith security research conducted by bug bounty participants. Alerting and detection strategies are implemented to monitor for further abuse of vulnerable code paths while product patches are developed.

2022-03-29: Minor performance degradation is observed for a small number of hosted Palantir Cloud environments. Instability is transient. Impacted customers are notified and additional mitigations are implemented.

2022-03-30: Product Development issues a patch and releases Foundry Multipass 3.646.0. This significantly mitigates the denial of service condition.

2022-03-31: Product Development issues a patch and releases Foundry Multipass 3.647.0. This resolves the identified vulnerability.

2022-04-01: Palantir's software delivery mechanisms are updated such that all hosted environments are upgraded to Multipass 3.647.0 and vulnerable versions can no longer be deployed.

2022-04-01: CVE reserved for issue.

2022-06-06: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir. Initial activity was observed as a result of [good-faith security research conducted by bug bounty participants](https://hackerone.com/palantir_public).
