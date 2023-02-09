---
description: >-
  If the application wants to create its own ActivityDefinition instead of the
  EHR
---

# Create an ActivityDefinition

Koppeltaal adds three [extensions](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition) to the `ActivityDefinition` resource:

<figure><img src="../../../.gitbook/assets/Screenshot 2023-02-09 at 09.25.51.png" alt=""><figcaption></figcaption></figure>



| Extension       | Description                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| endpoint        | Every `ActivityDefinition` must refer to an `Endpoint` Resource. The value of this `Endpoint` must be filled with the URL that the user is sent to while launching a `Task` that uses this `ActivityDefinition`                                                                                                                                                                                              |
| publisherId     | A unique identifier for the client publishing the `ActivityDefinition`. This can be used to, for example, retrieve all Tasks that refer to any `ActivityDefinition` from publisher _**X**_. This can be very useful for subscribing to changes in `Tasks` that are relevant to your application. This value requires an `id` type value, allowing more freedom of "groups" than the `resource-origin` field. |
| resource-origin | A reference to the `Device` that created the Resource. This is set by the Koppeltaal server as it's used by the [authorisation mechanism](../../../domeinbeheer/rollen-beheren/autorisatiemodel.md). This value cannot be set by the client while creating new Resources.                                                                                                                                    |

### Valid ActivityDefinition

```javascript
{
    "resourceType": "ActivityDefinition",
    "meta": {
        "profile": [
            "http://koppeltaal.nl/fhir/StructureDefinition/KT2ActivityDefinition"
        ]
    },
    "extension": [
        {
            "url": "http://koppeltaal.nl/fhir/StructureDefinition/KT2PublisherIdentifier",
            "valueId": "ID345900-002"
        },
        {
            "url": "http://koppeltaal.nl/fhir/StructureDefinition/KT2EndpointExtension",
            "valueReference": {
                "reference": "Endpoint/04230feb-8cf6-458b-bb46-409430f64701",
                "type": "Endpoint"
            }
        }
    ],
    "url": "http://Testtooling.com/ActivityDefinition/",
    "identifier": [
        {
            "use": "official",
            "system": "http:/vzvz.nl/Testtooling",
            "value": "Controlelijst voortgang"
        }
    ],
    "version": "1.1.0",
    "name": "Controlelijst",
    "title": "Controlelijst voortgang",
    "status": "active",
    "description": "Vul de controlelijst zo goed mogelijk in. Dit kost ongeveer 10 minuten."
}
```
