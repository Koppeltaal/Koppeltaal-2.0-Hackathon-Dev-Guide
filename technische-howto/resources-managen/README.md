# Resources Managen

Het is van groot belang om in te zien dat de **applicaties** verbinden met de Koppeltaal Server, en niet de **eindgebruikers**. Koppeltaal draagt er zorg voor dat de applicaties alleen operaties mogen uitvoeren op resources waartoe zij bevoegd zijn. Op gebruikers-niveau zijn de applicaties zelf verantwoordelijk om bij  te houden welke operaties uitgevoerd mogen worden.

Dit houdt dus, bijvoorbeeld, in dat een applicatie alle taken mag opvragen waartoe zij bevoegd zijn. Echter weet de Koppeltaal Server niet aan **wie** deze resource getoond wordt \(deze wordt immers aan een applicatie geleverd\). Het is dus aan de applicatie om de zorgen dat de taken van bijv. `Patient/123` enkel getoond wordt aan bevoegden.

