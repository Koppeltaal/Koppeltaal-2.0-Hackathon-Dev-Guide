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
| Issuer | iss | De `client_id` van het portaal |
| Audience | aud | De `URL` van de module provider |
| Unique message id | jti | Een unieke identificatie voor dit bericht. Deze waarde MOET worden behandeld als een NONCE, een volgend bericht met een identieke jti MOET worden afgewezen. De jti-waarde moet een willekeurig of pseudo-getal zijn, de `jti` MOET voldoende entropie bevatten om brute-force-aanvallen te blokkeren. |
| Issue time | iat | De timestamp van het genereren van het JWT-token. De waarde van dit veld MOET worden gevalideerd door de module provider om niet in de toekomst te zijn. |
| Expiration time | exp | Deze waarde MOET de time-out zijn van de uitwisseling die het naar de klant verzendt plus de time-out van de uitwisseling die door de klant wordt gebruikt om het te verzenden, de waarde MOET beperkt zijn tot 5 minuten. Deze waarde MOET worden gevalideerd door de module provider, elke waarde die de time-out overschrijdt, MOET worden afgewezen. |
| Subject | sub | Deze waarde MOET een persoonsreferentie zijn naar de gebruiker die de lancering uitvoert. Op deze manier kunnen toepassingen begrijpen wie de opgegeven taak start. Bijvoorbeeld `Practitioner/82421` |
| Task | task | De FHIR Task in JSON formaat. |
| FHIR Version | fhir-version | Dit veld hoeft niet meegegeven te worden omdat Koppeltaal FHIR `R4` gebruikt, dit is de default in HTI. |

The timestamps follows the ["UNIX time"](https://en.wikipedia.org/wiki/Unix_time) convention, being the number of seconds since the epoch.

### 3. JWT Ondertekenen

Nadat de JWT volledig gevuld is, moet deze worden ondertekend. Hiervoor kan [JWT Ondertekenen](../../connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md) gevolgd worden.

### 4.  Launchen

Het ondertekende token kan nu gelaunched worden naar de client. Om de endpoint te vinden waarheen gelaunched moet worden kan gebruik gemaakt worden van de `ActivityDefinition.endpoint` extensie. De juiste `ActivityDefinition` kan gevonden worden a.d.h.v. `Task.instantiatesCanonical` \(zie [Resource Ophalen](../../resources-managen/crud-operaties/resource-ophalen.md)\).

Er zijn minimale verschillen tussen het versturen via [HTI](hti-launch-versturen.md) en [SHOF](shof-launch-versturen.md).

