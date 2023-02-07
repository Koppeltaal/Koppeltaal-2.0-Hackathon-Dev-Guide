# Launching

Koppeltaal supports two launch mechanisms: [HTI](hti.md) and [SMART HTI On FHIR](smart-hti-on-fhir.md). Both mechanisms work at their core with the HTI JWT token, making both systems very similar in implementation. For sending out a launch, there are minimal differences. The receiving party (`module provider`) can, in the case of a SHOF launch, still choose to execute the HTI Flow.&#x20;

{% hint style="info" %}
After reading the launch documentation, we recommend looking at the [Launch Test Suite](https://launch-testsuite.koppeltaal.headease.nl/portal.html). Through this suite, the differences between HTI and SHOF are made clear. It also clarifies which payload is sent.
{% endhint %}
