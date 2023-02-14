# Security Bulletin

**Bulletin ID:** PLTRSEC-2023-02

**CVE:** CVE-2022-48306

**Affected Products / Versions:**

* Gotham Chat IRC versions before 30221005.210011.9242

**Publication Date:** Feb 14, 2023

## Summary

Several Palantir services, including Gotham Chat IRC helper, were not properly validating hostnames in TLS certificates. The affected services have been patched and automatically deployed to all Apollo-managed Foundry and Gotham instances. It is highly recommended that customers upgrade all affected services to the latest version.

## Background

Palantir Foundry is a platform that enables auditable, arbitrary-scale transformations on data while preserving fine-grained access controls.

Palantir Gotham is a platform that enables the world’s most important organizations to surface insights from complex data and presents them in a single view that enables users to make faster, more confident decisions.

The Gotham Chat IRC Helper is an optional plugin that allows Gotham Chat to connect to existing IRC servers and view messages.

## Details

On September 30th, 2022, it was discovered that the Gotham Chat IRC Helper was not verifying hostnames in TLS certificates due to a misuse of the javax.net.ssl.SSLSocketFactory API. A malicious attacker in a privileged network position could abuse this to perform a man-in-the-middle attack. A successful man-in-the-middle attack would allow them to intercept, read, or modify network communications to and from the affected service.

In the case of a successful man in the middle attack on the Gotham Chat IRC Helper, an attacker could read and modify network traffic such as messages received through the Gotham Chat IRC Helper. This would allow them to impersonate users or gain access to sensitive information.

## Remediation

On Palantir-managed environments, the following services have been upgraded to patched versions which properly validate hostnames in TLS certificates. Customers which manage their own environments without Apollo should reach out to their forward deployed engineer or other Palantir POC.

* Gotham IRC Helper, if it is in use, to version >=30221005.210011.9242 (Gotham only)

## Timeline

2022-9-30: Pentester notifies Palantir infosec about vulnerable code path in Gotham IRC Helper. Palantir appsec validates and begins search for similar issues in other products.

2022-10-3: Appsec notifies product development of vulnerability in Gotham Chat IRC Helper.

2022-10-14: Gotham IRC Helper patched and released.

2023-02-14: [Public disclosure as per our commitment to transparency](_https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a_).
