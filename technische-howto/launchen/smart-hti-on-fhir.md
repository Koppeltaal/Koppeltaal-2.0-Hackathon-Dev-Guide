---
description: SMART HTI On FHIR
---

# SHOF Flow

{% hint style="warning" %}
The Koppeltaal Server authorises applications, not users. This is a mismatch with the [SMART App Launch Framework](http://www.hl7.org/fhir/smart-app-launch/) (SALF). For example, the SALF flow always expects to return a user-level `access_token`. With SMART HTI On FHIR (SHOF), an `access_token` will also be returned, only this is a `no-op` token.
{% endhint %}

SMART HTI On FHIR (SHOF) uses the HTI token as the `launch` parameter value. This means that the launching party can reuse all logic during a launch. The main differences between HTI and SHOF are:

1. SHOF uses the `launch` parameter to pass the context as a HTI token, where HTI uses the `token` parameter to pass the context as a HTI token
2. SHOF is based on an international standard: [SMART App Launch Framework](http://www.hl7.org/fhir/smart-app-launch/).
3. SHOF performs an additional check on the logged-in user during the `/authorize` step, using a shared IdP. This prevents a launch token from being intercepted and executed. The user identifier contained in the launch token is compared to the identifier of the logged in user on the IdP.

### Requirements

1. A [JWKS endpoint must be available](../connectie-maken-met-koppeltaal/requirements/jwks-opzetten.md).
2. The user performing the launch must have an account on the shared IdP, the username has to be present on the corresponding user Resource (`Patient`, `Practitioner`, or `RelatedPerson`).

### Information Flow

<figure><img src="../../.gitbook/assets/SMART on FHIR app launch and HTI.drawio (1).png" alt="SMART HTI Flow"><figcaption><p>SMART HTI Flow</p></figcaption></figure>

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)
