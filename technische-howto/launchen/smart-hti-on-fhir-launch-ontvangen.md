# SHOF Launch Ontvangen



### Requirements

1. De applicatie moet een [`ActivityDefinition`](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition) hebben [aangemaakt](../resources-managen/crud-operaties/resource-aanmaken.md).

### Flow

![bron: https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md](../../.gitbook/assets/image%20%281%29.png)

1. Bij binnenkomst van de launch wordt de conformance opgehaald bij de Koppeltaal Server. Hier kan de [authorize & token URL](smart-hti-on-fhir-launch-ontvangen.md#token-url-metadata) opgevraagd worden.
2. Er wordt een [redirect gestuurd](smart-hti-on-fhir-launch-ontvangen.md#authorize-request) naar de authorize URL met de onderstaande parameters. Dit geeft de `code` & `state` parameters terug aan de `redirect_url`.

3. De token URL wordt als `POST` uitgevoerd. Hierbij is de volgende data nodig:

   | Parameters |  |  |
      | :--- | :--- | :--- |
   | `grant_type` | required | Fixed value: `authorization_code` |
   | `code` | required | Code die meegegeven wordt op de redirect |
   | `redirect_uri` | required | Dezelfde redirect\_uri die gebruikt is bij de authorization request |


Hierbij wordt  de`code` omgeruild voor:

1. Een `id_token` \(bevat informatie  over de gebruiker\).
2. Een no-op `access_token` \(niet te gebruiken op  de Koppeltaal Server omdat deze user-specific is\).
3. Een `context` object, deze vult de auth server a.d.h.v. het JWT token die als `launch` param is meegegeven.

A.d.h.v. het context object kan bepaald worden wie met welke rol ingelogd is op het systeem en welke  taak geopend moet worden.

### Token & Authorize URL - Metadata

De Token URL kan gevonden worden in de Koppeltaal Server `CapabilityStatement` [https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata](https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata):

![](../../.gitbook/assets/screenshot-2021-09-22-at-21.11.54.png)

{% api-method method="get" host="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/authorize" %}
{% api-method-summary %}
Authorize Request
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter type="string" name="aud" required=true %}
URL van de Koppeltaal Server \(zelfde als de launch `iss` value\)
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="scope" required=true %}
Altijd: `launch openid`
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="state" required=true %}
Een opaque waarde die door de client wordt gebruikt om de status tussen de request en de callback te behouden. De autorisatieserver neemt deze waarde op bij het redirecten van de user-agent terug naar de client. De parameter MOET worden gebruikt voor het voorkomen van cross-site request forgery \(CSRF\) aanvallen of sessiefixatie.
{% endapi-method-parameter %}

{% api-method-parameter name="launch" required=true type="string" %}
Het HTI token \(deze komt binnen via delaunch param\)
{% endapi-method-parameter %}

{% api-method-parameter name="redirect\_uri" type="string" required=true %}
De URL waar de code naartoe teruggestuurd moet worden
{% endapi-method-parameter %}

{% api-method-parameter name="client\_id" type="string" required=true %}
De client\_id uit Domeinbeheer
{% endapi-method-parameter %}

{% api-method-parameter name="response\_type" type="string" required=true %}
Altijd: `code`
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
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

