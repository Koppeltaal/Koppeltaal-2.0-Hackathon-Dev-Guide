# Resource Aanmaken

{% hint style="info" %}
Zie de [FHIR documentatie](https://www.hl7.org/fhir/http.html#create) voor meer informatie.
{% endhint %}

### Advies

Koppeltaal adviseert om gebruik te maken van [logische referenties](https://www.hl7.org/fhir/references.html#logical) op alle objecten. De voornaamste reden hiervoor is dat een bronsysteem consistent kan bijhouden of er al een Koppeltaal-variant van het object bestaat. Daarnaast kunnen logische referenties helpen wanneer er meerdere bronsystemen zijn die moeten weten of een `Resource` al bestaat.

### Conditional Create

{% hint style="warning" %}
De conditional create zit nog in de "trial use" fase. De status van deze functionaliteit moet dus nog gereviewed worden. Binnen Koppeltaal is momenteel nog niet besloten welke create-variant de voorkeur betreft.
{% endhint %}

De FHIR specificatie beschrijft [conditional creates](https://www.hl7.org/fhir/http.html#ccreate). Wanneer een `Resource` aangemaakt wordt, kan er een `upsert` uitgevoerd worden a.d.h.v. de logische referentie. Wanneer meerdere applicaties in een domein dezelfde type `Resources` aanmaken, is het belangrijk dat er duidelijke afspraken zijn over welke identifier system gebruikt wordt. De conditional create helpt voorkomen dat er dubbele resources bij Koppeltaal aangemaakt worden.

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" method="post" summary="Conditional Create Request" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter name="If-None-Exist" type="string" required="true" in="header" %}
De logical identifier, bijv:

`identifier=http://my-lab-system|123`
{% endswagger-parameter %}

{% swagger-parameter name="Authorization" type="string" required="true" in="header" %}
Het 

`access_token`
{% endswagger-parameter %}

{% swagger-parameter name="" type="object" required="true" in="body" %}
Resource
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="201" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="412" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://hapi-fhir-server.koppeltaal.headease.nl/fhir" path="/<Resource>" method="post" summary="Create Request" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter name="Authorization" type="string" required="true" in="header" %}
Het

`access_token`
{% endswagger-parameter %}

{% swagger-parameter name="" type="object" required="true" in="body" %}
Resource
{% endswagger-parameter %}

{% swagger-response status="201" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="422" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}
