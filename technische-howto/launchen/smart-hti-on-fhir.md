---
description: SMART HTI On FHIR
---

# SHOF Flow

{% hint style="warning" %}
De Koppeltaal Server autoriseert applicaties, geen gebruikers. Dit is een mismatch met het [SMART App Launch Framework](http://www.hl7.org/fhir/smart-app-launch/) \(SALF\). Zo verwacht de SALF-flow altijd een `access_token` op gebruikers-niveau  terug te krijgen. Bij SHOF zal er ook een `access_token` teruggegeven worden, alleen is dit een no-op token.
{% endhint %}

SMART HTI On FHIR \(SHOF\) is compatible met HTI. Dit houdt in dat de lancerende partij geen weet heeft of er een HTI of SHOF-flow uitgevoerd wordt. Het voordeel van SHOF is dat het aansluit op een internationale standaard, namelijk het SALF. Daarnaast is er een auth server die de JWT-logica uit handen neemt voor de applicatie die een launch ontvangt. De uitgebreide beschrijving van SHOF is [hier](https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md) te vinden. 

### Requirements

1. Er moet een [JWKS endpoint opgezet](../connectie-maken-met-koppeltaal/requirements/jwks-opzetten.md) zijn.

### Informatie Flow

![bron: https://github.com/Koppeltaal/Koppeltaal-2.0-SMART-HTI-On-FHIR/blob/master/SMART-HTI-On-FHIR.md](../../.gitbook/assets/image%20%281%29.png)

