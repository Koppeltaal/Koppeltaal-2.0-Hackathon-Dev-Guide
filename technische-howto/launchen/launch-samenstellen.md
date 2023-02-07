---
description: The launch steps on this page are the same for HTI as SHOF
---

# Compose a launch

To perform the launch, the following steps must be performed for both HTI and SHOF:

### 1. Create a FHIR Task

The `HTI:core` v1.1 profile describes the following fields for the `Task`:

| FHIR task field       | Mandatory | Description                                                                                                                                                                                                                        |
| --------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| resourceType          | yes       | This field must always be filled with the value "Task".                                                                                                                                                                            |
| id                    | yes       | The logical identifier of the task. More details are described in the [Task id section](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md#the-task-id).                                                  |
| instantiatesCanonical | no        | A [canonical](http://hl7.org/fhir/R4/references.html#canonical) URL that refers to the FHIR definition. More details can be found in the [activity definition](https://www.hl7.org/fhir/r4/activitydefinition.html) documentation. |
| for                   | yes       | A [person reference](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md#person-reference) to the user that has to execute the `Task`.                                                                     |
| intent                | yes       | This field must always be filled with a value from the [RequestIntent value set](https://www.hl7.org/fhir/R4/valueset-request-intent.html), if not applicable, use `plan`.                                                         |
| status                | yes       | Status of the task. This field must be filled with a value from the [TaskStatus value set](https://www.hl7.org/fhir/R4/valueset-task-status.html).                                                                                 |

A `HTI:core Task` can look like this:

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

### 2. Create the JWT

The `HTI:core` v1.1 profile describes the following fields for the JWT:

| Description       | Field        | Value                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Issuer            | iss          | The `client_id` van het portaal                                                                                                                                                                                                                                                                                                                                                                                                             |
| Audience          | aud          | The launch `URL` of the module provider. This can be found via the `ActivityDefinition.endpoint` extension.                                                                                                                                                                                                                                                                                                                                 |
| Unique message id | jti          | A unique identifier for this message. This value **MUST** be treated as a NONCE, a subsequent message with an identical jti **MUST** be rejected. The jti value must be a random or pseudo number, the jti **MUST** contain enough entropy to block brute-force attacks.                                                                                                                                                                    |
| Issue time        | iat          | The timestamp of generating the JWT token, the value of this field **MUST** be validated by the module provider to not be in the future.                                                                                                                                                                                                                                                                                                    |
| Expiration time   | exp          | The "exp" (expiration time) claim identifies the expiration time on or after which the JWT **MUST NOT** be accepted for processing. The value **MUST** be limited to 5 minutes. This value **MUST** be validated by the module provider, any value that exceeds the timeout **MUST** be rejected.                                                                                                                                           |
| Subject           | sub          | This value **MUST** be a person reference to the user executing the launch. This way, applications can understand _who_ is launching the provided `Task`. For example, `Practitioner/82421`                                                                                                                                                                                                                                                 |
| Task              | task         | The FHIR Task object in JSON format.                                                                                                                                                                                                                                                                                                                                                                                                        |
| FHIR Version      | fhir-version | The FHIR version for the provided `Task`. When this field is not provided, the FHIR version **MUST** be the latest stable FHIR release. Consumers should evaluate this field in a case-insensitive manner. Currently, the following fields are allowed: `STU3`, `R4` and `R5`. It is strongly advised to always set this field, even when using the latest stable FHIR version. This prevents HTI breaking after a new FHIR stable release. |

The timestamps follows the ["UNIX time"](https://en.wikipedia.org/wiki/Unix\_time) convention, being the number of seconds since the epoch.

### 3. Sign the JWT&#x20;

After the JWT is completely filled, it must be signed. For this, [Signing the JWT](../connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md) can be followed.

### 4.  Launch

The signed token can now be launched to the client. To find the endpoint to be launched to, the `ActivityDefinition.endpoint` extension can be used. The correct ActivityDefinition can be found using `Task.instantiatesCanonical` (see [resource-ophalen.md](../resources-managen/crud-operaties/resource-ophalen.md "mention")).&#x20;

There are minimal differences between sending via [HTI](hti-launch-versturen.md) and [SHOF](broken-reference).
