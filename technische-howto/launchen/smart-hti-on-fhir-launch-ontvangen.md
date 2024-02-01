# Receiving a SHOF launch

## Requirements

1. The application [created](../resources-managen/crud-operaties/resource-aanmaken.md) an [`ActivityDefinition`](https://simplifier.net/koppeltaalv2.0/kt2activitydefinition).
2. The user performing the launch must have an account on the shared IdP

## Flow

<figure><img src="../../.gitbook/assets/SMART on FHIR app launch and HTI.drawio (1).png" alt="SMART HTI Flow"><figcaption><p>SMART HTI Flow</p></figcaption></figure>

1. When the launch arrives, the [conformance is retrieved from the Koppeltaal Server](../koppeltaal-server-metadata-opvragen.md). Here the authorize & token URL can be requested.
2. A redirect is sent to the authorize URL. The auth server will redirect the browser to a shared IdP using an OIDC code flow. If the launching platform does not use this shared IdP as a login mechanism, the user will have to login here as well. At the POC it is possible to create an account directly from this login screen. New users are given the role patient by default. The authorize call will check if the logged in user matches the user from the launch token.
3. A successful authorize call returns the `code` & `state` parameters to the `redirect_uri`. Note that the `state` (provided at the start of the `/authorize` call) is returned to the `redirect_uri`, this way you can find out which launch request is involved and, for example, relate to a specific user session.
4. From the back-end, execute the [Get Token request](smart-hti-on-fhir-launch-ontvangen.md#get-token). Here, the `code` is exchanged for:
   1. an `id_token` (contains information of the logged in user as `JWT`).
   2. A no-op `access_token` (not to be used on the Koppeltaal Server because it is user-specific).
   3. Additional context fields such as `task`, which the auth server fills based on the JWT provided as launch param.

By means of the context object it can be determined who, with which role, is logged on to the system and which task should be opened. When there is a valid response to this request, the user can be authenticated and e.g. a session can be created for the user.

{% swagger baseUrl="https://auth-service.koppeltaal.headease.nl" path="/oauth2/authorize" method="get" summary="Authorize Request" expanded="true" %}
{% swagger-description %}
The URL should be determined from the Koppeltaal metadata
{% endswagger-description %}

{% swagger-parameter in="query" name="aud" type="string" required="true" %}
URL of the Koppeltaal server (the same as the `iss` value)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="scope" type="string" required="true" %}
Always:

`launch openid fhirUser`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state" type="string" required="true" %}
An opaque value used by the client to maintain the state between the request and the callback. The authorization server takes this value when redirecting the user agent back to the client. The parameter MUST be used to prevent cross-site request forgery (CSRF) attacks or session fixation
{% endswagger-parameter %}

{% swagger-parameter in="query" name="launch" type="string" required="true" %}
The HTI token (from the

`launch`

param)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="redirect_uri" type="string" required="true" %}
The URL to which the

`code`

should be returned
{% endswagger-parameter %}

{% swagger-parameter in="query" name="client_id" type="string" required="true" %}
The `client_id` (from Domain Admin) of the application receiving the launch
{% endswagger-parameter %}

{% swagger-parameter in="query" name="response_type" type="string" required="true" %}
Altijd:

`code`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="code_challenge" required="true" %}
A generated code challenge as per

[rfc7636](https://www.rfc-editor.org/rfc/rfc7636#appendix-B)
{% endswagger-parameter %}

{% swagger-parameter in="query" name="code_challenge_method" required="true" %}
Always:

`S256`
{% endswagger-parameter %}

{% swagger-response status="302" description="The Location header will redirect with a code and the state" %}
```
Loocation: https://launch-testsuite.koppeltaal.headease.nl/module_authentication_shof?code=05b542e2-6206-449b-924b-99d39168029b&state=99a8cf9a-28c2-4867-8123-486c04003482
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/token" method="post" summary="Get Token" expanded="true" %}
{% swagger-description %}
The URL should be determined from the Koppeltaal metadata
{% endswagger-description %}

{% swagger-parameter in="body" name="client_assertion" required="true" %}
JWT as composed for the

[SMART Backend Service](../connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md#1.-jwt-samenstellen) for the application receiving the launch
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`application/x-www-form-urlencoded`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="client_assertion_type" required="true" %}
Always:

`urn:ietf:params:oauth:client-assertion-type:jwt-bearer`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="redirect_uri" type="string" required="true" %}
The same

`redirect_uri`

as used at the authorize request
{% endswagger-parameter %}

{% swagger-parameter in="body" name="code" type="string" required="true" %}
`code`

provided by the auth server on the redirect from the authorize request
{% endswagger-parameter %}

{% swagger-parameter in="body" name="grant_type" type="string" required="true" %}
Always:

`authorization_code`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="code_verifier" required="true" %}
Unhashed value used foe generating the

`code_challenge`

in the authorize step as per

[rfc7636](https://www.rfc-editor.org/rfc/rfc7636#appendix-B)
{% endswagger-parameter %}

{% swagger-response status="200" description="The OIDC response plus additional context fields used in the launch is returned" %}
```javascript
{
  "access_token": "NOOP",
  "id_token": "eyJraWQiOiJvUDQ4NmxJcmwzRFFQdXF0dVVqUmxqc01oWFA0alJmdXoxS19uX0dpQmRrIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJiMDJkNmVhNi1iMWEyLTRjZDQtODJmNS1iNjQyM2Q2NmE5ODgiLCJuYmYiOjE2MzI4MTMzNTAsImV4cCI6MTYzMjgxNjk1MCwibm9uY2UiOiJmNGMxODZlNy1jMzI2LTQxODAtYjFmMi1jYTllMWI4YTgyYWQiLCJzdWIiOiJQYXRpZW50LzE5NjMiLCJhenAiOiJiMDJkNmVhNi1iMWEyLTRjZDQtODJmNS1iNjQyM2Q2NmE5ODgifQ.UfBtTACLOhsCMr4Tlen3RUFek06WgWc-aaTPQzJzmHVGYBLY3CnJXTLI1FfCzp1ChM3vx-e2jbFCDHak6ennsuitki-1HnrZitTKpG8qKZK_f24gwVFM5LmzdUXtuTszJSeulpRG8zmNI96pqaIW4ru995LwhKLd-XSOY02BbAMo4XZ46ZW8DBXnhr32CI9TUza8NEQoxlQAF8EboUhro5vauPrjdshP3jQFUNSs5NceB4er3RnF10Zd6SiLFP-_c2ynaj_v87fJEgVGw63byYcKm6O3bTW2KsSz_YNYDYv8DWjYAp25P79e-Hlc3ERcybhLnLy0_-Rkvjk5P_240g"
  "scope": "openid fhirUser launch",
  "token_type": "Bearer",
  "expires_in": "3600",
  "resource": "Task/37fa63e3-b118-4180-8470-74b1df508bf6",
  "definition": "https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT/ActivityDefinition/fbbf47a5-2a55-4e3a-8dec-a8eb63875374",
  "sub": "Practitioner/52b6535b-4e69-455b-9d9f-952e708ad393",
  "patient": "Patient/70184602-4d15-4ca9-89a6-00e78123b22e",
  "intent": "plan"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Client unauthorized or wrong code_verifier" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)
