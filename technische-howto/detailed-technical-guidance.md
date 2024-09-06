# Detailed technical guidance

The developer guide helps out to get started. But of course, there is more to Koppeltaal. There are multiple resources that can be used when you need more advanced technical guidance

## Implementation Guide

When you implementation specific use-cases, and want to know if Koppeltaal has any specific implementation rules for that use-case, the Implementation Guide can be used.

{% embed url="https://simplifier.net/guide/koppeltaal?version=current" %}

## Simplifier profile

When you need to grasp the data structures used in Koppeltaal, it's best to look at the simplifier profile. This profile contains all the supported resources, its attributes and cardinalities. As well, it contains useful examples that can help you get started.&#x20;

#### Finding the current profile version

Profiles evolve, and have multiple versions. To look at the exact version used on the server, you can [request](koppeltaal-server-metadata-opvragen.md) the server `/metadata`. The `/metadata` shows the server's `CapabilityStatement`. The `CapabilityStatement.implementationGuide` should point to a KT2 `ImplementationGuide` resource. This is a canonical reference, and thus points to an `ImplementationGuide.url` value. Currently, it points to `http://koppeltaal.nl/fhir/ImplementationGuide`.&#x20;

So, for the reference implementation, it can be requested as such:

{% embed url="https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/ImplementationGuide?url=http%3A%2F%2Fkoppeltaal.nl%2Ffhir%2FImplementationGuide" %}

This resource contains a `version` field that corresponds to the released version of simplifier used on the server.

#### Looking at the specific Simplifier version

After [finding the current profile version](detailed-technical-guidance.md#finding-the-current-profile-version), navigate to the simplifier packages history page and match the version:

{% embed url="https://simplifier.net/packages/Koppeltaalv2.00/0.14.0-beta.7/~history" %}

Opening up a specific version shows all the details of the specific package. The `Files` tab shows all files that are packages in the released. In here you can also find the profile details of that specific version.

## Koppeltaal 2.0 Specification & Architecture

Most of the documentation is already linked to the specific topics of the Koppeltaal 2.0 Specification & Architecture. These specifications contain a very detailed description of the functionality in Koppeltaal, but also all technical requirements. The requirements can be found in the topics

{% embed url="https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27115333/Standaard+topics" %}
KT2.0 topics
{% endembed %}

{% embed url="https://vzvz.atlassian.net/wiki/spaces/KTSA/overview" %}
KT 2.0 Specification & Architecture
{% endembed %}
