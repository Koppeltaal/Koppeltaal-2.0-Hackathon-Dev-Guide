# Resource Aanmaken

### Advies

Koppeltaal adviseert om gebruik te maken van [logische referenties](https://www.hl7.org/fhir/references.html#logical) op alle objecten. De voornaamste reden hiervoor is dat een bronsysteem consistent kan bijhouden of er al een Koppeltaal-variant van het object bestaat. Daarnaast kunnen logische referenties helpen wanneer er meerdere bronsystemen zijn die moeten weten of een `Resource` al bestaat.

### Conditional Create

{% hint style="warning" %}
De conditional create zit nog in de "trial use" fase. De status van deze functionaliteit moet dus nog gereviewed worden.
{% endhint %}

De FHIR specificatie beschrijft [conditional creates](https://www.hl7.org/fhir/http.html#ccreate). Wanneer een `Resource` aangemaakt wordt, kan er een `upsert` uitgevoerd worden a.d.h.v. de logische referentie. Wanneer meerdere applicaties in een domein dezelfde type `Resources` aanmaken, is het belangrijk dat er duidelijke afspraken zijn over welke identifier system gebruikt wordt. De conditional create helpt voorkomen dat er dubbele resources bij Koppeltaal aangemaakt worden. 

{% api-method method="post" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" %}
{% api-method-summary %}
Conditional Create Request
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="If-None-Exist" type="string" required=true %}
De logical identifier, bijv:`identifier=http://my-lab-system|123`
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
Het `access_token`
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="" type="object" required=true %}
Resource
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
De resource bestond al. De POST wordt niet verwerkt.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
Resource is aangemaakt. De resource met`resource-origin` extensie en `id` wordt teruggegeven.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
De resource kan niet geparsed worden of comformeert niet aan de basis FHIR validatie regels
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Resource type wordt niet ondersteund, of geen FHIR-endpoint
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=412 %}
{% api-method-response-example-description %}
Meer dan één match gevonden. De `If-None-Exist` is niet selectief genoeg.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}
Voldoet niet aan de FHIR profielen of Koppeltaal business regels
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" %}
{% api-method-summary %}
Create Request
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Het`access_token`
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="" type="object" required=true %}
Resource
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
Resource is aangemaakt. De resource met`resource-origin` extensie en `id` wordt teruggegeven.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
De resource kan niet geparsed worden of comformeert niet aan de basis FHIR validatie regels
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Resource type wordt niet ondersteund, of geen FHIR-endpoint
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}
Voldoet niet aan de FHIR profielen of Koppeltaal business regels
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

