# SHOF Launch Ontvangen

## Requirements

1. De applicatie moet een [`ActivityDefinition`](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition) hebben [aangemaakt](../resources-managen/crud-operaties/resource-aanmaken.md).

## Flow

![bron: https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md](<../../.gitbook/assets/image (1).png>)

1. Bij binnenkomst van de launch wordt de conformance opgehaald bij de Koppeltaal Server. Hier kan de [authorize & token URL](smart-hti-on-fhir-launch-ontvangen.md#token-and-authorize-url-metadata) opgevraagd worden.
2. Er wordt een [redirect gestuurd](smart-hti-on-fhir-launch-ontvangen.md#authorize-request) naar de authorize URL. Dit geeft de `code` & `state` parameters terug aan de `redirect_url`. Let er op dat de state waarde gekoppeld wordt aan de `redirect_uri`, deze moet in de volgende stap namelijk weer meegegeven worden.
3. Voer vanuit de back-end de [Get Token request](smart-hti-on-fhir-launch-ontvangen.md#get-token) uit. Hierbij wordt de`code` omgeruild voor:
   1. Een `id_token` (bevat informatie over de gebruiker als `JWT`).
   2. Een no-op `access_token` (niet te gebruiken op de Koppeltaal Server omdat deze user-specific is).
   3. Additionele context velden zoals `task`, deze vult de auth server a.d.h.v. het JWT token die als `launch` param is meegegeven.

A.d.h.v. het context object kan bepaald worden wie met welke rol ingelogd is op het systeem en welke taak geopend moet worden. Wanneer er een valide response komt op deze request, is de user te authenticeren en kan er bijv. een sessie aangemaakt worden voor de gebruiker.

{% swagger baseUrl="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/authorize" method="get" summary="Authorize Request" %}
{% swagger-description %}
De URL moet bepaald worden a.d.h.v. de 

`CapabilityStatement`
{% endswagger-description %}

{% swagger-parameter type="string" name="aud" required="true" in="query" %}
URL van de Koppeltaal Server (zelfde als de launch 

`iss`

 value)
{% endswagger-parameter %}

{% swagger-parameter type="string" name="scope" required="true" in="query" %}
Altijd: 

`launch openid`
{% endswagger-parameter %}

{% swagger-parameter type="string" name="state" required="true" in="query" %}
Een opaque waarde die door de client wordt gebruikt om de status tussen de request en de callback te behouden. De autorisatieserver neemt deze waarde op bij het redirecten van de user-agent terug naar de client. De parameter MOET worden gebruikt voor het voorkomen van cross-site request forgery (CSRF) aanvallen of sessiefixatie.
{% endswagger-parameter %}

{% swagger-parameter name="launch" required="true" type="string" in="query" %}
Het HTI token (deze komt binnen via delaunch param)
{% endswagger-parameter %}

{% swagger-parameter name="redirect:uri" type="string" required="true" in="query" %}
De URL waar de code naartoe teruggestuurd moet worden
{% endswagger-parameter %}

{% swagger-parameter name="client:id" type="string" required="true" in="query" %}
De client_id uit Domeinbeheer
{% endswagger-parameter %}

{% swagger-parameter name="response:type" type="string" required="true" in="query" %}
Altijd: 

`code`
{% endswagger-parameter %}

{% swagger-response status="302" description="" %}
```
Loocation: https://launch-testsuite.koppeltaal.headease.nl/module_authentication_shof?code=05b542e2-6206-449b-924b-99d39168029b&state=99a8cf9a-28c2-4867-8123-486c04003482
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/token" method="post" summary="Get Token" %}
{% swagger-description %}
De URL moet bepaald worden a.d.h.v. de 

`CapabilityStatement`
{% endswagger-description %}

{% swagger-parameter name="Content-Type" required="true" in="header" %}
application/x-www-form-urlencoded
{% endswagger-parameter %}

{% swagger-parameter type="string" name="redirect:uri" required="true" in="body" %}
Dezelfde 

`redirect_uri`

 die gebruikt is bij de authorization request
{% endswagger-parameter %}

{% swagger-parameter type="string" name="code" required="true" in="body" %}
Code die meegegeven wordt op de redirect
{% endswagger-parameter %}

{% swagger-parameter name="grant:type" required="true" type="string" in="body" %}
Altijd: 

`authorization_code`
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "access_token": "NOOP",
  "refresh_token": "NOOP",
  "token_type": "Bearer",
  "expires_in": "3600",
  "patient": "Patient/1963",
  "practitioner": null,
  "task": "Task/1961",
  "relatedperson": null,
  "activity": "https://hapi-fhir-server.koppeltaal.headease.nl/fhir/ActivityDefinition/1959",
  "id_token": "eyJraWQiOiJvUDQ4NmxJcmwzRFFQdXF0dVVqUmxqc01oWFA0alJmdXoxS19uX0dpQmRrIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJiMDJkNmVhNi1iMWEyLTRjZDQtODJmNS1iNjQyM2Q2NmE5ODgiLCJuYmYiOjE2MzI4MTMzNTAsImV4cCI6MTYzMjgxNjk1MCwibm9uY2UiOiJmNGMxODZlNy1jMzI2LTQxODAtYjFmMi1jYTllMWI4YTgyYWQiLCJzdWIiOiJQYXRpZW50LzE5NjMiLCJhenAiOiJiMDJkNmVhNi1iMWEyLTRjZDQtODJmNS1iNjQyM2Q2NmE5ODgifQ.UfBtTACLOhsCMr4Tlen3RUFek06WgWc-aaTPQzJzmHVGYBLY3CnJXTLI1FfCzp1ChM3vx-e2jbFCDHak6ennsuitki-1HnrZitTKpG8qKZK_f24gwVFM5LmzdUXtuTszJSeulpRG8zmNI96pqaIW4ru995LwhKLd-XSOY02BbAMo4XZ46ZW8DBXnhr32CI9TUza8NEQoxlQAF8EboUhro5vauPrjdshP3jQFUNSs5NceB4er3RnF10Zd6SiLFP-_c2ynaj_v87fJEgVGw63byYcKm6O3bTW2KsSz_YNYDYv8DWjYAp25P79e-Hlc3ERcybhLnLy0_-Rkvjk5P_240g"
}
```
{% endswagger-response %}
{% endswagger %}

## Token & Authorize URL - Metadata

De Token URL kan gevonden worden in de Koppeltaal Server `CapabilityStatement` [https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata](https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata):

![](../../.gitbook/assets/screenshot-2021-09-22-at-21.11.54.png)
