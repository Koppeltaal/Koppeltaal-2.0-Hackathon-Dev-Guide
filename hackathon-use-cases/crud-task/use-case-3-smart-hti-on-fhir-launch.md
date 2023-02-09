---
description: Launching a Task via SMART HTI On FHIR (SHOF).
---

# Use-case 3: SHOF Launch

This use-case ensures that clients can launch in a similar way to Koppeltaal 1.x. In addition, the SHOF launch performs an additional security check that ensures the launch token is not intercepted and executed by someone else.

### Composing a launch

{% hint style="info" %}
This step is identical between HTI and SHOF
{% endhint %}

The documentation [Compose a launch](use-case-3-smart-hti-on-fhir-launch.md#composing-a-launch) explains in detail how to this should be done.&#x20;

### Receiving a SHOF launch

The documentation [Receiving a SHOF launch](../../technische-howto/launchen/smart-hti-on-fhir-launch-ontvangen.md) explains in detail how to handle being launched to via SHOF.

### Testing

The SHOF Launch can be tested using the [Launch Test Suite](https://launch-testsuite.koppeltaal.headease.nl/portal.html) to launch to the POC module. You can launch to the POC module using the following parameters in the Test Suite:

| Attribute      | Value                                                                                                                |
| -------------- | -------------------------------------------------------------------------------------------------------------------- |
| Audience (aud) | [https://poc-module.koppeltaal.headease.nl/module\_launch](https://poc-module.koppeltaal.headease.nl/module\_launch) |
| Launch url     | [https://poc-module.koppeltaal.headease.nl/module\_launch](https://poc-module.koppeltaal.headease.nl/module\_launch) |
