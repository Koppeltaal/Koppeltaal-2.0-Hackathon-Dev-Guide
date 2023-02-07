# Create a key pair

To retrieve data from the Koppeltaal server, an `access_token` must be retrieved. How this is done will be explained later. For now, it is important to know is that this works with a [JWT](https://jwt.io/) message. The JWT must be signed using an asymmetric key pair.&#x20;

This [Docker image](https://github.com/Koppeltaal/Koppeltaal-2.0-Generate-KeyPair) can be used to generate an RSA key pair that works within the POC setup. The script generates an RSA key pair with a key length of 2048 bits and a `PKCS8` key format.

{% hint style="info" %}
For proper connection to the POC applications, we recommend the use of an RSA key pair.
{% endhint %}

The key is generated using the following commands:

```bash
ssh-keygen -t rsa -m PKCS8 -b 2048 -f private.key
openssl rsa -in private.key -pubout -outform PEM -out public.key

printf "Private key:\n\n"
cat private.key
printf "\nPublic key:\n\n"
cat public.key
printf "\n"
```
