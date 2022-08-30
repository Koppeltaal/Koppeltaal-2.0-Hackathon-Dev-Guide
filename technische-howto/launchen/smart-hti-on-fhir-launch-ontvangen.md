# SHOF Launch Ontvangen

## Requirements

1. De applicatie moet een [`ActivityDefinition`](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition) hebben [aangemaakt](../resources-managen/crud-operaties/resource-aanmaken.md).
2. De gebruiker die de launch uitvoert moet een account hebben op de gedeelde IdP

## Flow

![bron: https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md](<../../.gitbook/assets/image (1).png>)

1. Bij binnenkomst van de launch wordt de conformance opgehaald bij de Koppeltaal Server. Hier kan de [authorize & token URL](smart-hti-on-fhir-launch-ontvangen.md#token-and-authorize-url-metadata) opgevraagd worden.
2. Er wordt een [redirect gestuurd](smart-hti-on-fhir-launch-ontvangen.md#authorize-request) naar de authorize URL. De auth server zal de browser redirecten naar een gedeelde IdP middels een OIDC code flow. Als het launcerende platform geen gebruik maakt van dit gedeelde IdP zal de gebruiker nog een keer moet inloggen. Bij de POC is het vanaf dit login scherm direct mogelijk om een account aan te maken. Nieuwe gebruikers krijgen by default de rol `patient`. De authorize call zal kijken of de ingelogde gebruiker overeenkomt met de gebruiker uit het launch token.
3. Een succesvolle authorize call geeft de `code` & `state` parameters terug aan de `redirect_url`. Let er op dat de state waarde gekoppeld wordt aan de `redirect_uri`, deze moet in de volgende stap namelijk weer meegegeven worden.
4. Voer vanuit de back-end de [Get Token request](smart-hti-on-fhir-launch-ontvangen.md#get-token) uit. Hierbij wordt de`code` omgeruild voor:
   1. Een `id_token` (bevat informatie  over de gebruiker als `JWT`).
   2. Een no-op `access_token` (niet te gebruiken op  de Koppeltaal Server omdat deze user-specific is).
   3. Additionele context velden zoals `task`, deze vult de auth server a.d.h.v. het JWT token die als `launch` param is meegegeven.

A.d.h.v. het context object kan bepaald worden wie met welke rol ingelogd is op het systeem en welke taak geopend moet worden. Wanneer er een valide response komt op deze request, is de user te authenticeren en kan er bijv. een sessie aangemaakt worden voor de gebruiker.

{% swagger baseUrl="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/authorize" method="get" summary="Authorize Request" %}
{% swagger-description %}
De URL moet bepaald worden a.d.h.v. de 

`CapabilityStatement`
{% endswagger-description %}

{% swagger-parameter in="query" name="aud" type="string" %}
URL van de Koppeltaal Server (zelfde als de launch 

`iss`

 value)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="scope" type="string" %}
Altijd: 

`launch openid`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state" type="string" %}
Een opaque waarde die door de client wordt gebruikt om de status tussen de request en de callback te behouden. De autorisatieserver neemt deze waarde op bij het redirecten van de user-agent terug naar de client. De parameter MOET worden gebruikt voor het voorkomen van cross-site request forgery (CSRF) aanvallen of sessiefixatie.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="launch" type="string" %}
Het HTI token (deze komt binnen via delaunch param)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="redirect_uri" type="string" %}
De URL waar de code naartoe teruggestuurd moet worden
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_id" type="string" %}
De client_id uit Domeinbeheer
{% endswagger-parameter %}

{% swagger-parameter in="query" name="response_type" type="string" %}
Altijd: 

`code`
{% endswagger-parameter %}

{% swagger-response status="302" description="The Location header will redirect with a code and the state" %}
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

{% swagger-parameter in="header" name="Content-Type" %}
application/x-www-form-urlencoded
{% endswagger-parameter %}

{% swagger-parameter in="body" name="redirect_uri" type="string" %}
Dezelfde 

`redirect_uri`

 die gebruikt is bij de authorization request
{% endswagger-parameter %}

{% swagger-parameter in="body" name="code" type="string" %}
Code die meegegeven wordt op de redirect
{% endswagger-parameter %}

{% swagger-parameter in="body" name="grant_type" type="string" %}
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
