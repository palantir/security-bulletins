# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-15

**CVE:** CVE-2022-27890

**Affected Products / Versions:**

* AtlasDB versions before 0.730.0

**Publication Date:** Feb 14, 2023

## Summary

Several Palantir services, including AtlasDB were not properly validating hostnames in TLS certificates. The affected services have been patched and automatically deployed to all Apollo-managed Foundry instances. It is highly recommended that customers upgrade all affected services to the latest version.

## Background

Palantir Foundry is a platform that enables auditable, arbitrary-scale transformations on data while preserving fine-grained access controls.

Palantir Gotham is a platform that enables the world’s most important organizations to surface insights from complex data and presents them in a single view that enables users to make faster, more confident decisions.

AtlasDB is a transactional layer on top of a key value store that serves as one of the primary data query interfaces for Palantir platforms.

## Details

On September 30th, 2022, it was discovered that the Gotham Chat IRC Helper was not verifying hostnames in TLS certificates due to a misuse of the javax.net.ssl.SSLSocketFactory API. A malicious attacker in a privileged network position could abuse this to perform a man-in-the-middle attack. A successful man-in-the-middle attack would allow them to intercept, read, or modify network communications to and from the affected service. Upon further investigation, several other services, including AtlasDB, magritte-ftp, and sls-logging were discovered to use a similar vulnerable pattern.

* In the case of AtlasDB, the vulnerability was mitigated by other network controls such as two-way TLS when deployed as part of a Palantir stack. Palantir still recommends upgrading to a non-vulnerable version out of an abundance of caution.

## Remediation

On Palantir-managed environments, the following services have been upgraded to patched versions which properly validate hostnames in TLS certificates. Customers which manage their own environments without Apollo should reach out to their forward deployed engineer or other Palantir POC.

* AtlasDB to version >=0.730.0

## Timeline

2022-9-30: Pentester notifies Palantir infosec about vulnerable code path in Gotham IRC Helper. Palantir appsec validates and begins search for similar issues in other products.

2022-10-3: Appsec notifies product development of vulnerability in Gotham Chat IRC Helper.

2022-10-5: Pentester notifies Palantir infosec of additional findings in Magritte-ftp, AtlasDB, and sls-logging.

2022-10-5: Appsec notifies product development of additional findings.

2022-10-14: AtlasDB patched and released

2023-02-14: [Public disclosure as per our commitment to transparency](_https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a_).
