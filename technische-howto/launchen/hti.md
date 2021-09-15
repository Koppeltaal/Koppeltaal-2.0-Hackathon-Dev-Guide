---
description: Health Tools Interoperability
---

# HTI

[HTI](https://github.com/GIDSOpenStandaarden/GIDS-HTI-Protocol/blob/master/HTI.md) is een light-weight oplossing van [GIDS](https://www.gidsopenstandaarden.org/hti-health-tools-interoperability) om snel client-to-client een launch te kunnen implementeren. Kort samengevat is de HTI launch simpelweg een [ondertekende JWT](../connectie-maken-met-koppeltaal/requirements/jwt-ondertekenen.md) met als payload een [FHIR Task](https://www.hl7.org/fhir/task.html) die verstuurd wordt van de ene client naar de andere. Middels een public key kan geverifieerd worden dan de JWT van een betrouwbare bron komt. De `Task` dient als context voor de launch.

