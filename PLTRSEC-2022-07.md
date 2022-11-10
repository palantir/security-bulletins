# Security Bulletin

**Bulletin ID:** PLTRSEC-2022-07

**CVE:** Not Applicable

**Affected Products / Versions:** Rubixbeat versions 2.0.0 to 2.266.0 **

**Publication Date:** November 7, 2022

## Summary

An information disclosure issue was discovered in Rubixbeat, a logging component of Palantir Apollo, when receiving logs originating from the Foundry Code-Workbooks service.

## Background

Code-Workbooks is a rapid development and iteration tool for writing code to transform Foundry datasets. Code-Workbooks, and other Foundry services, are often running in Apollo Kubernetes-based orchestration environments, ensuring that network and service isolation can be codified in strict policies.

Rubixbeat is a logging collector for Apollo environments, implemented on top of Elastic N.V.‘s [Beats log forwarding libraries](https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html) (Libbeat). Rubixbeat is installed on all Apollo nodes and flexibly routes service, infrastructure, and system logs.

## Details

On June 10th, 2022, routine log analysis found Foundry tokens in logs originating from the Code-Workbooks service. These log messages were errors thrown by a vendored Libbeat library (docker_json.go), when encountering a malformed line in a file containing single-line-JSON-formatted log lines. Depending on the contents of the unparsed input log line, the resulting error log could include arbitrary sensitive data, such as Foundry tokens.

The Palantir signals-logging-infra library is leveraged for large parts of Rubixbeat’s behavior, and includes the docker_json.go dependency for parsing Docker logs in its default “json-file” format. In response, Palantir has denylisted lines matching the output format of the specific Libbeat parser error messages. As an additional defensive measure, the signals-logging-infra library added a regex filter that can detect and remove Foundry tokens in these spurious log lines. Additional denylisting of lines matching the parser error was also implemented.

Addressing the root cause of this vulnerability involves a broad set of challenges. It is possible that Elastic N.V. doesn’t consider parser errors in Libbeat leaking secrets or other sensitive data to be a vulnerability, because their libraries implicitly assume a single-responsibility model for log storage and ownership.

## Remediation

On Palantir-managed Apollo hubs enrollments, the affected services have been automatically upgraded to the fully-patched version.

For Foundry infrastructure without Apollo, customers will need to update Rubixbeat to version 2.267.0 or greater. Additionally, backend service logs may need to be purged on the underlying Apollo hub infrastructure.

### Palantir Foundry - Palantir Cloud or Apollo-Connected Environments

**Versions:** Rubixbeat <= 2.266.0

**Impact:** Remediated.

**Remediation:**

* Palantir Apollo has deployed updated versions of platform services to remediate this issue. No customer action is required.
* Customers are encouraged to review their Code-Workbooks service logs to determine if any tokens have been leaked.

### Palantir Foundry - Customer hosted (without Apollo)

Rubixbeat is not a component of customer hosted environments without Apollo management.

## Timeline

May 27, 2019: Introduction of error logging for parser errors in Libbeat docker_json.go module. The broader patch was intended to enable skipping of otherwise unreadable lines in input files.

February 25, 2020: Rubixbeat 2.0.0 was released, upgrading its Libbeat dependency to v7, the first version that included the parser errors in logging.

June 10, 2022: Initial discovery of tokens in Rubix logs. Access to logs was revoked. Remediations and token filtering were implemented. Rubixbeat 2.267.0 was released and deployed through Apollo across all connected stacks.

2022-11-07: [Public disclosure as per our commitment to transparency](https://blog.palantir.com/broadening-our-bug-bounty-program-trust-security-and-transparency-aa3bf82f3f9a).

## Acknowledgement

This issue was identified internally at Palantir.
