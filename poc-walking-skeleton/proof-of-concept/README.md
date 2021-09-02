---
description: Ook bekend als de Walking Skeleton
---

# Proof Of Concept

Koppeltaal 2.0 is Agile tot stand gekomen. De bedachte architectuur wordt beproefd middels POC-applicaties. De applicaties die gebruikt worden binnen de POC zijn **niet** de applicaties die uiteindelijk gebruikt worden wanneer Koppeltaal 2.0 live gaat.

{% hint style="info" %}
Applicaties binnen de POC gebruiken nu vaak IRMA om de gebruiker te identificeren en over systemen te kunnen relateren. Dit is geen onderdeel van de standaard. Het kan dus goed zijn dat er bij Koppeltaal 2.0 geen gebruik wordt gemaakt van IRMA.
{% endhint %}

De POC bestaat uit verschillende type applicaties:

| Applicatietype | Beschrijving |
| :--- | :--- |
| Stelselcomponent | Applicaties die de kern van Koppeltaal 2.0 vormen. |
| Placeholder | Applicaties die e-health applicaties imiteren. |
| Extra dienst | Applicaties die  buiten de kern  van Koppeltaal 2.0 vallen maar handig zijn voor de ontwikkeling. |

De POC bestaat uit de volgende applicaties:

| Applicatie | Type | Beschrijving |
| :--- | :--- | :--- |
| Koppeltaal Server | Stelselcomponent | Een FHIR HAPI R4 implementatie voor Koppeltaal. |
| Domeinbeheer | Stelselcomponent | Hier kunnen applicaties zich aanmelden voor een domein. Ook wordt hier een rol toegekend aan de applicatie die bepaalt welke acties de applicatie mag uitvoeren op de Koppeltaal Server. |
| Auth Server | Stelselcomponent | Hier kan een applicatie - na goedkeuring bij het Domeinbeheer - een access token opvragen middels de [Backend Services Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token) flow. |
| EPD | Placeholder | Deze applicatie is gemaakt om een EPD te imiteren. Uiteindelijk is dit uitgegroeid tot een applicatie waarbij de veelvoorkomende Koppeltaal Resources gemanaged kunnen worden. |
| Portaal | Placeholder | Deze applicatie fungeert als patientportaal, behandelarenportaal en related person portaal. Wanneer men hier inlogt, zoekt het  portaal een gebruiker a.d.h.v. het ingelogde e-mailadres in de Koppeltaal Server. Afhankelijk van welke resource gevonden  wordt \(`Patient`, `Practitioner` of `RelatedPerson`\) wordt de rol bepaald.  |
| SMART Test Suite | Extra Dienst | Geen officieel component, maar erg handig tijdens de ontwikkeling. Deze test suite helpt bij de implementatie van de  [Backend Services Authorization](https://hl7.org/fhir/uv/bulkdata/authorization/index.html#obtaining-an-access-token). |



