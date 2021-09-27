---
description: De launch stappen in deze pagina werkt hetzelfde voor HTI als SHOF
---

# Launch samenstellen

Om de launch uit te voeren moeten de volgende stappen voor zowel HTI als SHOF uitgevoerd worden:

### 1. FHIR Task maken

Het `HTI:core` profiel beschrijft de volgende velden voor de  `Task`:

| FHIR task field | Verplicht | Beschrijving |
| :--- | :--- | :--- |
| resourceType | ja | Dit veld moet altijd gevuld zijn met de waarde "Task". |
| id | ja | De logische identifier van de taak. Meer details staan beschreven in de [Task id](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md#the-task-id) sectie. |
| instantiatesCanonical | nee | Een [canonical](http://hl7.org/fhir/R4/references.html#canonical) URL die refereert naar de FHIR definition. Meer details hierover zijn te vinden in de [activity definition](https://www.hl7.org/fhir/activitydefinition.html) documentatie. |
| for | ja | Een [person reference](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md#person-reference) naar de gebruiker die de taak moet uitvoeren. |
| intent | ja | Dit veld moet altijd gevuld zijn met een waarde van de [FHIR value set RequestIntent](https://www.hl7.org/fhir/R4/valueset-request-intent.html), indien niet van toepassing, gebruik "plan". |
| status | ja | Status van de taak. Dit veld moet gevuld zijn met een waarde van de [FHIR value set TaskStatus](https://www.hl7.org/fhir/R4/valueset-task-status.html). |

Een `HTI:core Task` kan er als volgt uitzien:

```javascript
{
    "resourceType": "Task",
    "id": "a5e57fd0",
    "instantiatesCanonical": "https://example.org/ActivityDefinition/a5e58200",
    "for": {
      "reference": "Patient/a5e5844e"
    },
    "intent": "plan",
    "status": "requested"
}
```

### 2. JWT Maken

Het `HTI:core` profiel beschrijft de volgende velden voor de JWT:

| Description | Field | Value |
| :--- | :--- | :--- |
| Issuer | iss | This field **MUST** be a reference to the portal application that creates the JWT token. It **MAY** consist of a url or domain name of the portal application. The portal application and module provider **MUST** agree on the value of the iss field. The module provider **MUST** validate the message with the public key associated with the iss reference. The portal application **MAY** disseminate its public keys by the JKWS protocol, in that case the JWT token **MUST** contain a kid field in the JWT header. |
| Audience | aud | This field **MUST** be a reference to the module provider for which the JWT token is created for. The reference **MAY** consist of a url or domain name of the module provider. The portal application and module provider **MUST** agree on the value of the aud field. The module provider **MUST** validate the aud field to have the expected value. |
| Unique message id | jti | A unique identifier for this message. This value **MUST** be treated as a NONCE, a subsequent message with an identical jti **MUST** be rejected. The jti value must be a random or pseudo number, the jti **MUST** contain enough entropy to block brute-force attacks. |
| Issue time | iat | The timestamp of generating the JWT token, the value of this field **MUST** be validated by the module provider to not be in the future. |
| Expiration time | exp | This value **MUST** be the time-out of the exchange sending it to the client plus the time-out of the exchange used by the client to send it, the value **MUST** be limited to 5 minutes. This value **MUST** be validated by the module provider, any value that exceeds the timeout **MUST** be rejected. |
| Subject | sub | This value **MUST** be a person reference to the user executing the launch. This way, applications can understand _who_ is launching the provided `Task`. For example, `Practitioner/82421` |
| Task | task | The FHIR Task object in JSON format. |
| FHIR Version | fhir-version | The FHIR version for the provided `Task`. When this field is not provided, the FHIR version **MUST** be the latest stable FHIR release. Consumers should evaluate this field in a case-insensitive manner. Currently, the following fields are allowed: `STU3`, `R4` and `R5`. It is strongly advised to always set this field, even when using the latest stable FHIR version. This prevents HTI breaking after a new FHIR stable release. |

The timestamps follows the ["UNIX time"](https://en.wikipedia.org/wiki/Unix_time) convention, being the number of seconds since the epoch.

### 3. JWT Ondertekenen

Nadat de JWT volledig gevuld is, moet deze worden ondertekend. Hiervoor kan [JWT Ondertekenen](../../connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md) gevolgd worden.

### 4.  Launchen

Het ondertekende token kan nu gelaunched worden naar de client. Om de endpoint te vinden waarheen gelaunched moet worden kan gebruik gemaakt worden van de `ActivityDefinition.endpoint` extensie. De juiste `ActivityDefinition` kan gevonden worden a.d.h.v. `Task.instantiatesCanonical` \(zie [Resource Ophalen](../../resources-managen/crud-operaties/resource-ophalen.md)\).

Er zijn minimale verschillen tussen het versturen via [HTI](hti-launch-versturen.md) en [SHOF](shof-launch-versturen.md).

