# Lab 3 oplossing

## Part 2: Explore Python Development Tools 3.1.12

- Findings and important commands:

    In dit lab werd er geleerd om met PIP en met virtuele omgevingen te werken binnen Python.
    Alle commando's werden uitgevoerd op de voorziene DEVASC VM.
    De belangrijkste tools en commando's om te onthouden zijn `venv`, voorbeeldcommando: `python3 -m venv devfun`. Wat een virtuele omgeving aanmaakt genaamd devfun waarin Python versie 3 gebruikt zal worden.
    Daarnaast werd PIP aangeleerd voor packages te installeren en `pip3 freeze` kan gebruikt worden om na te gaan welke er geinstalleerd zijn.
    Als laatste werd `deactivate` gebruikt om een omgeving af te sluiten.



## Part 3: Explore Python Classes 3.4.6

- Findings and important commands:

    In dit lab werd een herhaling gegevens over functies, methodes en klasses binnen Python.
    
    De  `__init__() methode` werd aangeleerd, dit wordt uitgevoerd als een object gemaakt wordt op basis van deze class. 
    Het object wordt geinitialiseerd ('init'). 
    Het eerst argument van `__init__()` is self, verwijzend naar het object dat aangemaakt is.

    Het toevoegen van een methode aan een klasse werd als volgt gedaan:

    ```
    class Location:
        def __init__(self, name, country):
            self.name = name
            self.country = country

        def myLocation(self):
            print("Hi, my name is " + self.name + " and I live in " + self.country + ".")
            
    ```

    In bovenstaand voorbeeld is Location de naam van de klasse en myLocation is de methode.

    initiatie van de klasse genaamd Location kan dan als volgt:
    `loc1 = Location("Tomas", "Portugal")`
    Dit kan erna als volgt gebruikt worden: `loc1.myLocation()`


    Als laatste onderdeel in dit lab werd er gewerkt met het script circleClass.py.

    ```

    # Given a radius value, print the circumference of a circle.
    # Formula for a circumference is c = pi * 2 * radius

    class Circle:

        def __init__(self, radius):
            self.radius = radius

        def circumference(self):
            pi = 3.14
            circumferenceValue = pi * self.radius * 2
            return circumferenceValue

        def printCircumference(self):
            myCircumference = self.circumference()
            print ("Circumference of a circle with a radius of " + str(self.radius) + " is " + str(myCircumference))

    # First instantiation of the Circle class.
    circle1 = Circle(2)
    # Call the printCircumference for the instantiated circle1 class.
    circle1.printCircumference()

    # Two more instantiations and method calls for the Circle class.
    circle2 = Circle(5)
    circle2.printCircumference()

    circle3 = Circle(7)
    circle3.printCircumference()

    ```

    Uit het script is op te merken dat de klasse circle drie keer geinitialiseerd is en dat er 3 verschillende methodes (de init methode meegerekend) zijn gebruikt.

    Uitleg methodes:

    - De circumference berekent de omtrek en slaat de waarde op in de circumferenceValue variabele.
    
    - De printCircumference-methode drukt een string af. Hiervoor wordt `str()` gebruikt om de waarden van myCircumference en self.radius om te zetten naar een string.




    




    

