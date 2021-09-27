# Launchen

Koppeltaal ondersteunt twee launch-mechanismen: [HTI](hti.md) en [SMART HTI On FHIR](smart-hti-on-fhir.md). Beide mechanismen werken in de kern met het HTI JWT token, waardoor beide systemen erg vergelijkbaar zijn qua implementatie. Voor het uitsturen van een launch zijn er minimale verschillen. De ontvangende partij \(`module provider`\) kan, in het geval van een SHOF launch, er nog voor kiezen om de [HTI Flow](hti.md) uit te voeren.

{% hint style="info" %}
Na het lezen va de launch documentatie raden wij aan om te kijken naar de [Launch Test Suite](https://launch-testsuite.koppeltaal.headease.nl). Via deze suite worden de verschillen tussen HTI en SHOF inzichtelijk gemaakt. Ook wordt het duidelijk welke payload opgestuurd wordt. 
{% endhint %}

