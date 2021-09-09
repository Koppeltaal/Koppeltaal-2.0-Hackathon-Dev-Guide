# Requirements

Om toegang te krijgen tot de Koppeltaal server is een access token nodig. Hiervoor moeten de volgende stappen uitgevoerd zijn:

1. De applicatie kan [JWTs signen](../jwt-ondertekenen.md).
2. De applicatie heeft een [JWKS endpoint](../jwks-opzetten.md).
3. De applicatie is het [domein toegetreden](../../../domeinbeheer/domein-toetreden.md) met het opgezette JWKS endpoint en is in het bezit van een   `client_id`. 
4. De applicatie heeft een rol en de status "Actief".

