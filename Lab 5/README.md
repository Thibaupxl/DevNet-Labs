# Lab 5 oplossing

## Part 1: Software Version Control with GitLab netacad 3.3.11

- Findings and important commands:

    In dit lab werd er geleerd om met Git te werken.
    Alle commando's werden uitgevoerd op de voorziene DEVASC VM.
    Als eerste werd geleerd hoe een Git repo geinitialiseerd kan worden, als tweede werd staging en commits aangeleerd.
    
    ```
    git init: voor een repository te initaliseren
    git status: toont de gewijzigde bestanden die voorbereid zijn voor een volgende commit
    git add DEVASC.txt: voor staging
    git commit -m "Committing DEVASC.txt to begin tracking changes"
    git log: voor een geschiedenis van alle commits te tonen
    git checkout feature: om van branch te wisselen, in dit geval naar de branch genaamd 'feauture'
    git merge feature: Voegt de inhoud uit de feature branch samen met de master branch
    ```


    Er werd ook geleerd hoe een conflict opgelost kon worden, met behulp van het `cat` commando en `vim` werd het bestand aangepast om het conflict op te lossen.
    
    Daarna werd dit gestaged en gecommit met het commando: `git commit -a -m "Manually merged from test branch"` naar de master branch.

    Als laatste werd er uitgelegd hoe een Github account aangemaakt kon worden en werd er een repo aangemaakt waarin een bestand DEVASC.txt toegevoegd werd.



## Part 2: Create a Python Unit TestLab netacad: 3.5.7

- Findings and important commands:

    In dit lab werd er geleerd over Unit testing binnen Python.

    What unittest class do you use to create an individual unit of testing?
    TestCase, hiermee kunnen nieuwe testcases aangemaakt worden.

    How does the test runner know which methods are a test?
    Methods whose names start with the letters test_ informs the test runner about which methods are tests.

    What command will list all of the command line options for unittest shown in the following output?
    `python3 -m unittest -h`

    Hierna werd er met het  test_data.py bestand gewerkt.
    
    De functie json_search() werd aangemaakt als volgt:

    ```python
    from test_data import *
    def json_search(key,input_object):
        ret_val=[]
        if isinstance(input_object, dict): # Iterate dictionary
            for k, v in input_object.items(): # searching key in the dict
                if k == key:
                    temp={k:v}
                    ret_val.append(temp)
                if isinstance(v, dict): # the value is another dict so repeat
                    json_search(key,v)
                elif isinstance(v, list): # it's a list
                    for item in v:
                        if not isinstance(item, (str,int)): # if dict or list repeat
                            json_search(key,item)
        else: # Iterate a list because some APIs return JSON object in a list
            for val in input_object:
                if not isinstance(val, (str,int)):
                    json_search(key,val)
        return ret_val
    print(json_search("issueSummary",data))
    ```

    Ik heb dit script uitgevoerd via vscode:
    ![jsonsearch.py](/afbeeldingen/lab5_4.png)


    Hierna werd er een unittest aangemaakt om na te gaan of de functie effectief werkt

    Binnen deze test werden de volgende 3 methodes gebruikt
     - Voor een gegeven sleutel in het JSON-object, kijk na of de testcode ook deze sleutel kan vinden.
     - Voor een niet-bestaande sleutel in het JSON-object, kijk of de testcode ook bevestigt dat er geen sleutel is
     - Controleer of de functie een lijst geeft, dit zou altijd het geval moeten zijn.

     Na het uitvoeren van de unit test zien we dat deze faalt:
     key should be found, return list should not be empty ... FAIL


     Hierna wordt het script gecorrigeerd naar:

         ```python
    from test_data import *
    ret_val=[]
    def json_search(key,input_object):
        if isinstance(input_object, dict): # Iterate dictionary
            for k, v in input_object.items(): # searching key in the dict
                if k == key:
                    temp={k:v}
                    ret_val.append(temp)
                if isinstance(v, dict): # the value is another dict so repeat
                    json_search(key,v)
                elif isinstance(v, list): # it's a list
                    for item in v:
                        if not isinstance(item, (str,int)): # if dict or list repeat
                            json_search(key,item)
        else: # Iterate a list because some APIs return JSON object in a list
            for val in input_object:
                if not isinstance(val, (str,int)):
                    json_search(key,val)
        return ret_val
    print(json_search("issueSummary",data))
    ```

    Er wordt een tweede error gevonden, de ret_val is nu een globale variable en de waarde wordt hierdoor behouden.
    Om dit laatste op te lossen is er een outer function toegevoegd als volgt:

    ```python
    from test_data import *
    def json_search(key,input_object):
        ret_val=[]
        def inner_function(key,input_object):
            if isinstance(input_object, dict): # Iterate dictionary
                for k, v in input_object.items(): # searching key in the dict
                    if k == key:
                        temp={k:v}
                        ret_val.append(temp)
                    if isinstance(v, dict): # the value is another dict so repeat
                        json_search(key,v)
                    elif isinstance(v, list): # it's a list
                        for item in v:
                            if not isinstance(item, (str,int)): # if dict or list repeat
                                json_search(key,item)
            else: # Iterate a list because some APIs return JSON object in a list
                for val in input_object:
                    if not isinstance(val, (str,int)):
                        json_search(key,val)
        inner_function(key,input_object)
        return ret_val
    print(json_search("issueSummary",data))
    ```

    Na deze twee aanpassingen slaagt de test, het print statement wordt nu normaal verwijderd.



## Part 3: Parse Different Data Types with PythonLab netacad: 3.6.6

- Findings and important commands:

    In het eerste deel van dit lab werd er geleerd om een XML bestand te parsen met een Python script.

    Ik heb dit script genaamd parsexml uitgevoerd in vscode als volgt:
    ![parsexml.py](/afbeeldingen/lab5_1.png)


    In het tweede deel van dit lab werd er geleerd om een JSON bestand te parsen met een Python script.

    Ik heb dit script genaamd parsjson uitgevoerd in vscode als volgt:
    ![parsejson.py](/afbeeldingen/lab5_2.png)

    Hierna werden de volgende 2 print statements toegevoegd aan het script, hierdoor wordt de output als YAML data weergegeven
        ```python
        print("\n\n---")
        print(yaml.dump(ourjson))
        ```
    
    In het laatste deel van het lab werd er YAML data geparsed en weergegeven als JSON data.
    Ook dit laatste script heb ik uitgevoerd in vscode:
    ![parseyaml.py](/afbeeldingen/lab5_3.png)








    

