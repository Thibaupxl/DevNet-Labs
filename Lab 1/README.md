# Lab 1 oplossing

## 1.1 Install different tools/packages on Ubuntu DEVASC-LABVM:

- Task preparation and implementation:

	- Voor de taak uit te voeren worden de commando's die nodig zijn opgezocht, dit zijn:
        - `sudo apt update` kan gebruikt worden om alles up te daten.
        - Met `sudo apt install python3.8` wordt de gevraagde versie van python geinstalleerd.
        - `sudo apt install python3-pip` voor PIP.
        - Visual Studio Code is reeds geinstalleerd.
        - `pip3 install jupyter` kan gerunt worden voor jupyter te installeren.
        - `sudo apt install idle-python3.8`voor idle-python te installeren.


- Task troubleshooting:

    Er was weinig troubleshooting nodig, enkel bij het commando `pip3 install jupyter` kwamen er enkele errors, maar nader inzien zijn deze vanzelf opgelost erna.
    Dit is nagegaan door het commando opnieuw te runnen waarna er ook geen errors meer waren.

    ![Screenshot van Errors](/afbeeldingen/Lab1_1.1.png)


- Task verification:

    Enkel idle-python en jupyter was nog niet geinstalleerd op mijn VM.
    De installaties kunnen nagegaan worden door de commando's te gebruiken:

    ![verificatie installatie](/afbeeldingen/lab1_2.png)




## 1.2 Run geopy and timedate Python Scipts on the DEVASC-LABVM using the tools above (1.1):

- Task preparation and implementation:

    Ik heb eerst de 3 nodige scripts (timedate.py, geopy-geocoders_location.py, location.py) geopend op: https://github.com/KathleenHombroeks/PythonExperiments/tree/main/mixed

    - Visual Studio Code:

        Een nieuwe file aangemaakt en hierin de code geplakt uit timedate.py, geopy-geocoders_location.py en location.py
        Hierna werd telkens de code gerunt door deze te selecteren en op run te klikken.

    - Jupyter:

    	- Jupyter Notebook gestart via terminal door `jupyter notebook` in te geven.
		- `%run timedate.py` ingeven voor het script te runnen.
		- `%run geopy-geocoders_location.py` ingeven voor het script te runnen.
        - `%run location.py` ingeven voor het script te runnen.

    - Python IDLE:

    	- Python IDLE gestart via terminal door idle-python3.8 in te geven.
		- Het script timedate.py ingevoerd en uitgevoerd.
		- Het script geopy-geocoders_location.py ingevoerd en uitgevoerd.
		- Het script location.py ingevoerd en uitgevoerd.


- Task troubleshooting:

    - Er was weinig troubleshooting nodig, enkel voor geopy was er nog geen module geinstalleerd en kwam er de melding `ModuleNotFoundError: No module named 'geopy'`.
    - Dit was op te lossen door dit te installeren met: `sudo pip3 install geopy`


- Task verification:

    - Python code runne in Visual Studio

    ![Python code in visual studio](/afbeeldingen/lab1_1.2.png)

    - Python code runne in Python IDLE

    ![Python code in Python IDLE](/afbeeldingen/lab1_1.3.png)




## 1.3 Install different tools/packages on Windows OS (deep dive exercise) ++

- Task preparation and implementation:

    De gewenste Python versie kan gedownload worden op: https://www.python.org/downloads/windows/, Visual studio Code op https://code.visualstudio.com/ en Jupyter Notebook via `pip install notebook`. Python IDLE is reeds inbegrepen bij Python.
    
    - Python 3.8 and PIP, Python IDLE
        Dit programma was reeds geinstalleerd op mijn apparaat.

    
    - Visual Studio Code:
        Dit programma was reeds geinstalleerd op mijn apparaat.


    - Jupyter, Python IDLE:
        Dit programma was reeds geinstalleerd op mijn apparaat.


- Task troubleshooting:

    Troubleshooting was nergens nodig bij het uitvoeren van deze taken, alle programmas waren reeds geinstalleerd op mijn windows apparaat.
    Ik heb wel enkele geupdate, zoals `python -m pip install --upgrade pip` en van Python is reeds versie 3.10 geinstalleerd.


- Task verification:
    Dat Python, IDLE en Jupyter werkend is kan nagegaan worden door in de terminal het volgende uit te voeren:
    ![Verificate 1.3](/afbeeldingen/lab1_1.4.png)





## 1.4 Install different tools/packages on Ubuntu 22.04.01 LTS (deep dive exercise) ++

- Task preparation and implementation:



- Task troubleshooting:



- Task verification: