# Resource Updaten

{% hint style="danger" %}
TODO: If-Match discussie verwerken  - afdwingen of niet
{% endhint %}

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#update) voor meer informatie.
{% endhint %}

### Concurrency

Om te voorkomen dat er data wordt overschreven, moet een applicatie altijd aangeven op basis van welke versie de`Resource` geüpdate wordt. Dit wordt [gedaan](https://www.hl7.org/fhir/http.html#concurrency) middels de `If-Match` header. Wanneer de update gebaseerd is op een te oude versie zal de server met een `409 Conflict` responden. De `If-Match` value moet overeenkomen de `ETag` value. De `ETag` is een response header die na een [Create](resource-aanmaken.md) of [Get](resource-ophalen.md) meegegeven wordt door de Koppeltaal server.

{% api-method method="put" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>" %}
{% api-method-summary %}
Complete resource
{% endapi-method-summary %}

{% api-method-description %}
De logische id moet ook aanwezig zijn in de `Resource` zelf.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
De "logical id" van de `Resource`
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="If-Match" type="string" required=true %}
De versie waarop de update toegepast is, bijv: W/"23"
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
De access\_token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="" type="string" required=false %}
De `Resource`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Resource is aangepast. De resource met resource-origin extensie en logical id wordt teruggegeven
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

{% api-method-response-example httpCode=405 %}
{% api-method-response-example-description %}
De Resource bestaat niet \(a.d.h.v. de logische id\)
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
Change gebaseerd op een oude versie
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

### Delen van een Resource Updaten

Om middels een kleine payload een  resource  te updaten kan gebruik gemaakt worden van een  `PATCH` request. De payload van de `PATCH` moet een van de volgende zijn:

1. Een [JSON Patch](https://datatracker.ietf.org/doc/html/rfc6902)  \(Content-Type application/json-patch+json\)
2. Een [XML Patch](https://datatracker.ietf.org/doc/html/rfc5261)  \(Content-Type application/xml-patch+xml\)
3. Een [FHIRPath Patch](https://www.hl7.org/fhir/fhirpatch.html) parameters Resource  \(Content-Type [FHIR Content Type](https://www.hl7.org/fhir/http.html#mime-type)\)

Zo ziet de payload er uit van een JSON Patch om de status te updaten van een `Task`:

```text
[{
    "op": "replace",
    "path": "/status",
    "value": "completed"
}]
```

Voorbeelden van meer type patches kunnen [hier](https://www.hl7.org/fhir/test-cases.zip) gedownload worden.

{% api-method method="patch" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>" %}
{% api-method-summary %}
Deel van een Resource Updaten
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter required=true %}
De "logical id" van de `Resource`
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="If-Match" required=true %}
De versie waarop de update toegepast is, bijv: W/"23"
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="" type="object" required=true %}
Een JSON Patch object
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

