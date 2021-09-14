---
description: Haalt alle resources op voor één specifieke Resource-type
---

# Alle Resources Ophalen

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#read) voor meer informatie.
{% endhint %}

{% api-method method="get" host="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" %}
{% api-method-summary %}
Alle Resources Ophalen
{% endapi-method-summary %}

{% api-method-description %}
Get ALL voor type &lt;Resource&gt;. 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Resource" type="string" required=true %}
Resource van de resource list
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token verkregen via de Auth Server   
\(zie Connectie maken met Koppeltaal\)
{% endapi-method-parameter %}
{% endapi-method-headers %}
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

{% hint style="info" %}
Teruggegeven resultaten kunnen gefilterd zijn i.v.m. [Search Narrowing](../../../domeinbeheer/rollen-beheren/search-narrowing.md).
{% endhint %}

