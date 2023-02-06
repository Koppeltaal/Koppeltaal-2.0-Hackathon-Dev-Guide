---
description: Het opvoeren van een Taak
---

# Use-Case 1: Opvoeren Taak

{% hint style="info" %}
Wanneer er Resources aangemaakt moeten worden die nodig zijn voor het aanmaken van een Task, kan gebruik gemaakt worden van het [EPD](https://poc-epd.koppeltaal.headease.nl). Hier kunnen bijv.`ActivityDefinitions` en `Endpoints` aangemaakt worden. Wel is het belangrijk dat je als applicatie ook [rechten nodig hebt](../../../domain-access/rollen-beheren/autorisatiemodel.md) om deze Resources te zien/gebruiken.
{% endhint %}

{% hint style="warning" %}
Wanneer het EPD gebruikt wordt om Resources aan te maken is het belangrijk om im [simplifier](https://simplifier.net/Koppeltaalv2.0/\~resources?fhirVersion=R4\&sortBy=RankScore\_desc) te kijken naar de verplichte velden van de resources. Het EPD geeft zelf niet aan welke velden verplicht zijn, en geeft geen informatie terug over waarom een Resource niet aangemaakt kon worden.
{% endhint %}

### Taak aanmaken

Een `Task` aanmaken in de Koppeltaal Server werkt net als alle andere Resources: middels de RESTful API.&#x20;

De `Task` resource kan het makkelijkst aangemaakt worden middels een officiële FHIR library. Raadpleeg eerst de [FHIR Downloads pagina](https://hl7.org/fhir/r4/downloads.html) om te kijken of hier een bruikbare library tussen staat. De uiteindelijk uitkomst van de code moet een POST zijn richting de Koppeltaal Server. Meer informatie over het aanmaken van resources is [hier](../../../technische-howto/resources-managen/crud-operaties/resource-aanmaken.md) beschreven.

#### Profiel

De Koppeltaal Server dwingt het [Koppeltaal KT2Task profiel](https://simplifier.net/koppeltaalv2.0/kt2task) af. De `Task` objecten die uitgestuurd worden moeten hier aan voldoen. Per Resource moet ook aangegeven worden middels welk profiel deze is samengesteld. Dit gebeurt in het [Resource.meta.profile](https://www.hl7.org/fhir/resource-definitions.html#Meta.profile) veld.

#### Voorbeeld Task

Dit is een voorbeeld van een valide `Task` payload:

```javascript
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
    "reference": "Patient/774"
  }
}
```
