# Use-Cases

De hackathon bestaat uit twee use-cases. Deze use-cases concentreren zich op de FHIR `Task` resource. Deze Resource is de kern van Koppeltaal en verwijst naar andere belangrijke Resources. Aan het einde van de hackathon willen we graag de volgende twee use-cases zien werken:

1. Vanuit een behandelarenportaal een `Task` toekennen aan een `Patient` .
2. Vanuit een patiëntenportaal de `Task` uitvoeren.

{% hint style="info" %}
Wanneer er Resources aangemaakt moeten worden die nodig zijn voor het aanmaken van een Task, kan gebruik gemaakt worden van het [EPD](https://poc-epd.koppeltaal.headease.nl). Hier kunnen bijv.`ActivityDefinitions` en `Endpoints` aangemaakt worden. Wel belangrijk is dat je als applicatie ook [rechten nodig hebt](../domeinbeheer/rollen-beheren/autorisatiemodel.md) om deze Resources te zien/gebruiken.
{% endhint %}

### Profiel

De Koppeltaal Server dwingt het [Koppeltaal KT2Task profiel](https://simplifier.net/koppeltaalv2.0/kt2task) af. De `Task` objecten die uitgestuurd worden moeten hier aan voldoen. Per Resource moet ook aangegeven worden middels welk profiel deze is samengesteld. Dit gebeurt in het [Resource.meta.profile](https://www.hl7.org/fhir/resource-definitions.html#Meta.profile) veld.

### Voorbeeld Task

Dit is een voorbeeld van een valide `Task` payload:

```javascript
{
  "resourceType": "Task",
  "meta": {
    "profile": [
      "http://example.org/fhir/StructureDefinition/KT2Task"
    ]
  },
  "identifier": [
    {
      "system": "https://poc-portal.koppeltaal.headease.nl",
      "value": "a4fb3572-e82c-421d-aca3-42c26face61a"
    }
  ],
  "instantiatesCanonical": "https://staging-hapi-fhir-server.koppeltaal.headease.nl/fhir/ActivityDefinition/762/_history/1",
  "status": "ready",
  "intent": "order",
  "for": {
    "reference": "Patient/774"
  }
  "executionPeriod": {
    "start": "2021-09-21T10:50:20+02:00"
  },
  "owner": {
    "reference": "Patient/774"
  }
}
```

