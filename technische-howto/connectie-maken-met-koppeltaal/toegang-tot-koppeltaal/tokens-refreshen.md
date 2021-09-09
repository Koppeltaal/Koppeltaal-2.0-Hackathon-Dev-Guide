# Tokens Refreshen

Succesvol uitgereikte  `access_tokens` hebben een levensduur. Zoals bij de pagina [Authorizeren](smart-backend-service.md) getoond wordt, ziet een response er bijvoorbeeld als volgt uit:

```javascript
{
  "access_token": "eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJmaGlyLXNlcnZlciIsIm5iZiI6MTYzMTE5NDM0MCwiZXhwIjoxNjMxMTk3OTQwLCJub25jZSI6IjQ4NTI5NTc2LTFiZTctNGNmOS04MWM0LWRkMTVhMjE4NjcwNyIsInR5cGUiOiJhY2Nlc3MiLCJzY29wZSI6IiIsImF6cCI6IjVhZDdjZjZhLTk1NTYtNGQyMy05MWNhLTI1MGRhZmExZGYwOSJ9.cgBzTRhbvLFPug9bqvCtaVi9ogHpMDqqemoTJjA1C3OpMsU42VyrnNUZ41qtcsZfqjI5OspT678MyVhDHq6DDRc9GLbg8RFLjrow17PfBCgkFALCKXWi9r6gTOZdaGdEPKfqavn1r8-S2HnIaWdEVfNPA1ZlBBxkJsYLl-8zgPmykZDNCbIH1e_SevGc56GeF5dPjHzxSiAI2_t19FM0OL3JfLZ-T8DR5tcOo7xfDYD086AUUr0hQIkzbrhuLGHSM5X6QcX84IfZlC0jQ6v_YbdMXlMBDZfUZN1nbsjxtDRwiz0IzZtIOF1XXpS1j0rKy517Vu_cc6LOS1OasUAAEw",
  "expires_in": 3600,
  "refresh_token": "89d970e4-5048-4310-b6f8-c5bc83f92e11",
  "token_type": "Bearer"
}
```

De response geeft netjes aan wat de `access_token` is en hoe lang \(in seconden\) deze geldig is. Nadat deze tijd verstreken is, zal de auth server de `access_token` niet meer accepteren. 

De response bevat ook een `refresh_token`. Deze token kan gebruikt worden laagdrempelig een nieuwe `access_token` op te vragen.

{% api-method method="post" host="https://authentication-service.koppeltaal.headease.nl" path="/oauth2/token" %}
{% api-method-summary %}
Refresh token consumeren
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-form-data-parameters %}
{% api-method-parameter name="scope" type="string" required=true %}
Moet overeenkomen met de ingevulde scope bij het opvragen van de `access_token`
{% endapi-method-parameter %}

{% api-method-parameter name="refresh\_token" type="string" required=true %}
Vullen met de `refresh_token` waarde uit het verlopen `access_token`
{% endapi-method-parameter %}

{% api-method-parameter name="grant\_type" type="string" required=true %}
Altijd vullen met `refresh_token`
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "access_token": "eyJraWQiOiJKR3pTYmgzVS1TQlVabllIQjJQaWhzN0FBc0Vubk8zelpqUS1RSjFTN0tzIiwiYWxnIjoiUlM1MTIiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2F1dGhlbnRpY2F0aW9uLXNlcnZpY2Uua29wcGVsdGFhbC5oZWFkZWFzZS5ubC8iLCJhdWQiOiJmaGlyLXNlcnZlciIsIm5iZiI6MTYzMTE5MzQ5MCwiZXhwIjoxNjMxMTk3MDkwLCJub25jZSI6ImNkY2ZlYmIyLTU4MGEtNDE2YS05MGY4LTE1MzU3ZDk0NzMwNSIsInR5cGUiOiJhY2Nlc3MiLCJhenAiOiI1YWQ3Y2Y2YS05NTU2LTRkMjMtOTFjYS0yNTBkYWZhMWRmMDkifQ.n4w_WLae5We9Vqzq3AuJuGOdBnrMa6NrXVWErplkcw7ruOnEwXW5Py98GNeQB22O7aNsLhTueUBKJ_REq7zzV6PhTg0JZpnwAjo2y5k6o8OK5kKl2SoiIwPCzQz91dVFCC5VqGU7fl5EE_90_9iPZV6Yukha_Yu9f7zSn5kgkjFucw7qPSaPlhyK0fCpK70LNvXcU3is05CeWx4Hf5bA8iCD0-VVqkEzIVz-Pt2YkfG7xa3dWM3SNih3nPH6BnypQIUPAD1vm7vL1yi3csC93W25-q4eD3oXAzdWi_pWsStGg6Pda7jZTIPh63GnEm8MbI_oFBPbCSYMxSslt4Hxsw",
  "expires_in": 3600,
  "refresh_token": "fbbed832-0fb8-40ad-ae13-c5744103ca39",
  "token_type": "Bearer"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
De eerste melding: scope komt niet overeen met de scope uit de initiële access\_token aanvraag  
De tweede melding: Er is iets mis met refresh\_token waarde
{% endapi-method-response-example-description %}

```
Bad Request, invalid scope
Bad Request, invalid token
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

