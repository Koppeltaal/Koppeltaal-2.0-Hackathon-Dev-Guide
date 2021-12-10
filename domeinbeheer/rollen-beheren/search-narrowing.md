# Search Narrowing

{% hint style="info" %}
Rollen en permissies worden elke 5 seconden in de cache ververst. Wanneer er  aanpassingen gemaakt worden kan het 5 seconden duren voordat deze in werking treden.
{% endhint %}

Wanneer een applicatie resources mag lezen met een `Own` of `Granted` scope, betekent dit dat niet alle resources teruggeven mogen worden door de Koppeltaal server. Om dit zo simpel mogelijk te houden voor platformen die gebruik maken van Koppeltaal is gekozen voor "Search Narrowing". Dit houdt in dat het platform simpelweg een query uit kan voeren als `GET <KT_DOMAIN>/Patient` om "alle" `Patient` resources op te halen. De Koppeltaal server zorgt er voor dat enkel de `Patient` resources teruggegeven worden waartoe de applicatie geautoriseerd is.&#x20;

{% embed url="https://drive.google.com/file/d/1bJFTUdJPhLLOxm-iSk33K8raf9Ebm3JN/view" %}
Search Narrowing
{% endembed %}

