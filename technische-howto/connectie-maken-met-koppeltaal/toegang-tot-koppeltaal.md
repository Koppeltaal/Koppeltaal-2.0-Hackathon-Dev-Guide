# Toegang tot Koppeltaal

## Request Flow

Koppeltaal vereist dat middels de [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) flow een `access_token` opgevraagd wordt. Hiervoor wordt het volgende diagram gehanteerd:

![SMART Backend auth flow](<../../.gitbook/assets/backend-service-authorization-diagram (2).png>)

{% hint style="info" %}
De inhoud van de JWT en de OAuth request worden [hier](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#protocol-details) gedetailleerd beschreven. Koppeltaal kent een uitzondering op de `scope` parameter. Deze mag meegestuurd worden maar is niet verplicht en wordt niet verwerkt.
{% endhint %}

### 1. JWT samenstellen

In dit diagram is te zien dat er eerst een JWT token wordt samengesteld en [ondertekend](requirements/jwt-ondertekenen.md). De volgende velden moeten worden gezet:

| Veld | Waarde                                                                                                                                                                                                                                                                                                                                  |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| iss  | Vullen met  de `client_id` verkregen uit [Domein toetreden](../../domeinbeheer/domein-toetreden.md)                                                                                                                                                                                                                                     |
| sub  | Vullen met  de `client_id` verkregen uit [Domein toetreden](../../domeinbeheer/domein-toetreden.md)                                                                                                                                                                                                                                     |
| exp  | UNIX timestamp van nu + 5 minuten                                                                                                                                                                                                                                                                                                       |
| aud  | <p>Vullen met <a href="https://authentication-service.koppeltaal.headease.nl/oauth2/token">https://authentication-service.koppeltaal.headease.nl/oauth2/token</a></p><p>(deze waarde kan altijd uit de <a href="https://hapi-fhir-server.koppeltaal.headease.nl/fhir/metadata"><code>CapabilityStatement</code></a> gehaald worden)</p> |
| jti  | Random identifier, deze wordt gebruikt door de auth server om te verifiëren dat JWT's niet gereplayed worden. Gebruik hier iets als een GUID.                                                                                                                                                                                           |

### 2. Access Token Opvragen

Voer de onderstaande request uit:

{% swagger baseUrl="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/token" method="post" summary="Opvragen van de access_token" %}
{% swagger-description %}
Zie de Response tab voor een voorbeeld-response.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="string" %}
`application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion" type="string" %}
Het ondertekende JWT token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion_type" type="string" %}
Altijd  vullen met 

`urn:ietf:params:oauth:client-assertion-type:jwt-bearer`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="grant_type" type="string" %}
Altijd vullen met 

`client_credentials`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="scope" type="string" %}
Inhoud mag leeg zijn. De scope wordt door de auth server bepaald door het autorisatiemodel
{% endswagger-parameter %}

{% swagger-response status="200" description="De response bevat de access_token, geldigheidsduur in seconden en het type (altijd Bearer)" %}
```javascript
{
  "access_token": "eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJmaGlyLXNlcnZlciIsIm5iZiI6MTYzMTE5NDM0MCwiZXhwIjoxNjMxMTk3OTQwLCJub25jZSI6IjQ4NTI5NTc2LTFiZTctNGNmOS04MWM0LWRkMTVhMjE4NjcwNyIsInR5cGUiOiJhY2Nlc3MiLCJzY29wZSI6IiIsImF6cCI6IjVhZDdjZjZhLTk1NTYtNGQyMy05MWNhLTI1MGRhZmExZGYwOSJ9.cgBzTRhbvLFPug9bqvCtaVi9ogHpMDqqemoTJjA1C3OpMsU42VyrnNUZ41qtcsZfqjI5OspT678MyVhDHq6DDRc9GLbg8RFLjrow17PfBCgkFALCKXWi9r6gTOZdaGdEPKfqavn1r8-S2HnIaWdEVfNPA1ZlBBxkJsYLl-8zgPmykZDNCbIH1e_SevGc56GeF5dPjHzxSiAI2_t19FM0OL3JfLZ-T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```
{% endswagger-response %}

{% swagger-response status="400" description="De aanvraag is verkeerd ingediend. Waarschijnlijk missen er parameters of wordt de body op  een verkeerde manier aangeleverd." %}
```
Bad request
```
{% endswagger-response %}
{% endswagger %}

Zoals hierboven in de `Response` tab te zien is, wordt de `access_token` als onderdeel van de response  meegegeven. Deze  `access_token` moet meegegeven worden als [`Authorization` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) op elke request naar de Koppeltaal server. Het formaat van de header is als volgt:

```
Authorization: <type> <credentials>
```

In de voorbeeld-response zou de header er als volgt uit moeten zien:

```
Authorization: Bearer eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ
```

### Token vernieuwen

In de POC is een `access_token` maar liefst één uur geldig. Wanneer deze tijd verstreken is zal Koppeltaal server een `401` error teruggeven. De [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) kent geen `refresh_token`. De applicatie dient punt 1 & 2 opnieuw uit te voeren.
