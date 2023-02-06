# Access to Koppeltaal

## Request Flow

Koppeltaal vereist dat middels de [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) flow een `access_token` opgevraagd wordt. Hiervoor wordt het volgende diagram gehanteerd:

Koppeltaal requires that applications use the [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) flow to request an `access_token`. The following diagram is used for this purpose

![SMART Backend auth flow](<../../.gitbook/assets/backend-service-authorization-diagram (2).png>)

{% hint style="info" %}
The contents of the JWT and the OAuth request are described in detail [here](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#protocol-details). Koppeltaal has an exception to the `scope` parameter. It may be sent by the client, but its value is set by the auth server based on the client's role.
{% endhint %}

{% hint style="warning" %}
The FHIR documentation mentions multiple `alg` header values (e.g., RS384, ES384). Within the POC environment, we only support RS512.
{% endhint %}

### 1. JWT creation

The above diagram shows that a JWT token is first compiled and [signed](requirements/jwt-ondertekenen.md). The following fields must be set:

| Field | Value                                                                                                                                                                                                                                                                                                |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| iss   | Fill with the `client_id` value provided while [Joining a domain](../../domeinbeheer/domein-toetreden.md)                                                                                                                                                                                            |
| sub   | Fill with the `client_id` value provided while [Joining a domain](../../domeinbeheer/domein-toetreden.md)                                                                                                                                                                                            |
| exp   | UNIX timestamp of now + 5 minutes                                                                                                                                                                                                                                                                    |
| aud   | <p>Fill with <a href="https://auth-service.koppeltaal.headease.nl/oauth2/token">https://auth-service.koppeltaal.headease.nl/oauth2/token</a></p><p>(value can be extracted from the <a href="../koppeltaal-server-metadata-opvragen.md#smart-on-fhir-conformance">SMART on FHIR conformance</a>)</p> |
| jti   | Random identifier, this is used by the auth server to prevent replay attacks. Use something like a GUID here.                                                                                                                                                                                        |

### 2. Access Token Request

Execute the following request:

{% swagger baseUrl="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/token" method="post" summary="Request access_token" %}
{% swagger-description %}
See the Response tab for an example response.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="string" required="true" %}
`application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion" type="string" required="true" %}
The signed JWT
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion_type" type="string" required="true" %}
Always fill with 

`urn:ietf:params:oauth:client-assertion-type:jwt-bearer`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="grant_type" type="string" required="true" %}
Always fill with

`client_credentials`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="scope" type="string" required="true" %}
Content can be empty. The 

`scope`

 is set by the auth server based on the client's role
{% endswagger-parameter %}

{% swagger-response status="200" description="The response contains the access_token, validity period in seconds and the type (always Bearer)" %}
```javascript
{
  "access_token": "eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJmaGlyLXNlcnZlciIsIm5iZiI6MTYzMTE5NDM0MCwiZXhwIjoxNjMxMTk3OTQwLCJub25jZSI6IjQ4NTI5NTc2LTFiZTctNGNmOS04MWM0LWRkMTVhMjE4NjcwNyIsInR5cGUiOiJhY2Nlc3MiLCJzY29wZSI6IiIsImF6cCI6IjVhZDdjZjZhLTk1NTYtNGQyMy05MWNhLTI1MGRhZmExZGYwOSJ9.cgBzTRhbvLFPug9bqvCtaVi9ogHpMDqqemoTJjA1C3OpMsU42VyrnNUZ41qtcsZfqjI5OspT678MyVhDHq6DDRc9GLbg8RFLjrow17PfBCgkFALCKXWi9r6gTOZdaGdEPKfqavn1r8-S2HnIaWdEVfNPA1ZlBBxkJsYLl-8zgPmykZDNCbIH1e_SevGc56GeF5dPjHzxSiAI2_t19FM0OL3JfLZ-T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```
{% endswagger-response %}

{% swagger-response status="400" description="The application has been submitted incorrectly. Probably due to missing parameters or the body is delivered in the wrong way." %}
```
Bad request
```
{% endswagger-response %}
{% endswagger %}

Zoals hierboven in de 200 `Response` te zien is, wordt de `access_token` als onderdeel van de response  meegegeven. Deze `access_token` moet meegegeven worden als `Bearer` token in de [`Authorization` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) op elke request naar de Koppeltaal server. Het formaat van de header is als volgt:

As shown above in the 200 Response, the `access_token` is passed as part of the response. This `access_token` must be passed along as a `Bearer` token in the [`Authorization` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) on every request to the Koppeltaal server. The format of the header is as follows:

```
Authorization: <type> <credentials>
```

In the example response, the header should look like this:

```
Authorization: Bearer T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw
```

### Refreshing the access\_token&#x20;

Het `access_token` heeft een relatief korte levensduur. Wanneer het token verlopen is zal Koppeltaal server een `401` error teruggeven. De [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) kent geen `refresh_token`. De applicatie dient punt 1 & 2 opnieuw uit te voeren.

The `access_token` has a relatively short lifetime. When the token expires, the Koppeltaal server will return a `401` error. The [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) does not support a `refresh_token`. The application needs to redo steps 1 & 2.
