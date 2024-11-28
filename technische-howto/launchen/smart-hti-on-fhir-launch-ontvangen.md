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

## Authorize Request

<mark style="color:blue;">`GET`</mark> `https://auth-service.koppeltaal.headease.nl/oauth2/authorize`

The URL should be determined from the Koppeltaal metadata

#### Query Parameters

<table><thead><tr><th width="160">Name</th><th width="82">Type</th><th>Description</th></tr></thead><tbody><tr><td>aud<mark style="color:red;">*</mark></td><td>string</td><td>URL of the Koppeltaal server (the same as the <code>iss</code> value)</td></tr><tr><td>scope<mark style="color:red;">*</mark></td><td>string</td><td><p>Always:</p><p><code>launch openid fhirUser</code></p></td></tr><tr><td>state<mark style="color:red;">*</mark></td><td>string</td><td>An opaque value used by the client to maintain the state between the request and the callback. The authorization server takes this value when redirecting the user agent back to the client. The parameter MUST be used to prevent cross-site request forgery (CSRF) attacks or session fixation</td></tr><tr><td>launch<mark style="color:red;">*</mark></td><td>string</td><td>The HTI token (from the <code>launch</code>param)</td></tr><tr><td>redirect_uri<mark style="color:red;">*</mark></td><td>string</td><td><p>The URL to which the</p><p><code>code</code></p><p>should be returned</p></td></tr><tr><td>client_id<mark style="color:red;">*</mark></td><td>string</td><td>The <code>client_id</code> (from Domain Admin) of the application receiving the launch</td></tr><tr><td>response_type<mark style="color:red;">*</mark></td><td>string</td><td><p>Always:</p><p><code>code</code></p></td></tr><tr><td>code_challenge<mark style="color:red;">*</mark></td><td>String</td><td>A generated code challenge as per <a href="https://www.rfc-editor.org/rfc/rfc7636#appendix-B">rfc7636</a></td></tr><tr><td>code_challenge_method<mark style="color:red;">*</mark></td><td>String</td><td><p>Always:</p><p><code>S256</code></p></td></tr></tbody></table>

{% tabs %}
{% tab title="302" %}
The Location header will redirect with a code and the state

```
Location: https://launch-testsuite.koppeltaal.headease.nl/module_authentication_shof?code=05b542e2-6206-449b-924b-99d39168029b&state=99a8cf9a-28c2-4867-8123-486c04003482
```
{% endtab %}
{% endtabs %}

## Get Token

<mark style="color:green;">`POST`</mark> `https://authentication-service.koppeltaal.headease.nl/oauth2/token`

The URL should be determined from the Koppeltaal metadata

#### Headers

<table><thead><tr><th width="171">Name</th><th width="102">Type</th><th>Description</th></tr></thead><tbody><tr><td>Content-Type<mark style="color:red;">*</mark></td><td>String</td><td><code>application/x-www-form-urlencoded</code></td></tr></tbody></table>

#### Request Body

<table><thead><tr><th width="217">Name</th><th width="102">Type</th><th>Description</th></tr></thead><tbody><tr><td>client_assertion<mark style="color:red;">*</mark></td><td>String</td><td><p>JWT as composed for the</p><p><a href="../connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md#1.-jwt-samenstellen">SMART Backend Service</a> for the application receiving the launch</p></td></tr><tr><td>client_assertion_type<mark style="color:red;">*</mark></td><td>String</td><td><p>Always:</p><p><code>urn:ietf:params:oauth:client-assertion-type:jwt-bearer</code></p></td></tr><tr><td>redirect_uri<mark style="color:red;">*</mark></td><td>string</td><td>The same <code>redirect_uri</code> as used at the authorize request</td></tr><tr><td>code<mark style="color:red;">*</mark></td><td>string</td><td><code>code</code> provided by the auth server on the redirect from the authorize request</td></tr><tr><td>grant_type<mark style="color:red;">*</mark></td><td>string</td><td><p>Always:</p><p><code>authorization_code</code></p></td></tr><tr><td>code_verifier<mark style="color:red;">*</mark></td><td>String</td><td><p>Unhashed value used foe generating the <code>code_challenge</code> in the authorize step as per</p><p><a href="https://www.rfc-editor.org/rfc/rfc7636#appendix-B">rfc7636</a></p></td></tr></tbody></table>

{% tabs %}
{% tab title="200" %}
The OIDC response plus additional context fields used in the launch is returned

```json
{
  "access_token": "NOOP",
  "id_token": "eyJraWQiOiJvUDQ4NmxJcmwzRFFQdXF0dVVqUmxqc01oWFA0alJmdXoxS19uX0dpQmRrIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJiMDJkNmVhNi1iMWEyLTRjZDQtODJmNS1iNjQyM2Q2NmE5ODgiLCJuYmYiOjE2MzI4MTMzNTAsImV4cCI6MTYzMjgxNjk1MCwibm9uY2UiOiJmNGMxODZlNy1jMzI2LTQxODAtYjFmMi1jYTllMWI4YTgyYWQiLCJzdWIiOiJQYXRpZW50LzE5NjMiLCJhenAiOiJiMDJkNmVhNi1iMWEyLTRjZDQtODJmNS1iNjQyM2Q2NmE5ODgifQ.UfBtTACLOhsCMr4Tlen3RUFek06WgWc-aaTPQzJzmHVGYBLY3CnJXTLI1FfCzp1ChM3vx-e2jbFCDHak6ennsuitki-1HnrZitTKpG8qKZK_f24gwVFM5LmzdUXtuTszJSeulpRG8zmNI96pqaIW4ru995LwhKLd-XSOY02BbAMo4XZ46ZW8DBXnhr32CI9TUza8NEQoxlQAF8EboUhro5vauPrjdshP3jQFUNSs5NceB4er3RnF10Zd6SiLFP-_c2ynaj_v87fJEgVGw63byYcKm6O3bTW2KsSz_YNYDYv8DWjYAp25P79e-Hlc3ERcybhLnLy0_-Rkvjk5P_240g"
  "scope": "openid fhirUser launch",
  "token_type": "Bearer",
  "expires_in": "3600",
  "resource": "Task/37fa63e3-b118-4180-8470-74b1df508bf6",
  "definition": "ActivityDefinition/64b95e9a-e0fa-49ea-9cc5-00ff61300d3d",
  "sub": "Practitioner/52b6535b-4e69-455b-9d9f-952e708ad393",
  "patient": "Patient/70184602-4d15-4ca9-89a6-00e78123b22e",
  "intent": "plan"
}
```
{% endtab %}
{% endtabs %}

## Topics

[TOP-KT-007 - Koppeltaal Launch](https://vzvz.atlassian.net/wiki/spaces/KTSA/pages/27123510/TOP-KT-007+-+Koppeltaal+Launch)
