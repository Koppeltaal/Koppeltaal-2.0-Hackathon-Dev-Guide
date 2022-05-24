# Resource Updaten

{% hint style="danger" %}
TODO: If-Match discussie verwerken  - afdwingen of niet
{% endhint %}

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#update) voor meer informatie.
{% endhint %}

### Concurrency

Om te voorkomen dat er data wordt overschreven, moet een applicatie altijd aangeven op basis van welke versie de`Resource` geüpdate wordt. Dit wordt [gedaan](https://www.hl7.org/fhir/http.html#concurrency) middels de `If-Match` header. Wanneer de update gebaseerd is op een te oude versie zal de server met een `409 Conflict` responden. De `If-Match` value moet overeenkomen de `ETag` value. De `ETag` is een response header die na een [Create](resource-aanmaken.md) of [Get](resource-ophalen.md) meegegeven wordt door de Koppeltaal server.

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>" method="put" summary="Complete resource" %}
{% swagger-description %}
De logische id moet ook aanwezig zijn in de 

`Resource`

 zelf.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="string" %}
De "logical id" van de 

`Resource`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="If-Match" type="string" %}
De versie waarop de update toegepast is, bijv: W/"23"
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
De access_token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="string" %}
De 

`Resource`
{% endswagger-parameter %}

{% swagger-response status="200" description="Resource is aangepast. De resource met resource-origin extensie en logical id wordt teruggegeven" %}
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

{% swagger-response status="405" description="De Resource bestaat niet (a.d.h.v. de logische id)" %}
```
```
{% endswagger-response %}

{% swagger-response status="409" description="Change gebaseerd op een oude versie" %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="Voldoet niet aan de FHIR profielen of Koppeltaal business regels" %}
```
```
{% endswagger-response %}
{% endswagger %}

### Delen van een Resource Updaten

Om middels een kleine payload een  resource  te updaten kan gebruik gemaakt worden van een  `PATCH` request. De payload van de `PATCH` moet een van de volgende zijn:

1. Een [JSON Patch](https://datatracker.ietf.org/doc/html/rfc6902)  (Content-Type application/json-patch+json)
2. Een [XML Patch](https://datatracker.ietf.org/doc/html/rfc5261)  (Content-Type application/xml-patch+xml)
3. Een [FHIRPath Patch](https://www.hl7.org/fhir/fhirpatch.html) parameters Resource  (Content-Type [FHIR Content Type](https://www.hl7.org/fhir/http.html#mime-type))

Zo ziet de payload er uit van een JSON Patch om de status te updaten van een `Task`:

```
[{
    "op": "replace",
    "path": "/status",
    "value": "completed"
}]
```

Voorbeelden van meer type patches kunnen [hier](https://www.hl7.org/fhir/test-cases.zip) gedownload worden.

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<:id>" method="patch" summary="Deel van een Resource Updaten" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" %}
De "logical id" van de 

`Resource`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="If-Match" type="string" %}
De versie waarop de update toegepast is, bijv: W/"23"
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="" type="object" %}
De Patch
{% endswagger-parameter %}

{% swagger-response status="200" description="Patch is toegepast. De complete Resource wordt teruggegeven" %}
```
```
{% endswagger-response %}
{% endswagger %}
