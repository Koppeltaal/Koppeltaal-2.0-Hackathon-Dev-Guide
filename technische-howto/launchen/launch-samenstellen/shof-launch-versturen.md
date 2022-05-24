# SHOF Launch Versturen

De SHOF launch wordt middels een `GET` request uitgevoerd. De server kan de  gebruiker simpelweg redirecten naar de onderstaande request.

{% hint style="warning" %}
Let op dat de SHOF launch twee keer een  `iss`  defined, één keer inde JWT en één keer in de launch. In beide gevallen hebben ze een andere waarde.
{% endhint %}

{% hint style="warning" %}
Het staat nog ter discussie om de SHOF Launch middels een POST uit te voeren. Op deze manier maakt het niet uit of er een HTI of SHOF launch uitgestuurd wordt.
{% endhint %}

{% swagger baseUrl="{{ActivityDefinition.endpoint}}" path="" method="get" summary="SHOF Launch " %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="launch" type="string" %}
Ondertekende  JWT
{% endswagger-parameter %}

{% swagger-parameter in="query" name="iss" type="string" %}
De URL van de Koppeltaal Server
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}
