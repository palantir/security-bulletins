# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-04

**CVE:** CVE-2022-27894

**Affected Products / Versions:** Foundry Blobster versions 3.207.0 to 3.227.0. Resolved in 3.228.0.

**Publication** **Date:** November 03, 2022

## Summary

The ***Blobster*** service was found to have a cross-site scripting (XSS) vulnerability that could have allowed an attacker with access to Foundry to launch attacks against other users. This vulnerability is resolved in Blobster 3.228.0, which has been automatically deployed to all Apollo-managed Foundry instances. As part of maintaining good security hygiene, it is highly recommended that all customers upgrade to the latest version of Blobster.

## Background

In Palantir Foundry, the Blobster service allows users to upload arbitrary files into the Compass filesystem. This is intentional, supported behavior, as Foundry is designed to ingest and handle highly-customized workflows for customers, and Palantir places content and execution restrictions on these uploads to prevent malicious abuse.

## Details

On April 22, 2022, it was discovered that the content security policy for the Compass Blobster service was not correctly applying expected process execution restrictions on uploaded files. This introduced a cross-site scripting (XSS) vulnerability, which meant that an authenticated user account could upload a malicious JavaScript file, followed by an HTML file that would load and execute the JavaScript. The attacker could then provide other users with a link to this HTML file and, if the user clicked on the malicious link, execute the JavaScript payload in the context of the target user. This would allow the attacker to perform actions in Foundry as the target user, or to capture the target user's session token.

An investigation was opened and subsequently determined that this vulnerability was introduced on January 21, 2022, as part of a migration in the underlying web framework used by Compass, which changed the headers necessary to apply content restrictions on uploaded data. Palantir Product Development teams developed and deployed modifications to the content security policy that re-enabled execution restrictions on files uploaded to Compass via Blobster.

Our Investigation concluded with no indications of abuse or malicious activity observed across the entirety of our hosted platform.

While we are confident there was no adverse impact to customer environments or data, this misconfiguration does not meet our high bar for security, and we remain committed to providing best in class information security. We have scheduled a root cause analysis for the relevant engineering teams, and a plan of action will be implemented to prevent similar regressions in the future.

## Remediation

For Foundry infrastructure without Apollo, customers will need to update Blobster to 3.228.0 or greater.

On Palantir-managed Foundry enrollments, the Blobster service has been automatically upgraded to the fully-patched version.

### Palantir Foundry - Customer hosted (without Apollo)

**Versions:** Foundry Blobster versions 3.207.0 to 3.227.0

**Impact:** A malicious user with access to Blobster can upload files that, when visited, execute Javascript.

**Remediation:**

* Upgrade to Foundry Blobster version 3.228.0 or greater.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions:** Foundry Blobster versions 3.207.0 to 3.227.0

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.

## Timeline

2022-01-21: Product Development migrates the Compass Blobster service to a server implementation that did not honor all previously-enforced content security policies.

2022-04-22: A Palantir employee discovers the issue, and files a critical incident notification with the Palantir CIRT.

2022-04-22: Palantir CIRT acknowledges the incident and initiates response procedures.

2022-04-22: As a result of the coordinated investigation, affected products and versions are identified. Foundry Blobster versions 3.207.0 (released January 21, 2022) to 3.227.0 (released April 20, 2022) are impacted with this bug.

2022-04-22: Product Development issues a patch and releases Compass Blobster version 3.228.0. The patch is also backported to Compass Blobster versions 3.185.6, 3.215.4, 3.123.4, and 3.225.1. Palantir's software delivery mechanisms are updated such that vulnerable Foundry Blobster versions can no longer be deployed.

2022-04-22: Customer disclosure of the issue.

2022-04-22: Palantir CIRT begins a manual review and investigation of all uploaded file types in the period of concern.

2022-04-24: Palantir CIRT completes a manual review of the all access to logging systems containing captured session tokens. No indicators of abuse or malicious activity are observed across our hosted platform.

2022-06-10: CVE reserved for issue.

2022-11-03: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
