# Use-Case 1: Create a Task

{% hint style="info" %}
When referenced Resources need to be created that are required for Task creation (e.g. `ActivityDefinition` or `Endpoint`), the [EHR](https://poc-epd.koppeltaal.headease.nl/) can be used. As the Resources are created by a different application, make sure that your application has the [rights](../../../domeinbeheer/rollen-beheren/autorisatiemodel.md) to see/use these Resources.
{% endhint %}

{% hint style="warning" %}
When using the EHR to create Resources, it is important to look in simplifier for the mandatory fields of the Resources. The EHR itself does not indicate which fields are mandatory, and does not return information about why a Resource could not be created.
{% endhint %}

### Task creation

Creating a `Task` in the Koppeltaal server works just like any other Resource: using the RESTful API.&#x20;

The `Task` resource can most easily be created using an official FHIR library. First consult the [FHIR Downloads page](https://hl7.org/fhir/r4/downloads.html) to see if you can find a useful library. The final output of the code should be a `POST` request to the Koppeltaal server. More information on creating resources is described [here](../../../technische-howto/resources-managen/crud-operaties/resource-aanmaken.md).

#### Profile

De Koppeltaal Server dwingt het [Koppeltaal KT2Task profiel](https://simplifier.net/koppeltaalv2.0/kt2task) af. De `Task` objecten die uitgestuurd worden moeten hier aan voldoen. Per Resource moet ook aangegeven worden middels welk profiel deze is samengesteld. Dit gebeurt in het [Resource.meta.profile](https://www.hl7.org/fhir/resource-definitions.html#Meta.profile) veld.

The Koppeltaal server enforces compliance with the [Koppeltaal KT2Task profile](https://simplifier.net/koppeltaalv2.0/kt2task). The `Task` Resource must indicate that it's using the KT2Task profile. This is done in the `Resource.meta.profile` field.

#### Example Task

```json
{
  "resourceType": "Task",
  "meta": {
    "profile": [
      "http://koppeltaal.nl/fhir/StructureDefinition/KT2Task"
    ]
  },
  "identifier": [
    {
      "system": "https://poc-portal.koppeltaal.headease.nl",
      "value": "a4fb3572-e82c-421d-aca3-42c26face61a"
    }
  ],
  "instantiatesCanonical": "https://hapi-fhir-server.koppeltaal.headease.nl/fhir/ActivityDefinition/762/_history/1",
  "status": "ready",
  "intent": "order",
  "executionPeriod": {
    "start": "2021-09-21T10:50:20+02:00"
  },
  "owner": {
    "reference": "Patient/00216d69-9c36-4c42-8837-8ee1bbc9c926"
  }
}
```
