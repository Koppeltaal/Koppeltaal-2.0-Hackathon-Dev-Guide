---
description: >-
  Deze guide geeft een beknopt overzicht van de Koppeltaal 2.0 kernconcepten en
  hoe te integreren.
---

# Developer Guide

## Document Information

### Copyright Notice

![](.gitbook/assets/sharealike.png)

This document is released under the [Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) license](https://creativecommons.org/licenses/by-sa/4.0/).

### Status

This document is in draft.

### Author / contact

[Joris Scharp](mailto:joris@headease.nl)\
[Headease B.V.](https://www.headease.nl/)

## Belangrijke verschillen t.o.v. Koppeltaal 1.x

| Concept               | **KT 1.x**                   | KT 2.0                    |
| --------------------- | ---------------------------- | ------------------------- |
| **FHIR Exchange**     | Messaging                    | RESTful API               |
| **Content Validatie** | Aparte service               | Ingebouwde profielen      |
| **Autorisaties**      | Geen - applicatie ziet alles | Op resource & CRUD-niveau |

### FHIR Exchange

Door het gebruik van de (FHIR) RESTful API exchange is er een enkele bron van waarheid. Koppeltaal 1.x fungeerde als een message broker, waardoor elke deelnemen de verantwoordelijkheid had om alle data zelf up-to-date te houden.&#x20;

Ook is een groot voordeel dat Koppeltaal 2.0 met kleine berichten werkt. Dus geen self-contained bundels. Dit scheelt een hoop data over de lijn en de kans op `409 Conflicts` wordt aanzienlijk verkleind.

### Content Validatie

Voorheen kon een aparte service gebruikt worden om te kijken of de content van een `Bundle` valide is. Deze validatie werd echter niet afgedwongen door de Koppeltaal server.

Koppeltaal 2.0 maakt gebruik van [profielen](https://simplifier.net/Koppeltaalv2.0/\~resources?fhirVersion=R4). Een profiel geeft exact aan wat de regels zijn per [Resource](https://www.hl7.org/fhir/resourcelist.html). De Koppeltaal server kan profielen afdwingen en valideren of men zich aan een profiel houdt.

### **Autorisaties**

Applicaties maken verbinding met de Koppeltaal Server. Binnen Domeinbeheer worden [rollen toegekend](domeinbeheer/rollen-beheren/) aan applicaties. Een rol kan per [Resource](https://www.hl7.org/fhir/resourcelist.html) een CRUD-permissie bevatten. Zo mag je in Koppeltaal 2.0 alleen werken met resources waar je toe gerechtigd bent. Binnen Koppeltaal 1.x kunnen applicaties alles zien waar ze op geabonneerd zijn binnen een Domein.

{% hint style="warning" %}
Let op: Resources worden op applicatie-niveau gemanaged door Koppeltaal 2.0.&#x20;

De gouden regel is dat de applicatie **zelf** bepaalt welke resources toegankelijk zijn op user-niveau binnen de applicatie.
{% endhint %}

##
