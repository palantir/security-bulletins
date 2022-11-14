# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-01

**CVE:** CVE-2022-27888

**Affected Products / Versions:** Foundry Issues versions 2.244.0 to 2.249.0. Resolved in 2.249.1.

**Publication** **Date:** March 29, 2022

## Summary

The ***Foundry Issues*** service was found to be logging in a manner that captured session tokens. This vulnerability is resolved in Foundry Issues 2.249.1, which has been automatically deployed to all Apollo-managed Foundry instances. Out of an abundance of caution, all session tokens with a time-to-live (TTL) longer than 24 hours have been revoked from our hosted platforms.

## Background

Palantir Foundry is a platform that enables auditable, arbitrary-scale transformations on data while preserving fine-grained access controls. Foundry Issues is an in-platform interface to Palantir Customer Support, and enables customers to report, track, and resolve problems within Foundry.

## Details

On March 15th, 2022, it was discovered that the Foundry Issues service was logging the session token of users that directly interacted with the service. These sessions tokens, if accessed by an unauthorized party, could allow Foundry access under the user context for the lifetime of the token (e.g. a default of 10 hours).

An investigation was opened and subsequently determined that this vulnerability was introduced in Foundry Issues version 2.244.0. As a result, any account that interacted with the Foundry Issues service between February 21st, 2022 and March 16, 2022 had their session token incorrectly and unexpectedly recorded in Palantir service event logs. These event logs are stored on the backend of the Palantir Foundry infrastructure and, in hosted Palantir environments, also flow to centralized event logging infrastructure that is used by Palantir engineers to provide operational support.

Access to the logs where these tokens may have been captured is strictly limited to authorized Palantir personnel based on role. These systems are highly segmented from corporate infrastructure, are encrypted, and meet strict security controls, including multi-factor authentication. Upon discovery of the issue, all access to the logging infrastructure was temporarily suspended and all logged session tokens were purged. Furthermore, the risk of token exposure is time-bound by the session token time-to-live (TTL) on the Palantir Foundry platform. The TTL for Foundry sessions is 10 hours by default, after which the session token expires and is no longer useful.

Our Investigation concluded with no indications of abuse or malicious activity observed across the entirety of our hosted platform.

While we are confident there was no adverse impact to customer environments or data, this misconfiguration does not meet our high bar for security, and we remain committed to providing best in class information security. We have scheduled a root cause analysis for the relevant engineering teams, and a plan of action will be implemented to prevent similar regressions in the future.

## Remediation

For Foundry infrastructure without Apollo, customers will need to update Foundry Issues to 2.249.1 or greater. Additionally, backend event logs may need to be purged on the underlying Foundry infrastructure.

On Palantir-managed Foundry enrollments, the Foundry Issues service has been automatically upgraded to the fully-patched version. Out of an abundance of caution, all session tokens were revoked. In most cases, these tokens had already expired due to the time-to-live values.

### Palantir Foundry - Customer hosted (without Apollo)

**Versions:** Foundry Issues [2.244.0 - 2.249.0]

**Impact:** Foundry installations with vulnerable versions of Issues may leak session tokens into local logs.

**Remediation:**

* Upgrade to Foundry Issues version 2.249.1 or greater.
* Event logs may need to be purged on the underlying Foundry infrastructure to remove valid session tokens from logs.
* Customers are encouraged to review Foundry audit (security) logs to look for unusual authentication activity during the time period specified.
    * Review SSH or privileged access to your Foundry infrastructure. Foundry event logs are stored on the backend filesystem of the underlying hosts. A malicious actor would need to have read access to the logs in question to extract session tokens. This can be further investigated with your endpoint security tooling.
    * Review Multipass authentication logs. As session tokens are already authenticated, you can identify abuse by identifying unusual IP addresses or access patterns for a user. Since session tokens have a short time-to-live, concurrent access may be an indicator of malicious activity.
* Please coordinate with your forward deployed engineering team or Palantir POC for further information on remediation and response.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions:** Foundry Issues [2.244.0 - 2.249.0]

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.

## Timeline

2022-02-21: Product Development merges a code change that inadvertently logs Foundry Multipass session tokens.

2022-03-15: A Palantir employee discovers the issue, and files a critical incident notification with the Palantir CIRT.

2022-03-15: Palantir CIRT acknowledges the incident and initiates response procedures.

2022-03-15: As a result of the coordinated investigation, affected products and versions are identified. Foundry Issues versions 2.244.0 (released February 21, 2022) to 2.249.0 (released March 15, 2022) are impacted with this bug.

2022-03-15: Product Development issues a patch and releases Foundry Issues version 2.249.1. Products fixes are backported to Foundry Issues 2.249.0, 2.248.0, 2.247.0. Palantir's software delivery mechanisms are updated such that vulnerable Foundry Issues versions can no longer be deployed.

2022-03-16: Customer disclosure of the issue.

2022-03-16: Palantir CIRT performs a manual review and investigation of all Palantir employees who had accessed Foundry Issues event logs in the period of concern.

2022-03-16: Palantir CIRT revokes or expires all active tokens.

2022-03-16: CVE reserved for issue.

2022-03-19: Palantir CIRT completes a manual review of the all access to logging systems containing captured session tokens. No indicators of abuse or malicious activity are observed across our hosted platform.

2022-03-29: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
