# Access to Koppeltaal

## Request Flow

{% hint style="info" %}
For an interactive demo, go to our [Koppeltaal Test Suite](https://testsuite.koppeltaal.headease.nl/) and execute these steps
{% endhint %}

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

## Request access\_token

<mark style="color:green;">`POST`</mark> `https://auth-service.koppeltaal.headease.nl/oauth2/token`

See the Response tab for an example response.

#### Headers

| Name                                           | Type   | Description                         |
| ---------------------------------------------- | ------ | ----------------------------------- |
| Content-Type<mark style="color:red;">\*</mark> | string | `application/x-www-form-urlencoded` |

#### Request Body

| Name                                                      | Type   | Description                                                                                                          |
| --------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| client\_assertion<mark style="color:red;">\*</mark>       | string | The signed JWT                                                                                                       |
| client\_assertion\_type<mark style="color:red;">\*</mark> | string | <p>Always fill with</p><p><code>urn:ietf:params:oauth:client-assertion-type:jwt-bearer</code></p>                    |
| grant\_type<mark style="color:red;">\*</mark>             | string | <p>Always fill with</p><p><code>client_credentials</code></p>                                                        |
| scope<mark style="color:red;">\*</mark>                   | string | <p>Content can be empty. The</p><p><code>scope</code></p><p>is set by the auth server based on the client's role</p> |

{% tabs %}
{% tab title="200" %}
```javascript
{
  "access_token": "eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJmaGlyLXNlcnZlciIsIm5iZiI6MTYzMTE5NDM0MCwiZXhwIjoxNjMxMTk3OTQwLCJub25jZSI6IjQ4NTI5NTc2LTFiZTctNGNmOS04MWM0LWRkMTVhMjE4NjcwNyIsInR5cGUiOiJhY2Nlc3MiLCJzY29wZSI6IiIsImF6cCI6IjVhZDdjZjZhLTk1NTYtNGQyMy05MWNhLTI1MGRhZmExZGYwOSJ9.cgBzTRhbvLFPug9bqvCtaVi9ogHpMDqqemoTJjA1C3OpMsU42VyrnNUZ41qtcsZfqjI5OspT678MyVhDHq6DDRc9GLbg8RFLjrow17PfBCgkFALCKXWi9r6gTOZdaGdEPKfqavn1r8-S2HnIaWdEVfNPA1ZlBBxkJsYLl-8zgPmykZDNCbIH1e_SevGc56GeF5dPjHzxSiAI2_t19FM0OL3JfLZ-T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```
{% endtab %}

{% tab title="400" %}
The request has been submitted incorrectly. Probably due to missing parameters or the body is delivered in the wrong way.
{% endtab %}
{% endtabs %}

As shown above in the 200 Response, the `access_token` is passed as part of the response. This `access_token` must be passed along as a `Bearer` token in the [`Authorization` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) on every request to the Koppeltaal server. The format of the header is as follows:

```
Authorization: <type> <credentials>
```

In the example response, the header should look like this:

```
Authorization: Bearer T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw
```

### Refreshing the access\_token

The `access_token` has a relatively short lifetime. When the token expires, the Koppeltaal server will return a `401` error. The [SMART Backend Services: Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) does not support a `refresh_token`. The application needs to redo steps 1 & 2.

## Topics

[TOP-KT-005c - Applicatie toegang: SMART on FHIR backend services](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27125356/TOP-KT-005c+-+Applicatie+toegang+SMART+on+FHIR+backend+services)
