# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-03

**CVE:** CVE-2022-27893

**Affected Products / Versions:** osisoft-pi-web-connector versions 0.15.0 - 0.43.0. Resolved in 0.44.0.

**Publication Date:** November 03, 2022

## Summary

The *Foundry Magritte plugin **osisoft-pi-web-connector*** was found to be logging in a manner that captured authentication requests. This vulnerability is resolved in osisoft-pi-web-connector version 0.44.0. Magritte sources which leverage this plugin using HTTP Basic Authentication should change their OSISoft PI System account credentials.

## Background

Magritte is the Palantir Foundry data ingestion framework. Magritte agents run Java plugins that implement both standard and custom transfer protocols according to customer needs, including *osisoft-pi-web-connector* for integrating with customer OSISoft PI Systems. Magritte plugins must be cryptographically signed by Palantir, to ensure authenticity and customer control over the Foundry data ingestion capability.

## Details

On April 14, 2022, it was discovered that the Authentication HTTP request headers logged by the Magritte plugin *osisoft-pi-web-connector* could be capturing plaintext usernames and passwords in places where Basic Authentication was being utilized.

An investigation was opened and subsequently determined that this vulnerability was introduced across two pull requests to the osisoft-pi-web-connector plugin. The first change adding HTTP request logging was submitted on October 8, 2021, and released as version 0.15.0. The second change adding HTTP Basic Authentication was submitted on January 13, 2022 and released as version 0.23.0. Following this release, Magritte data-ingestion-administrators requiring access to OSISoft PI System data could manually install or upgrade their instance of the osisoft-pi-web-connector to a vulnerable version. Logged OSISoft PI System secrets took the form of either eight-hour Kerberos tickets, or HTTP Basic Authentication username/password pairs.

Access to the logs where these authentication secrets were captured is strictly limited to authorized Palantir personnel based on role. These systems are highly segmented from corporate infrastructure, are encrypted, and meet strict security controls, including multi-factor authentication. Upon discovery of the issue, all access to the logging infrastructure was temporarily suspended. By default, OSISoft PI System gateways are only accessible from the customer’s own VPC. For environments that have Basic Authentication configured, Palantir is working directly with impacted to reset any OSISoft PI System credentials used for data ingestion.

Our Investigation concluded with no indications of abuse or malicious activity observed across the entirety of our hosted platform.

## Remediation

The osisoft-pi-web-connector plugin must be manually upgraded if installed in a Foundry instance.

**Versions:** osisoft-pi-web-connector 0.15.0 - 0.43.0

**Impact:** Magritte agents with vulnerable versions of this plugin may leak account credentials into logs.

**Remediation:**

* Manually upgrade the osisoft-pi-web-connector plugin to version 0.44.0 or newer
* Change the account credentials that Magritte uses to connect to OSISoft PI System
* Customers are encouraged to review their OSISoft PI System instance logs for activity from unexpected IP addresses or anomalous web User-Agents.

## Timeline

2021-10-08: The Palantir Product Development team merges a code change that intentionally logs HTTP request headers issued by the Magritte osisoft-pi-web-connector plugin.

2022-01-13: Product Development introduces an additional change which adds Basic Authentication support to the osisoft-pi-web-connector plugin.

2022-04-14: Issue was identified internally at Palantir. Palantir CIRT acknowledges the incident and initiates response procedures.

2022-04-14: As a result of the coordinated investigation, affected products and versions are identified. Product Development issues a patch and releases of osisoft-pi-web-connector. Product fixes are backported to TODO. Palantir's software delivery mechanisms are updated such that vulnerable plugin versions can no longer be deployed.

2022-04-14: Palantir CIRT performs a manual review and investigation of all Palantir employees who had accessed Magritte agent logging during the exposure timeframe.

2022-04-15: Customer disclosure of the issue and engagement of credential rotation begins.

2022-04-15: Palantir CIRT completes a manual review of access to logging systems containing captured authorization headers. No indicators of abuse or malicious activity are observed across our hosted platform.

2022-04-15: All external OSISoft PI System accounts were confirmed updated.

2022-11-03: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
