---
description: >-
  This guide gives a brief overview of the core Koppeltaal 2.0 concepts and how
  to integrate with its services
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

## Important differences with Koppeltaal 1.x

| Concept               | **KT 1.x**                   | KT 2.0                    |
| --------------------- | ---------------------------- | ------------------------- |
| **FHIR Exchange**     | Messaging                    | RESTful API               |
| **Content Validatie** | Aparte service               | Ingebouwde profielen      |
| **Autorisaties**      | Geen - applicatie ziet alles | Op resource & CRUD-niveau |

### FHIR Exchange

By using the (FHIR) RESTful API exchange there is a single source of truth. Koppeltaal 1.x acted as a message broker, giving each participant the responsibility to keep all data up-to-date.&#x20;

Another big advantage is that Koppeltaal 2.0 works with small messages. So no self-contained bundles. This saves a lot of data being transferred over the line and the chance of 409 Conflicts is considerably reduced.

### Content Validatie

Previously, a separate service could be used to check whether the content of a `Bundle` is valid. However, this validation was not enforced by the Koppeltaal server.&#x20;

Koppeltaal 2.0 uses [profiles](https://simplifier.net/Koppeltaalv2.0/\~resources?fhirVersion=R4). A profile indicates exactly what the rules are per [Resource](https://www.hl7.org/fhir/r4/resourcelist.html). The Koppeltaal server can enforce profiles by validating that Resources adhere to the profile.

### **Autorisaties**

Applicaties maken verbinding met de Koppeltaal Server. Binnen Domeinbeheer worden [rollen toegekend](domeinbeheer/rollen-beheren/) aan applicaties. Een rol kan per [Resource](https://www.hl7.org/fhir/resourcelist.html) een CRUD-permissie bevatten. Zo mag je in Koppeltaal 2.0 alleen werken met resources waar je toe gerechtigd bent. Binnen Koppeltaal 1.x kunnen applicaties alles zien waar ze op geabonneerd zijn binnen een Domein.

Applications connect to the Koppeltaal Server. Within "Domeinbeheer" (Domain Management), roles are assigned to applications. A role can contain a CRUD permission per Resource. For example, in Koppeltaal 2.0 you are only allowed to work with resources to which you are entitled. With Koppeltaal 1.x, applications can see everything that they are subscribed to within a Domain.

{% hint style="warning" %}
Note: Resources are managed at application level Koppeltaal 2.0.

The golden rule is that the application **itself** determines which resources are accessible at user level within the application.
{% endhint %}

##
