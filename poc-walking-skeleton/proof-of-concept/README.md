---
description: Also known as the Walking Skeleton
---

# Proof Of Concept

Koppeltaal 2.0 has been developed by using an Agile methodology. The devised architecture is tested using POC applications. The applications used within the POC are **not** the applications that will ultimately be used when Koppeltaal 2.0 goes live.

{% hint style="info" %}
Applications within the POC now often use Yivi (formerly known as IRMA) to identify the user and relate across systems. This is not part of the standard. It's likely that Koppeltaal 2.0 will not use Yivi in the production environment.
{% endhint %}

The POC consists of the following type of applications:

| Application type   | Description                                                                                                                                              |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| System component   | Applications that form the core of Koppeltaal 2.0.                                                                                                       |
| Supplier           | A supplier who uses their non-production platform together with the PoC Koppeltaal 2.0 Server.                                                           |
| Pseudo-application | Application that mimics an e-health application. These are removed from the list as they need to be updated with the latest changes in the specification |
| Additional service | Application that is not part of the Koppeltaal standard, but helps during the development.                                                               |

The POC consists of the following applications:

<table><thead><tr><th width="252.33333333333331">Application</th><th width="186">Type</th><th>Description</th></tr></thead><tbody><tr><td>Koppeltaal Server</td><td>System component</td><td>A FHIR HAPI R4 implementation for Koppeltaal.</td></tr><tr><td>Domain Management</td><td>System component</td><td>Applications can register to join a domain. A role is also assigned to applications, which determines what actions the application is allowed to perform on the Koppeltaal Server.</td></tr><tr><td>Auth Server</td><td>System component</td><td>Here - after approval from Domain Management - applications can request an access token via the <a href="https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token">Backend Services Authorization</a> flow.</td></tr><tr><td>Koppeltaal IdP</td><td>System component</td><td>Identity Provider (IdP) used to validate that the launching user matches the logged in user. This essentially prevents tokens from being intercepted and executed.</td></tr><tr><td>Domain Access Test Tooling</td><td>Additional service</td><td>This test suite helps with the implementation of the <a href="https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token">Backend Services Authorization</a>.</td></tr><tr><td>Koppeltaal Test Tooling</td><td>Additional service</td><td>A rich test of tools that help developers implement Koppeltaal. Look at <a href="../../technische-howto/koppeltaal-test-tooling.md">this</a> page for more information.</td></tr></tbody></table>

