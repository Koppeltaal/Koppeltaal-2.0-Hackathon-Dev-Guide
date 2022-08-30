---
description: SMART HTI On FHIR
---

# SHOF Flow

{% hint style="warning" %}
De Koppeltaal Server autoriseert applicaties, geen gebruikers. Dit is een mismatch met het [SMART App Launch Framework](http://www.hl7.org/fhir/smart-app-launch/) (SALF). Zo verwacht de SALF-flow altijd een `access_token` op gebruikers-niveau  terug te krijgen. Bij SHOF zal er ook een `access_token` teruggegeven worden, alleen is dit een no-op token.
{% endhint %}

SMART HTI On FHIR (SHOF) is compatible met HTI. Dit houdt in dat de lancerende partij geen weet heeft of er een HTI of SHOF-flow uitgevoerd wordt. De voornaamste verschillen tussen HTI en SHOF zijn:

1. SHOF sluit aan op een internationale standaard, namelijk het SALF.&#x20;
2. Er een auth server die de JWT-logica uit handen neemt voor de applicatie die een launch ontvangt.
3. SHOF voert tijdens de `/authorize` stap een extra controle uit op de ingelogde gebruiker middels een gedeelde IdP. Dit voorkomt dat een launch token onderschept en uitgevoerd wordt. De user identifier die in het launch token zit wordt vergeleken met de identifier van de ingelogde gebruiker op het IdP.&#x20;

### Requirements

1. Er moet een [JWKS endpoint opgezet](../connectie-maken-met-koppeltaal/requirements/jwks-opzetten.md) zijn.
2. De gebruiker die de launch uitvoert moet een account hebben op de gedeelde IdP.&#x20;

### Informatie Flow

![bron: https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md](<../../.gitbook/assets/image (1).png>)
