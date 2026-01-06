---
description: The launch steps on this page are the same for HTI as SHOF
---

# Compose a launch

To perform the launch, the following steps must be performed for both HTI and SHOF:

### 1. Create Koppeltaal context claims

Koppeltaal supports the following claims:

<table><thead><tr><th width="124">HTI claim</th><th width="104">Type</th><th width="77">Required</th><th>Description</th></tr></thead><tbody><tr><td>resource</td><td>string</td><td>yes</td><td>The identifier of the task to be executed by the person in the <code>sub</code> field.</td></tr><tr><td>definition</td><td>url</td><td>no</td><td>A <code>Reference</code> that refers to the FHIR <code>ActivityDefinition</code>. The reference can be found on the <a href="https://simplifier.net/koppeltaalv2.0/instantiates"><code>Task.instantiates</code></a> extension.</td></tr><tr><td>sub</td><td>reference</td><td>yes</td><td>This is a <a href="https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/diffs/1?base_sha=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb&#x26;branch=fhir-2-fields&#x26;commentable=true&#x26;name=fhir-2-fields&#x26;pull_number=23&#x26;qualified_name=refs%2Fheads%2Ffhir-2-fields&#x26;sha1=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb&#x26;sha2=6f3ee98c1338f689c62e45a222d1e9e1d64142b8&#x26;short_path=baf3810&#x26;unchanged=expanded&#x26;w=false#person-reference">person reference</a> to the patient, practitioner or related person.</td></tr><tr><td>patient</td><td>reference</td><td>no</td><td>This is a <a href="https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/diffs/1?base_sha=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb&#x26;branch=fhir-2-fields&#x26;commentable=true&#x26;name=fhir-2-fields&#x26;pull_number=23&#x26;qualified_name=refs%2Fheads%2Ffhir-2-fields&#x26;sha1=0899d43a8ed3a53ccaf1ca3f1638753a28bdb7fb&#x26;sha2=6f3ee98c1338f689c62e45a222d1e9e1d64142b8&#x26;short_path=baf3810&#x26;unchanged=expanded&#x26;w=false#person-reference">person reference</a> to the patient, only used when the 'sub' is not a patient</td></tr><tr><td>intent</td><td>string</td><td>no</td><td>The intention of the launch, this field should be used to provide the intention of the launch such as the preparation, performance or review of the resource, is used, a value from the <a href="https://www.hl7.org/fhir/R4/valueset-request-intent.html">FHIR value set RequestIntent</a></td></tr><tr><td>idp_hint</td><td>string</td><td>no</td><td>Informs the auth server to use a specific IdP for the user authentication. Should be filled with the logical identifier of the IdP. See <a href="https://vzvznl.github.io/Koppeltaal-2.0-FHIR/multiple-idp-support.html">the IG</a> for more information.</td></tr></tbody></table>

An example of the resulting HTI claims:

```javascript
{
    "resource": "Task/5f684c5f-2837-4505-a534-365431912f37",
    "definition": "ActivityDefinition/d76ba97b-bfce-4a75-8e7a-2133778d1089",
    "sub": "Practitioner/225d67a7-69b9-4343-b488-064945fe3fd3",
    "patient" : "Patient/b592f103-f75b-4a63-a5dd-b75799775258",
    "intent": "plan",
    "idp_hint": "primary-practitioner-idp"
}
```

### 2. Create the JWT

The `HTI:core` v2.0 profile also contains claims that are always set on a JWT:

| Description       | Field | Value                                                                                                                                                                                                                                                                                             |
| ----------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Issuer            | iss   | The `client_id` van het portaal                                                                                                                                                                                                                                                                   |
| Audience          | aud   | The device id of the module provider. This can be found via the `ActivityDefinition.resource-origin` extension.                                                                                                                                                                                   |
| Unique message id | jti   | A unique identifier for this message. This value **MUST** be treated as a NONCE, a subsequent message with an identical jti **MUST** be rejected. The jti value must be a random or pseudo number, the jti **MUST** contain enough entropy to block brute-force attacks.                          |
| Issue time        | iat   | The timestamp of generating the JWT token, the value of this field **MUST** be validated by the module provider to not be in the future.                                                                                                                                                          |
| Expiration time   | exp   | The "exp" (expiration time) claim identifies the expiration time on or after which the JWT **MUST NOT** be accepted for processing. The value **MUST** be limited to 5 minutes. This value **MUST** be validated by the module provider, any value that exceeds the timeout **MUST** be rejected. |

These two sets of claims should be combined to form a valid HTI v2.0 JWT. For example:

```json
{
  "iat": 1585564845,
  "aud": "Device/123",
  "iss": "portal-client-id",
  "exp": 1585565745,
  "jti": "679e1e4c-bcb9-4fcc-80c4-f36e7063545c"
  "sub" : "Practitioner/225d67a7-69b9-4343-b488-064945fe3fd3",
  "resource" : "Task/5f684c5f-2837-4505-a534-365431912f37",
  "definition" : "ActivityDefinition/d76ba97b-bfce-4a75-8e7a-2133778d1089",
  "patient" : "Patient/b592f103-f75b-4a63-a5dd-b75799775258",
  "intent": "plan"
}
```

The timestamps follows the ["UNIX time"](https://en.wikipedia.org/wiki/Unix_time) convention, being the number of seconds since the epoch.

### 3. Sign the JWT

After the JWT is completely filled, it must be signed. For this, [Signing the JWT](../../connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md) can be followed.

### 4. Launch

The signed token can now be launched to the client. To find the endpoint to be launched to, the `ActivityDefinition.endpoint` extension can be used. The correct ActivityDefinition can be found using `Task.instantiates` extension (see [resource-ophalen.md](../../resources-managen/crud-operaties/resource-ophalen.md "mention")).

There are minimal differences between sending via [HTI](hti-launch-versturen.md) and [SHOF](broken-reference/).

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)
