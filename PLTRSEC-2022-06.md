# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-06

**CVE:** CVE-2022-27895

**Affected Products / Versions:** build2

**Publication Date:** November 14, 2022

## Summary

The phonograph2 service in Foundry was discovered to be exposing data in logs which were being captured by the build2 service.

## Background

Palantir Foundry is a platform that enables auditable, arbitrary-scale transformations on data while preserving fine-grained access controls. Phonograph provides APIs for creating, reading, updating, deleting, and searching the contents of Foundry datasets. To offer these capabilities, Phonograph leverages Foundry’s Spark-based build infrastructure for processing and caching Foundry data into its own indexes.

## Details

On June 9th, 2022, routine log analysis uncovered rows of customer data being centrally collected. Code review pointed to a root problem found in phonograph2-cache-worker-spark-module. In the process of a Phonograph Spark job running and generating log messages, the RemoteException stacktrace was being treated as “Safe” content for logging.

In response to this discovery, Palantir began reviewing its other services for variants of this information leak. A similar issue was discovered in transforms-spark-module-lib, a library relied on for many types of Spark jobs. Effectively, any services running Spark jobs that result in RemoteException stacktraces could expose customer data.

Only data from Phonograph jobs have been discovered in this incident, due to the short timeframe of centralized logging. However, in recognition of possible information leakage in logs from other Spark jobs, the Build2 service now filters any arguments associated with RemoteException log messages.

## Remediation

On Palantir-managed Apollo hubs enrollments, the affected services have been automatically upgraded to the fully-patched version.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions:** build2 < 1.785.0

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.
* Customers are encouraged to review their Build2 logs services to determine their exposure to information leaks in RemoteException stacktrace messages.

### Palantir Foundry - Customer hosted (without Apollo)

**Versions:** build2 < 1.785.0

**Impact:** Foundry Regions with vulnerable versions of the above services may leak arbitrary data into logs.

**Remediation:**

* Upgrade to Build2 version 1.785.0 or greater.
* Event logs may need to be purged on the underlying Foundry infrastructure to remove sensitive data from the logs.
* Please coordinate with your forward deployed engineering team or Palantir POC for further information on remediation and response.

## Timeline

2022-06-09: A Palantir employee discovers the issue, and files a critical incident notification with the Palantir CIRT.

2022-06-09: Palantir CIRT acknowledges the incident and initiates response procedures.

2022-06-09: Product Development issues a patch and releases build2 version 1.785.0, which remediates this issue. Palantir's software delivery mechanisms are updated such that vulnerable service versions can no longer be deployed.

2022-11-14: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
