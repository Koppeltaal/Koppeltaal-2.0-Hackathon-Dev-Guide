# Table of contents

* [Developer Guide](README.md)

## POC (Walking Skeleton)

* [Proof Of Concept](poc-walking-skeleton/proof-of-concept/README.md)
  * [Koppeltaal Server](https://fhir-server.koppeltaal.headease.nl/fhir/DEFAULT)
  * [Domain Management](https://domain-admin.koppeltaal.headease.nl/)
  * [Auth Server](https://auth-service.koppeltaal.headease.nl/)
  * [Koppeltaal IdP](https://iam.koppeltaal.headease.nl)
  * [EHR](https://poc-epd.koppeltaal.headease.nl/)
  * [Portal](https://poc-portal.koppeltaal.headease.nl/)
  * [Domain Access Test Suite](https://smart-testsuite.koppeltaal.headease.nl/)
  * [Launch Test Suite](https://launch-testsuite.koppeltaal.headease.nl/)

## Domain access

* [Joining a domain](domeinbeheer/domein-toetreden.md)
* [Role-based access control](domeinbeheer/rollen-beheren/README.md)
  * [Autorisation model](domeinbeheer/rollen-beheren/autorisatiemodel.md)
  * [Creating a role](domeinbeheer/rollen-beheren/rol-aanmaken.md)
  * [Search Narrowing](domeinbeheer/rollen-beheren/search-narrowing.md)
  * [Revoke Permission](domeinbeheer/rollen-beheren/revoke-permission.md)

## Technical HOW-TO <a href="#technische-howto" id="technische-howto"></a>

* [Request Koppeltaal server metadata](technische-howto/koppeltaal-server-metadata-opvragen.md)
* [Connecting to Koppeltaal](technische-howto/connectie-maken-met-koppeltaal/README.md)
  * [Requirements](technische-howto/connectie-maken-met-koppeltaal/requirements/README.md)
    * [Create a key pair](technische-howto/connectie-maken-met-koppeltaal/requirements/key-pair-maken.md)
    * [Signing the JWT](technische-howto/connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md)
    * [JWKS setup](technische-howto/connectie-maken-met-koppeltaal/requirements/jwks-opzetten.md)
  * [Access to Koppeltaal](technische-howto/connectie-maken-met-koppeltaal/toegang-tot-koppeltaal.md)
* [Managing resources](technische-howto/resources-managen/README.md)
  * [Versioning](technische-howto/resources-managen/versionering.md)
  * [CRUD Operations](technische-howto/resources-managen/crud-operaties/README.md)
    * [Retrieve all Resources](technische-howto/resources-managen/crud-operaties/alle-resources-ophalen.md)
    * [Retrieve specific Resource](technische-howto/resources-managen/crud-operaties/resource-ophalen.md)
    * [Create a Resource](technische-howto/resources-managen/crud-operaties/resource-aanmaken.md)
    * [Update a Resource](technische-howto/resources-managen/crud-operaties/resource-updaten.md)
  * [Subscribing to changes](technische-howto/resources-managen/abonneren-op-changes.md)
* [Launching](technische-howto/launchen/README.md)
  * [HTI Flow](technische-howto/launchen/hti.md)
  * [SHOF Flow](technische-howto/launchen/smart-hti-on-fhir.md)
  * [Compose a launch](technische-howto/launchen/launch-samenstellen/README.md)
  * [Initiating a launch](technische-howto/launchen/launch-samenstellen/hti-launch-versturen.md)
  * [Receiving a HTI launch](technische-howto/launchen/launch-ontvangen/README.md)
    * [Token introspection](technische-howto/launchen/launch-ontvangen/token-introspection.md)
  * [Receiving a SHOF launch](technische-howto/launchen/smart-hti-on-fhir-launch-ontvangen.md)

## Hackathon Use Cases

* [Requirements](hackathon-use-cases/requirements/README.md)
  * [Install and configure IRMA](hackathon-use-cases/requirements/irma-installeren-and-configureren.md)
* [Use-Cases](hackathon-use-cases/crud-task/README.md)
  * [Use-Case 1: Create a Task](hackathon-use-cases/crud-task/use-case-1-opvoeren-taak/README.md)
    * [Create an ActivityDefinition](hackathon-use-cases/crud-task/use-case-1-opvoeren-taak/zelf-een-activitydefinition-maken.md)
  * [Use-Case 2: HTI Launch](hackathon-use-cases/crud-task/use-case-2-hti-launch.md)
  * [Use-case 3: SHOF Launch](hackathon-use-cases/crud-task/use-case-3-smart-hti-on-fhir-launch.md)
  * [Use-case 4: Subscribing to changes](hackathon-use-cases/crud-task/use-case-4-abonneren-op-changes.md)

## Useful Links

* [Simplifier Profiles](https://simplifier.net/Koppeltaalv2.0/\~resources?fhirVersion=R4)
* [FHIR Docs](https://www.hl7.org/fhir/documentation.html)
* [HTI documentation](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md)
* [GitHub](https://github.com/Koppeltaal/)
* [Koppeltaal 2.0 Specifications & Architecture](https://public.vzvz.nl/display/KTSA/Koppeltaal+Stelselarchitectuur)
