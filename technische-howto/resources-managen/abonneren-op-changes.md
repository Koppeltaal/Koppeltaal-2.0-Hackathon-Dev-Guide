# Abonneren op changes

Het is vaak van belang om genotificeerd te worden van changes op specifieke resources. Binnen Koppeltaal is het mogelijk om op zeer nauwkeurig niveau genotificeerd te worden van changes.

{% hint style="warning" %}
Het notificatie-mechanisme heeft een maturity level 3. We weten dat FHIR hard werkt aan het verbeteren van het notificatie-mechanisme. In de toekomstige versie is het erg waarschijnlijk dat deze functionaliteit verandert. Momenteel is er bijvoorbeeld geen notificatie wanneer een resource verwijderd wordt.
{% endhint %}

### Een notificatie aanmaken

Applicaties kunnen zelf een notificatie aanmaken middels de [Subscription](https://www.hl7.org/fhir/subscription.html) resource. Deze dient zich te houden aan het [Koppeltaal 2.0 profiel](https://simplifier.net/koppeltaalv2.0/kt2\_subscription). De `Subscription` kan, net als de andere resources, beheerd worden middels de [CRUD Operaties](crud-operaties/).&#x20;

#### Criteria

Het `Subscription.criteria` veld kan door applicaties zelf gevuld worden. Bij elke change zal de Koppeltaal Server nagaan of er `Subscriptions` zijn waarbij de criteria matches aan de doorgevoerde change. Indien dit zo is, zal voor de gematchde subscriptions een notificatie gestuurd worden naar de `Subscription.channel.endpoint`.

#### Channel

Het `Subscription.channel` wordt gebruikt om aan te geven hoe en waar de notificatie heen moet. In de POC is het momenteel alleen mogelijk om het type `email` en `rest-hook` te gebruiken.

{% hint style="info" %}
De Koppeltaal Server past "Notification Narrowing" toe; vergelijkbaar met [Search Narrowing](../../domain-access/rollen-beheren/search-narrowing.md) maar dan voor het notificeren. Echter, wanneer er iets mis gaat in dit process, wordt de notificatie WEL verstuurd. Daarnaast kan het zijn dat een applicatie meerdere `Subscriptions` hebben die matchen op de gestelde `criteria`

Applicaties dienen er rekening mee te houden dat een notificatie niet altijd resulteert in een op te halen resource.
{% endhint %}

### Limitaties

Er zijn verschillen tussen de FHIR Subscription en het Koppeltaal 2.0 profiel. Zo staat Koppeltaal het niet toe om een `Subscription` aan te maken waar de `Subscription.payload` aangezet wordt. Op deze manier is de notificatie puur een "ping" om aan te geven dat er wat veranderd is, maar niet WAT er inhoudelijk veranderd is. Deze keuze is gemaakt om te voorkomen dat er per ongeluk gevoelige informatie naar een verkeerde endpoint wordt gestuurd.&#x20;

### Voorbeeldbericht

```json
{
  "resourceType": "Subscription",
  "meta": {
    "profile": [
      "http://koppeltaal.nl/fhir/StructureDefinition/KT2Subscription"
    ]
  },
  "status": "active",
  "reason": "Meld afgeronde taken",
  "criteria": "Task?status=ready",
  "channel": {
    "type": "rest-hook",
    "endpoint": "https://koppeltaal-testteam6.nl/fictief-subscription",
    "header": [
      "X-KTSubscription: TaskReady",
      "Authorization: Bearer <TOKEN>"
    ]
  }
}
```

