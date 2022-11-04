# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-05

**CVE:** N/A

**Affected Products / Versions:** apollo-deployment-state < 4.714.0, delivery-metadata < 2.565.0,  team-ownership < 0.171.0

**Publication Date:** November 4, 2022

## Summary

The *delivery-metadata* service in Palantir Apollo was found to permit API endpoints that did not adequately require authentication to query, potentially granting read access to metadata such as deployed software version numbers to unintended recipients. The subsequent investigation uncovered insufficient authentication controls in the *team-ownership* service as well, which is responsible for metadata pertaining to package installations. These vulnerabilities are resolved in apollo-deployment-state version 4.714.0, delivery-metadata version 2.565.0, and team-ownership version 0.171.0, respectively. As part of maintaining good security hygiene, it is highly recommended that all customers upgrade to the latest version of all relevant Apollo services.

## Background

Palantir Apollo is a comprehensive software change management platform responsible for the operational lifecycle of the Palantir Platform (including Foundry and Gotham). Apollo includes a management frontend, autopilot, and a number of middleware services, known as apollo-deployment-state. The Apollo service delivery-metadata handles state and policy inputs for automated package management, while team-ownership concerns contact information and operational responsibilities of package maintainers in a given deployment.

## Details

On May 27th, 2022, source code review of the delivery-metadata service uncovered API endpoints where information about Palantir software running on a stack would be provided to an API user, regardless of whether that user provided a valid authorization token. A malicious attacker could leverage information about currently running software to identify further vulnerabilities, potentially leading to additional exploitation.

An investigation was opened and subsequently determined that this vulnerability was also present in the team-ownership service. The investigation further determined that unauthenticated endpoints have existed in all versions of delivery-metadata prior to 2.565.0 and team-ownership prior to 0.171.0.

Our investigation concluded with no indications of abuse or malicious activity observed across the entirety of our hosted platform.

Customer support advisories have been issued informing of the issue and our response.

## Remediation

On Palantir-managed Apollo hubs enrollments, the affected services have been automatically upgraded to the fully-patched version.

### Palantir-Managed Apollo-Connected Environments

**Versions:** delivery-metadata < 2.565.0,  team-ownership < 0.171.0

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.

### Customer-Managed Apollo Environments

**Versions:** delivery-metadata < 2.565.0,  team-ownership < 0.171.0

**Impact:** Potential information leakage via vulnerable endpoints.

**Remediation:**

* * Upgrade the affected services to fixed versions.
* Please coordinate with your forward deployed engineering team or Palantir POC for further information on remediation and response.

## Timeline

2022-05-27: A Palantir employee discovers the issue, and files a critical incident notification with the Palantir CIRT.

2022-05-27: Palantir CIRT acknowledges the incident and initiates response procedures.

2022-05-27: Product Development issues a patch and releases delivery-metadata version 2.565.0 and team-ownership version 0.171.0, which remediate this issue. Additionally, apollo-deployment-state version 4.714.0 is released which introduces a check to prevent this issue from being reintroduced in the future. Palantir's software delivery mechanisms are updated such that vulnerable service versions can no longer be deployed.

2022-05-27: Customer disclosure of the issue.

2022-11-04: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
