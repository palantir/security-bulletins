# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-12

**CVE:** CVE-2022-27893

**Affected Products / Versions:** Palantir Gotham

**Publication Date:** Feb 3, 2022

## Summary

Palantir Gotham version prior to 3.22.11.2 included two unauthenticated endpoints that would have allowed an attacker to exhaust the memory of the Gotham dispatch server.

## Background

Palantir Gotham is a platform that enables the world’s most important organizations to surface insights from complex data and presents them in a single view that enables users to make faster, more confident decisions.

Dispatch is a core Gotham service used to enforce access control, generate logs, and coordinate requests to other parts of the system including the database.

## Details

On November 6th it was discovered that Gotham included an unauthenticated endpoint that would load portions of maliciously crafted zip files to memory. An attacker could repeatedly hit either of these endpoints, which would allow them to exhaust memory resources on the dispatch server.

## Remediation

On Palantir-managed Gotham enrollments, the relevant services have been automatically upgraded to the fully-patched version.

## Timeline

2022-11-06: Palantir infosec is notified about the unauthenticated zip upload vulnerability in Gotham. Palantir appsec validates and notifies product development.

2022-11-18: Gotham version 3.22.11.2 is released, resolving the issue.

2023-2-03: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).
