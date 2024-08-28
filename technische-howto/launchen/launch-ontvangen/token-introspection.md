# Token introspection

When an application receives an HTI launch, it will [contain a JWT token](../launch-samenstellen/). To ensure that not every application has to build in all the security logic to verify this token, Koppeltaal offers [Token Introspection](https://datatracker.ietf.org/doc/html/rfc7662) on the authentication server. This eliminates the need for the application to [verify the JWT itself](./#verify-the-jwt-yourself).

When Token Introspection approves a token, the extracted body of the JWT token will be returned. If a `200` response code is returned, the application must ALWAYS verify that the `active` attribute in the response is `true`.

<mark style="color:green;">`POST`</mark> `{AUTH_SERVER_URL}/oauth2/introspect`

#### Headers

| Name                                           | Type   | Description                         |
| ---------------------------------------------- | ------ | ----------------------------------- |
| Content-Type<mark style="color:red;">\*</mark> | String | `application/x-www-form-urlencoded` |
| Accept<mark style="color:red;">\*</mark>       | String | `application/json`                  |

#### Request Body

| Name                                                      | Type   | Description                                                                                                                                                 |
| --------------------------------------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_assertion<mark style="color:red;">\*</mark>       | String | <p>JWT as composed for the</p><p><a href="../../connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md#1.-jwt-samenstellen">SMART Backend Service</a></p> |
| client\_assertion\_type<mark style="color:red;">\*</mark> | String | <p>Always:</p><p><code>urn:ietf:params:oauth:client-assertion-type:jwt-bearer</code></p>                                                                    |
| token<mark style="color:red;">\*</mark>                   | String | The JWT to be validated                                                                                                                                     |



{% tabs %}
{% tab title="200" %}
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
{% endtab %}

{% tab title="401" %}
Not authorised to execute introspection.
{% endtab %}
{% endtabs %}

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)

[TOP-KT-021 - Token Introspection](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27131284/TOP-KT-021+-+Token+Introspection)
