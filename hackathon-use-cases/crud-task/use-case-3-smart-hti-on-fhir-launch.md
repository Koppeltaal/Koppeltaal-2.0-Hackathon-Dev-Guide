---
description: Lanceren van een taak middels SMART HTI On FHIR.
---

# Use-case 3: SHOF Launch

Deze use-case zorgt er voor dat er zo veel mogelijk als Koppeltaal 1.x ingelogd kan worden.

### Launch Uitsturen

Hiervoor kan [Launch samenstellen](../../technische-howto/launchen/launch-samenstellen/) geraadpleegd worden.

### Launch Ontvangen

Hiervoor kan [SHOF Launch Ontvangen](../../technische-howto/launchen/smart-hti-on-fhir-launch-ontvangen.md) geraadpleegd worden.

### Testen

De SHOF Launch kan getest worden middels de [Launch Test Suite](https://launch-testsuite.koppeltaal.headease.nl/portal.html). Er kan gelaunched worden naar de POC Module middels de volgende parameters in de Test Suite:

| Attribuut | Waarde |
| :--- | :--- |
| Audience \(aud\) | [https://poc-module.koppeltaal.headease.nl/module\_launch](https://poc-module.koppeltaal.headease.nl/module_launch) |
| Launch url | [https://poc-module.koppeltaal.headease.nl/module\_launch](https://poc-module.koppeltaal.headease.nl/module_launch) |

