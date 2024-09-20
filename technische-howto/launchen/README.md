# Launching

Koppeltaal supports two launch mechanisms: [HTI](hti.md) and [SMART HTI On FHIR](smart-hti-on-fhir.md). The latter is based on the [SMART App Launch](https://www.hl7.org/fhir/smart-app-launch/). There are slight differences, but where possible they are similar. Both mechanisms work at their core with the HTI JWT token, making both systems very similar in implementation. For sending out a launch, there are minimal differences. The receiving party (`module provider`) can, in the case of a SHOF launch, still choose to execute the HTI Flow.

{% hint style="info" %}
After reading the launch documentation, we recommend looking at the [Koppeltaal Test Tooling](../koppeltaal-test-tooling.md). This offers an interactive way of sending and/or receiving a launch.
{% endhint %}
