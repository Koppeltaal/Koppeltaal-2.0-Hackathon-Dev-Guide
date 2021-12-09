# Resource Aanmaken

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#create) voor meer informatie.
{% endhint %}

### Advies

Koppeltaal adviseert om gebruik te maken van [logische referenties](https://www.hl7.org/fhir/references.html#logical) op alle objecten. De voornaamste reden hiervoor is dat een bronsysteem consistent kan bijhouden of er al een Koppeltaal-variant van het object bestaat. Daarnaast kunnen logische referenties helpen wanneer er meerdere bronsystemen zijn die moeten weten of een `Resource` al bestaat.

### Koppeltaal Profielen

De Koppeltaal Server valideert alle binnenkomende resources. Ook dwingt de server af dat de resources conform de [Koppeltaal profielen](https://simplifier.net/Koppeltaalv2.0/\~resources?category=Profile\&fhirVersion=R4\&sortBy=RankScore\_desc) worden verstuurd. Profielen worden in FHIR opgeslagen als [StructureDefinition](http://www.hl7.org/FHIR/structuredefinition.html) resource. Om aan te geven dat een resource conform een profiel werkt kan de `Resource.meta.profiles` array gebruikt worden. De waarde moet altijd gevuld worden met de canonical identifier van het profiel. Deze kan hier gevonden worden:

![Canonical identifier ophalen bij simplifier.net](<../../../.gitbook/assets/Screenshot 2021-12-09 at 10.42.16.png>)

Bijvoorbeeld:

```json
{
  "resourceType": "Subscription",
  "meta": {
    "profile": [
      "http://koppeltaal.nl/fhir/StructureDefinition/KT2Subscription"
    ]
  }
  ...
  }
}
```

### Conditional Create

{% hint style="warning" %}
De conditional create zit nog in de "trial use" fase. De status van deze functionaliteit moet dus nog gereviewed worden. Binnen Koppeltaal is momenteel nog niet besloten welke create-variant de voorkeur betreft.
{% endhint %}

De FHIR specificatie beschrijft [conditional creates](https://www.hl7.org/fhir/http.html#ccreate). Wanneer een `Resource` aangemaakt wordt, kan er een `upsert` uitgevoerd worden a.d.h.v. de logische referentie. Wanneer meerdere applicaties in een domein dezelfde type `Resources` aanmaken, is het belangrijk dat er duidelijke afspraken zijn over welke identifier system gebruikt wordt. De conditional create helpt voorkomen dat er dubbele resources bij Koppeltaal aangemaakt worden.&#x20;

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" method="post" summary="Conditional Create Request" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="If-None-Exist" type="string" %}
De logical identifier, bijv:

`identifier=http://my-lab-system|123`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Het 

`access_token`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="object" %}
Resource
{% endswagger-parameter %}

{% swagger-response status="200" description="De resource bestond al. De POST wordt niet verwerkt." %}
```
```
{% endswagger-response %}

{% swagger-response status="201" description="Resource is aangemaakt. De resource metresource-origin extensie en id wordt teruggegeven." %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="De resource kan niet geparsed worden of comformeert niet aan de basis FHIR validatie regels" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="Resource type wordt niet ondersteund, of geen FHIR-endpoint" %}
```
```
{% endswagger-response %}

{% swagger-response status="412" description="Meer dan één match gevonden. De If-None-Exist is niet selectief genoeg." %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="Voldoet niet aan de FHIR profielen of Koppeltaal business regels" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" method="post" summary="Create Request" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Het

`access_token`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="object" %}
Resource
{% endswagger-parameter %}

{% swagger-response status="201" description="Resource is aangemaakt. De resource metresource-origin extensie en id wordt teruggegeven." %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="De resource kan niet geparsed worden of comformeert niet aan de basis FHIR validatie regels" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="Resource type wordt niet ondersteund, of geen FHIR-endpoint" %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="Voldoet niet aan de FHIR profielen of Koppeltaal business regels" %}
```
```
{% endswagger-response %}
{% endswagger %}
