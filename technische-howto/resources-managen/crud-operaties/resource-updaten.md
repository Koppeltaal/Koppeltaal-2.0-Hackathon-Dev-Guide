# Resource Updaten

{% hint style="danger" %}
TODO: If-Match discussie verwerken
{% endhint %}

### Concurrency

Om te voorkomen dat er data wordt overschreven, moet een applicatie altijd aangeven op basis van welke versie de`Resource` geüpdate wordt. Dit wordt [gedaan](https://www.hl7.org/fhir/http.html#concurrency) middels de `If-Match` header. Wanneer de update gebaseerd  is op een te oude versie zal de server met een `409 Conflict` responden. De `If-Match` value moet overeenkomen de `ETag` value. De `ETag` is een response header die na een [Create](resource-aanmaken.md) of [Get](resource-ophalen.md) meegegeven wordt door de Koppeltaal server.

{% api-method method="put" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" %}
{% api-method-summary %}
Complete resource
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
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
De resource
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



