---
description: >-
  Koppeltaal 2.0 kent een autorisatiemodel. Dit houdt in dat applicaties niet
  alle gegevens binnen een domein in kunnen zien. Wat er precies ingezien mag
  worden, wordt bepaald met het autorisatiemodel.
---

# Autorisatiemodel

## 1. Authenticeren

Wanneer een applicatie een [domein is toegetreden](../domein-toetreden.md), zal de applicatie een `client_id` toegekend hebben gekregen. Deze `client_id` zit verwerkt in het `access_token` dat wordt gebruikt om [connectie te maken met Koppeltaal](../../technische-howto/connectie-maken-met-koppeltaal/). Zo weet de Koppeltaal server welke applicatie een request uitvoert en ook wat haar rol is.

## 2. Eigenaarschap van Resources

De Koppeltaal server voegt automatisch een `resource-origin` extensie toe op elke `DomainResource` die aangemaakt wordt. Middels deze extensie kan de server bepalen wie de originele auteur is van resources. Dit is een essentieel deel van het autorisatiemodel.

## 3. Rol + permissies

Elke applicatie die een domein is toegetreden krijgt één rol toegekend. Een rol is gekoppeld aan meerdere permissies. Een permissie kent 3 eigenschappen:

### Resource 

Een permissie is altijd gekoppeld aan een enkele [FHIR Domain Resource](https://www.hl7.org/fhir/domainresource.html). 

### Actie 

In de permissie wordt aangegeven welke CRUD-actie \(create, read, update, delete\) het betreft. 

### Scope 

De scope geeft de scope  van `resource-owners` aan. De volgende waarden zijn mogelijk:

| Scope | Beschrijving |
| :--- | :--- |
| Own | De permissie is enkel van toepassing op resources waarvan de `resource-owner` overeenkomt met de geauthenticeerde applicatie. |
| All | De permissie is van toepassing op alle resources in het domein |
| Granted | De permissie is van toepassing op resources waarvan de `resource-owner` overeenkomt met de geselecteerde applicatie\(s\). |

