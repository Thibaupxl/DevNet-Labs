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








    

