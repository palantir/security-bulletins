# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-08

**CVE:** CVE-2022-27896

**Affected Products / Versions:** Code-Workbooks, version 4.144.0 to 4.460.0

**Publication Date:** November 10, 2022

## Summary

An information disclosure issue was discovered in Foundry Code-Workbooks.

## Background

Palantir Foundry is a platform that enables auditable, arbitrary-scale transformations on data while preserving fine-grained access controls. Code-Workbooks is a rapid development and iteration tool for writing code to transform Foundry datasets.

## Details

On June 10th, 2022, routine log analysis discovered Foundry tokens in central logs from the Code-Workbooks service. The logs were related to an interactive Python console Code-Workbooks offers to users, to quickly evaluate running Python code against a particular dataset. The Websockets endpoint backing that console was generating service log records of any Python code being run. These service logs included the Foundry token that represents the Code-Workbooks Python console.

In response to this discovery, Palantir released a same-day fix to all Apollo customers that removed this logging statement, and closed off access to Code-Workbooks service logs.

## Remediation

On Palantir-managed Apollo hubs enrollments, the affected services have been automatically upgraded to the fully-patched version.

For Foundry infrastructure without Apollo, customers will need to update Rubixbeat to version 2.267.0 or greater. Additionally, backend service logs may need to be purged on the underlying Apollo hub infrastructure.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions:** Code-Workbooks, version 4.144.0 to 4.460.0

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.
* Customers are encouraged to review X services to determine their exposure to information leaks in RemoteException stacktrace messages.

### Palantir Foundry - Customer hosted (without Apollo)

**Versions:** Code-Workbooks, version 4.144.0 to 4.460.0

**Impact:** Foundry Regions with vulnerable versions of the above services may leak arbitrary data into logs.

**Remediation:**

* Upgrade to Code-Workbooks version 4.461.0.
* Event logs may need to be purged on the underlying Foundry infrastructure to remove sensitive data from the logs.
* Please coordinate with your forward deployed engineering team or Palantir POC for further information on remediation and response.

## Timeline

November 4, 2020: Console autocompletion feature added to the Code-Workbooks Python console, which included a logging statement for the command being run.

June 10, 2022:  Initial discovery of tokens in Rubix logs. Access to logs was revoked. Remediation removes unnecessary logging statements in Code-Workbooks. Version 4.461.0 was deployed through Apollo on all connected stacks.

2022-11-10: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
