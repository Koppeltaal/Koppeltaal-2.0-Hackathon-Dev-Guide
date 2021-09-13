# Resource Aanmaken

### Requirement

Binnen Koppeltaal dient er gebruik gemaakt te worden van [logische referenties](https://www.hl7.org/fhir/references.html#logical) op alle objecten. De voornaamste reden hiervoor is dat een bronsysteem consistent kan bijhouden of er al een Koppeltaal-variant van het object bestaat. Daarnaast kunnen logische referenties helpen wanneer er meerdere bronsystemen zijn die moeten weten of een `Resource` al bestaat.

### Conditional Create

De FHIR specificatie beschrijft [conditional creates](https://www.hl7.org/fhir/http.html#ccreate). Wanneer een `Resource` aangemaakt wordt, dient deze gebruikt te worden i.c.m. de logische referentie. Wanneer meerdere applicaties in een domein dezelfde type `Resources` aanmaken, is het belangrijk dat er duidelijke afspraken zijn over welke identifier system gebruikt wordt.

{% api-method method="post" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>/<id>" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="If-None-Exist" type="string" required=true %}
The logical identifier
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}

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

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

