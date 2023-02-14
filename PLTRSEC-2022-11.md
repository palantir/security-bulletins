# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-11

**CVE:** CVE-2022-27892

**Affected Products / Versions:** Gotham versions before 3.22.11.2

**Publication Date:** Feb 3, 2023

## Summary

Palantir Gotham versions prior to 3.22.11.2 included an unauthenticated endpoint that would have allowed an attacker to exhaust the memory of the Gotham dispatch server.

## Background

Palantir Gotham is a platform that enables the world’s most important organizations to surface insights from complex data and presents them in a single view that enables users to make faster, more confident decisions.

Dispatch is a core Gotham service used to enforce access control, generate logs, and coordinate requests to other parts of the system including the database.

## Details

On October 23rd, 2022, it was discovered that the Gotham included an unauthenticated endpoint that would log arbitrary sized objects. An attacker could repeatedly hit this endpoint with a large json object, which would allow them to exhaust memory resources on the dispatch server.

## Remediation

On Palantir-managed Gotham enrollments, Gotham has been automatically upgraded to the fully-patched version.

## Timeline

2022-10-26: Palantir infosec is notified about the unauthenticated logging endpoint in Gotham. Palantir appsec validates.

2022-11-09: Product development releases Gotham 3.22.11.2 which resolves the issue.

2023-2-03: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).
