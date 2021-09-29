# SHOF Launch Versturen

De SHOF launch wordt middels een `GET` request uitgevoerd. De server kan de  gebruiker simpelweg redirecten naar de onderstaande request.

{% hint style="warning" %}
Let op dat de SHOF launch twee keer een  `iss`  defined, één keer inde JWT en één keer in de launch. In beide gevallen hebben ze een andere waarde.
{% endhint %}

{% hint style="warning" %}
Het staat nog ter discussie om de SHOF Launch middels een POST uit te voeren. Op deze manier maakt het niet uit of er een HTI of SHOF launch uitgestuurd wordt.
{% endhint %}

{% api-method method="get" host="{{ActivityDefinition.endpoint}}" path="" %}
{% api-method-summary %}
SHOF Launch 
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="launch" type="string" required=true %}
Ondertekende  JWT
{% endapi-method-parameter %}

{% api-method-parameter name="iss" type="string" required=true %}
De URL van de Koppeltaal Server
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

