# Token introspection

When an application receives an HTI launch, it will [contain a JWT token](../launch-samenstellen/). To ensure that not every application has to build in all the security logic to verify this token, Koppeltaal offers [Token Introspection](https://datatracker.ietf.org/doc/html/rfc7662) on the authentication server. This eliminates the need for the application to [verify the JWT itself](./#verify-the-jwt-yourself).

When Token Introspection approves a token, the extracted body of the JWT token will be returned. If a `200` response code is returned, the application must ALWAYS verify that the `active` attribute in the response is `true`.

{% swagger method="post" path="/oauth2/introspect" baseUrl="{AUTH_SERVER_URL}" summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="client_assertion" required="true" %}
JWT as composed for the

[SMART Backend Service](../../connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md#1.-jwt-samenstellen)
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Accept" required="true" %}
`application/json`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion_type" required="true" %}
Always:

`urn:ietf:params:oauth:client-assertion-type:jwt-bearer`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" required="true" %}
The JWT to be validated
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="The token is introspected. The active field displays whether it is valid" %}
{% tabs %}
{% tab title="Valid Token" %}
```json
{
  "active": true,
  "client_id": "l238j323ds-23ij4",
  "username": "jdoe",
  "scope": "read write dolphin",
  "sub": "Z5O3upPC88QrAjx00dis",
  "aud": "https://protected.example.net/resource",
  "iss": "https://server.example.com/",
  "exp": 1419356238,
  "iat": 1419350238,
  "extension_field": "twenty-seven"
}
```
{% endtab %}

{% tab title="Invalid Token" %}
```json
{
  "active": false
}
```
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Not authorised to execute introspection" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)

[TOP-KT-021 - Token Introspection](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27131284/TOP-KT-021+-+Token+Introspection)
