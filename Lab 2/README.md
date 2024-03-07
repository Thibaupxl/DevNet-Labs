# Lab 2 oplossing

## Part 1: Explore API Documentation Using the API Simulator

- Task preparation and implementation:

    Voor dit lab was er geen voorbereiding nodig, de nodige programma's waren reeds geinstalleerd.

- Task troubleshooting:

    In dit lab was er geen troubleshooting nodig, het lab vroeg om commando's uit te testen door ze te kopieren uit de opgave en te plakken in de terminal van de VM.

- Task verification:

    Tijdens deze oefeningen werd er vooral gewerkt met UI van de website library.demo.local voor bekend te geraken met de verschillende type responses en commando's voor een API. Hierna werden een paar commando's uitgelegd zoals curl: `curl -X GET "http://library.demo.local/api/v1/books" -H "accept: application/json"` die gekopieerd en geplakt moesten worden om ze zelf uit te testen op de VM.


## Part 2: Use Postman to Make API Calls to the API Simulator

- Task preparation and implementation:

    Voor dit lab was er geen voorbereiding nodig, de nodige programma's waren reeds geinstalleerd.

- Task troubleshooting:

    In dit lab was er geen troubleshooting nodig buiten tijdens het openen van Postman moest er een keuze gemaakt worden tussen een account aanmaken of gewerkt worden met de light versie. Ik heb de light versie gekozen en dit was voldoende voor de oefeningen af te werken.

- Task verification:

    Postman kan geopend worden via het icoontje op het bureaublad van de VM.
    Hierna werd er gevraagd om Postman uit te proberen door http://library.demo.local/api/v1/books in te vullen als request URL.
    Hierna wordt er op Send geklikt voor een request uit te sturen. Het post commando wordt ook uitgelegd via commando's en erna werd er met parameters gewerkt in Postman.

    Ik heb geleerd dat Postman zelf de request zal aanpassen door in de UI parameters in te vullen als volgt:
    De key `includeISBN` werd toegevoegd met als waarde `true` en de key sortBy met als waarde `Author`.
     ![Screenshot van Postman](/afbeeldingen/lab2_get.png)
     
     Na het invullen werd de URL aangepast naar: http://library.demo.local/api/v1/books?includeISBN=true&sortBy=author
    

## Part 3: Use Python to Add 100 Books to the API Simulator

- Task preparation and implementation:

    Voor dit lab was er geen voorbereiding nodig, de nodige programma's waren reeds geinstalleerd.

- Task troubleshooting:

    In dit lab was er geen troubleshooting

- Task verification:

    Voor 3B is mijn gebruikte commando:

    `print('My name is {}.'.format(fake.name() + ' and I wrote "Organic incremental neural-net" (ISBN 978-0-669-01935-3)'))`

    Voor de oplossing van vraag 8b te bekomen heb ik eerst het add100RandomBooks.py bestudeerd om te zien of dit hergebruikt kon worden, dit was het geval

    Door in de code  `for i in range(4, 105):` aan te passen naar `for i in range(4, 205):`

    Worden er nog eens 100 extra boeken aan de lijst toegevoegd. Dat dit gelukt is kunnen we nagaan door te surfen naar library.demo.local

    ![Screenshot van library.demo.local](/afbeeldingen/lab2_100books.png)



