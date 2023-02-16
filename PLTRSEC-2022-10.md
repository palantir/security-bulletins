# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-10

**CVE:** CVE-2022-27891

**Affected Products / Versions:** Palantir Gotham versions before 3.22.10.4

**Publication Date:** Feb 3, 2023

## Summary

Palantir Gotham included an unauthenticated endpoint that listed all active usernames on the stack with an active session. The affected services have been patched and automatically deployed to all Apollo-managed Gotham instances. It is highly recommended that customers upgrade all affected services to the latest version.

## Background

Palantir Gotham is a platform that enables the world’s most important organizations to surface insights from complex data and presents them in a single view that enables users to make faster, more confident decisions.

## Details

On September 30th, 2022, it was discovered that the Gotham included an unauthenticated endpoint that listed all active users. An attacker could repeatedly hit this endpoint, which would allow them to discover which users were active on the stack and at what times. This could aid in phishing attacks or leak sensitive personnel names.

## Remediation

For Gotham infrastructure without Apollo, customers will need to update the Gotham platform to version 3.22.10.4 or greater.

On Palantir-managed Gotham enrollments, the relevant services have been automatically upgraded to the fully-patched version.

## Timeline

2022-9-30: Pentesters notify Palantir infosec about vulnerable code path in Gotham. Palantir appsec validates and begins search for similar issues in other services.

2022-10-03: Appsec notifies product development of vulnerability.

2022-10-25: Product Development releases Gotham 3.22.10.4, which resolves the issue.

2023-02-03: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).
