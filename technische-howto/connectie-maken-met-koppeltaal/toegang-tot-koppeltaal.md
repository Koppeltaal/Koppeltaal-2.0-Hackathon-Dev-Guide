# Toegang tot Koppeltaal

## Request Flow

Koppeltaal vereist dat middels de [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) flow een `access_token` opgevraagd wordt. Hiervoor wordt het volgende diagram gehanteerd:

{% hint style="info" %}

{% endhint %}

![bron: https://hl7.org/fhir/uv/bulkdata/authorization/index.html\#obtaining-an-access-token](../../.gitbook/assets/backend-service-authorization-diagram%20%281%29.png)

{% hint style="info" %}
De inhoud van de JWT en de OAuth request worden [hier](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#protocol-details) gedetailleerd beschreven. Koppeltaal kent een uitzondering op de `scope` parameter. Deze mag meegestuurd worden maar is niet verplicht en wordt niet verwerkt.
{% endhint %}

### 1. JWT samenstellen

In dit diagram is te zien dat er eerst een JWT token wordt samengesteld en [ondertekend](requirements/jwt-ondertekenen.md). De volgende velden moeten worden gezet:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Veld</th>
      <th style="text-align:left">Waarde</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">iss</td>
      <td style="text-align:left">Vullen met de <code>client_id</code> verkregen uit <a href="../../domeinbeheer/domein-toetreden.md">Domein toetreden</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">sub</td>
      <td style="text-align:left">Vullen met de <code>client_id</code> verkregen uit <a href="../../domeinbeheer/domein-toetreden.md">Domein toetreden</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">exp</td>
      <td style="text-align:left">UNIX timestamp van nu + 5 minuten</td>
    </tr>
    <tr>
      <td style="text-align:left">aud</td>
      <td style="text-align:left">
        <p>Vullen met <a href="https://authentication-service.koppeltaal.headease.nl/oauth2/token">https://authentication-service.koppeltaal.headease.nl/oauth2/token</a>
        </p>
        <p>(deze waarde kan altijd uit de <a href="https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata"><code>CapabilityStatement</code></a> gehaald
          worden)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">jti</td>
      <td style="text-align:left">Random identifier, deze wordt gebruikt door de auth server om te verifi&#xEB;ren
        dat JWT&apos;s niet gereplayed worden. Gebruik hier iets als een GUID.</td>
    </tr>
  </tbody>
</table>

### 2. Access Token Opvragen

Voer de onderstaande request uit:

{% api-method method="post" host="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/token" %}
{% api-method-summary %}
Opvragen van de access\_token
{% endapi-method-summary %}

{% api-method-description %}
Zie de Response tab voor een voorbeeld-response.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Type" type="string" required=true %}
`application/x-www-form-urlencoded`
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-form-data-parameters %}
{% api-method-parameter name="client\_assertion" type="string" required=true %}
Het ondertekende JWT token
{% endapi-method-parameter %}

{% api-method-parameter name="client\_assertion\_type" type="string" required=true %}
Altijd  vullen met `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`
{% endapi-method-parameter %}

{% api-method-parameter name="grant\_type" type="string" required=true %}
Altijd vullen met `client_credentials`
{% endapi-method-parameter %}

{% api-method-parameter name="scope" type="string" required=true %}
Inhoud mag leeg zijn. De scope wordt door de auth server bepaald door het autorisatiemodel
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
De response bevat de access\_token, geldigheidsduur in seconden en het type \(altijd Bearer\)
{% endapi-method-response-example-description %}

```javascript
{
  "access_token": "eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJmaGlyLXNlcnZlciIsIm5iZiI6MTYzMTE5NDM0MCwiZXhwIjoxNjMxMTk3OTQwLCJub25jZSI6IjQ4NTI5NTc2LTFiZTctNGNmOS04MWM0LWRkMTVhMjE4NjcwNyIsInR5cGUiOiJhY2Nlc3MiLCJzY29wZSI6IiIsImF6cCI6IjVhZDdjZjZhLTk1NTYtNGQyMy05MWNhLTI1MGRhZmExZGYwOSJ9.cgBzTRhbvLFPug9bqvCtaVi9ogHpMDqqemoTJjA1C3OpMsU42VyrnNUZ41qtcsZfqjI5OspT678MyVhDHq6DDRc9GLbg8RFLjrow17PfBCgkFALCKXWi9r6gTOZdaGdEPKfqavn1r8-S2HnIaWdEVfNPA1ZlBBxkJsYLl-8zgPmykZDNCbIH1e_SevGc56GeF5dPjHzxSiAI2_t19FM0OL3JfLZ-T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
De aanvraag is verkeerd ingediend. Waarschijnlijk missen er parameters of wordt de body op  een verkeerde manier aangeleverd.
{% endapi-method-response-example-description %}

```
Bad request
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Zoals hierboven in de `Response` tab te zien is, wordt de `access_token` als onderdeel van de response  meegegeven. Deze  `access_token` moet meegegeven worden als [`Authorization` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) op elke request naar de Koppeltaal server. Het formaat van de header is als volgt:

```text
Authorization: <type> <credentials>
```

In de voorbeeld-response zou de header er als volgt uit moeten zien:

```text
Authorization: Bearer eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ
```
