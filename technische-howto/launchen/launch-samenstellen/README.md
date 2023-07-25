---
description: The launch steps on this page are the same for HTI as SHOF
---

# Compose a launch

To perform the launch, the following steps must be performed for both HTI and SHOF:

### 1. Create HTI context claims

The `HTI:core` v2.0 profile describes the following claims:

| HTI claim  | Type      | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------- | --------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| resource   | string    | yes      | The identifier of the task to be executed by the person in the `sub` field.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| definition | url       | no       | A [canonical](http://hl7.org/fhir/R4/references.html#canonical) URL that refers to the FHIR definition. More details can be found in the [activity definition](https://www.hl7.org/fhir/r4/activitydefinition.html) documentation.                                                                                                                                                                                                                                                                      |
| sub        | reference | yes      | This is a [person reference](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/diffs/1?base\_sha=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb\&branch=fhir-2-fields\&commentable=true\&name=fhir-2-fields\&pull\_number=23\&qualified\_name=refs%2Fheads%2Ffhir-2-fields\&sha1=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb\&sha2=6f3ee98c1338f689c62e45a222d1e9e1d64142b8\&short\_path=baf3810\&unchanged=expanded\&w=false#person-reference) to the patient or practitioner.                           |
| patient    | reference | no       | This is a [person reference](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/diffs/1?base\_sha=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb\&branch=fhir-2-fields\&commentable=true\&name=fhir-2-fields\&pull\_number=23\&qualified\_name=refs%2Fheads%2Ffhir-2-fields\&sha1=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb\&sha2=6f3ee98c1338f689c62e45a222d1e9e1d64142b8\&short\_path=baf3810\&unchanged=expanded\&w=false#person-reference) to the patient, only used when the 'sub' is not a patient |
| intent     | string    | no       | The intention of the launch, this field should be used to provide the intention of the launch such as the preparation, performance or review of the resource, is used, a value from the [FHIR value set RequestIntent](https://www.hl7.org/fhir/R4/valueset-request-intent.html)                                                                                                                                                                                                                        |

An example of the resulting HTI claims:

```javascript
{
    "resource": "Task/5f684c5f-2837-4505-a534-365431912f37",
    "definition": "https://module.example.com/ActivityDefinition/d76ba97b-bfce-4a75-8e7a-2133778d1089",
    "sub": "Practitioner/225d67a7-69b9-4343-b488-064945fe3fd3",
    "patient" : "Patient/b592f103-f75b-4a63-a5dd-b75799775258",
    "intent": "plan"
}
```

### 2. Create the JWT

The `HTI:core` v2.0 profile also contains claims that are always set on a JWT:

| Description       | Field | Value                                                                                                                                                                                                                                                                                             |
| ----------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Issuer            | iss   | The `client_id` van het portaal                                                                                                                                                                                                                                                                   |
| Audience          | aud   | The launch `URL` of the module provider. This can be found via the `ActivityDefinition.endpoint` extension.                                                                                                                                                                                       |
| Unique message id | jti   | A unique identifier for this message. This value **MUST** be treated as a NONCE, a subsequent message with an identical jti **MUST** be rejected. The jti value must be a random or pseudo number, the jti **MUST** contain enough entropy to block brute-force attacks.                          |
| Issue time        | iat   | The timestamp of generating the JWT token, the value of this field **MUST** be validated by the module provider to not be in the future.                                                                                                                                                          |
| Expiration time   | exp   | The "exp" (expiration time) claim identifies the expiration time on or after which the JWT **MUST NOT** be accepted for processing. The value **MUST** be limited to 5 minutes. This value **MUST** be validated by the module provider, any value that exceeds the timeout **MUST** be rejected. |

These two sets of claims should be combined to form a valid HTI v2.0 JWT. For example:

```json
{
  "iat": 1585564845,
  "aud": "https://module.example.com/launch/endpoint",
  "iss": "portal-client-id",
  "exp": 1585565745,
  "jti": "679e1e4c-bcb9-4fcc-80c4-f36e7063545c"
  "sub" : "Practitioner/225d67a7-69b9-4343-b488-064945fe3fd3",
  "resource" : "Task/5f684c5f-2837-4505-a534-365431912f37",
  "definition" : "https://module.example.com/ActivityDefinition/d76ba97b-bfce-4a75-8e7a-2133778d1089",
  "patient" : "Patient/b592f103-f75b-4a63-a5dd-b75799775258",
  "intent": "plan"
}
```

The timestamps follows the ["UNIX time"](https://en.wikipedia.org/wiki/Unix\_time) convention, being the number of seconds since the epoch.

### 3. Sign the JWT

After the JWT is completely filled, it must be signed. For this, [Signing the JWT](../../connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md) can be followed.

### 4. Launch

The signed token can now be launched to the client. To find the endpoint to be launched to, the `ActivityDefinition.endpoint` extension can be used. The correct ActivityDefinition can be found using `Task.instantiatesCanonical` (see [resource-ophalen.md](../../resources-managen/crud-operaties/resource-ophalen.md "mention")).

There are minimal differences between sending via [HTI](hti-launch-versturen.md) and [SHOF](broken-reference/).

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)
