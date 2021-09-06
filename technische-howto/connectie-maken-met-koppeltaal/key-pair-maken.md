# Key Pair Maken

Om data op te halen uit de Koppeltaal Server moet er een `access_token` opgehaald worden. Hoe deze request er exact uit moet zien wordt nog toegelicht. Belangrijk om te weten is dat dit middels een [JWT](https://jwt.io/) bericht werkt. Het JWT token moet ondertekend worden middels een asymmetrische key pair. 

Dit [Docker image](https://github.com/Koppeltaal/Koppeltaal-2.0-Generate-KeyPair) kan gebruik worden om een RSA key pair te genereren die werkt binnen de POC opzet. Het script genereert een RSA key pair met een key length van 2048 bits en een`PKCS8` key formaat. 

{% hint style="info" %}
Voor een goede aansluiting op de POC applicaties adviseren wij het gebruik van een RSA key pair.
{% endhint %}

De key wordt gegenereerd middels het volgende commando:

```text
ssh-keygen -t rsa -m PKCS8 -b 2048 -f private.key
openssl rsa -in private.key -pubout -outform PEM -out public.key

printf "Private key:\n\n"
cat private.key
printf "\nPublic key:\n\n"
cat public.key
printf "\n"
```

