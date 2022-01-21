---
description: Haalt alle resources op voor één specifieke Resource-type
---

# Alle Resources Ophalen

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#read) voor meer informatie.
{% endhint %}

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" method="get" summary="Alle Resources Ophalen" %}
{% swagger-description %}
Get ALL voor type <Resource>.
{% endswagger-description %}

{% swagger-parameter name="Resource" type="string" required="true" in="path" %}
Resource van de resource list
{% endswagger-parameter %}

{% swagger-parameter name="Authentication" type="string" required="true" in="header" %}
Authentication token verkregen via de Auth Server

\


(zie Connectie maken met Koppeltaal)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
Teruggegeven resultaten kunnen gefilterd zijn i.v.m. [Search Narrowing](../../../domeinbeheer/rollen-beheren/search-narrowing.md).
{% endhint %}
