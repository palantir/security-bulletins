# Security Bulletin

**Bulletin ID:** PLTRSEC-2023-01

**CVE:** N/A

**Affected Products / Versions:** db-controller, versions 0.45.0 to 0.101.0

**Publication Date:** January 06, 2023

## Summary

An information disclosure issue was discovered in db-controller that leaked database credentials when installed in a Kubernetes-based service orchestration environment.

## Background

db-controller is an infrastructure-level component used across all three of Palantir’s platforms. It facilitates consistent configuration management, metrics, and health monitoring for any databases it manages.

## Details

On July 5th, 2022, an information leak was discovered in db-controller logging output. On modern Palantir Cloud or Palantir Apollo-connected environments using Kubernetes orchestration, if db-controller was used to manage a pre-existing database, and the db-controller configuration overrides included a customized masterUserPassword, it was possible the configuration involving this master password would be exposed in logs.

In Apollo instances where db-controller database access is managed with a randomly-generated credential, these passwords were not exposed.

## Remediation

On Palantir-managed environments, db-controller has been upgraded to version 0.101.1 (or later), which no longer logs configuration fragments that could contain passwords. After this upgrade occurred, Palantir determined all stacks that may have logged passwords in the past and changed those passwords to new randomly generated values.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions**: db-controller, versions 0.45.0 to 0.101.0
**Impact**: Remediated
**Remediation**:

* Palantir Apollo has deployed updated versions of platform services to remediate this issue.
* Operations teams coordinated with stack owners to reset impacted database credentials.

## Timeline

2021-11-09: db-controller 0.45.0 released, with a new interface for discovering information about database instances running in an Apollo cluster.

2022-07-05: Routine log review of db-controller discovered the master user password was being leaked into logs for this service. db-controller 0.101.1 was released with a fix for the issue, and deployed across all centrally-managed stacks. Palantir determined which centrally managed stacks matched the criteria for database credentials exposure, and started planning and coordinating password rotations.

2022-07-11: Password rotation for all known impacted stacks was completed.

2022-01-23: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
