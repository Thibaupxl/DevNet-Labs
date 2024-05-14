# Lab 7 oplossing

## Part 1: Install the CSR1000v VM

- Task preparation and implementation:

    Voor dit lab werden eerst alle nodige bestanden gedownload voor de CSR1000v VM aan te maken.

- Task troubleshooting:

    In dit lab was er onverwacht lang troubleshooting nodig, ik heb tot driemaal toe de installatie van de CSR1000v uitgevoerd op VirtualBox.
    Ik heb tijdens deze troubleshooting de instellingen van de host only adapter nagegaan in Windows en alle mogelijke instellingen van de adapter in VirtualBox.
    Na wat rondzoeken op het internet kwam ik erop uit dat nog enkelen hetzelfde probleem hadden, de enige constante was het gebruik van een bepaalde versie 7.X van VirtualBox. Hierna heb ik besloten om het te proberen in VMWare vooraleer een andere versie van VirtualBox te downloaden.

- Task verification:

    Alle bestanden waaronder het VM bestand werd gedownload uit de folder op blackboard. Erna werd de VM toegevoegd met de nodige ISO aan VirtualBox.
    De VM werd opgestart en de host adapter werd nagegaan.
    `show ip interface brief` werd uitgevoerd in de terminal maar vertoonde geen IP adres.

    
    Ik ben uiteindelijk verder gegaan in VMWare, de installatie is hier zonder problemen gelukt.
    Pingen vanuit de VM in VirtualBox naar de CSR1000v VM in VMWare lukt ook.
    Erna werd de verbinding getest met SSH en ook via de WebUI van de CSR1000v. 
    Als laatste werd ook met de eigen pc de WebUI getest.


## Part 2: Explore YANG Models

- Task preparation and implementation:

    Tijdens dit lab werd er gewerkt met YANG en een pyang module wat het werken met YANG bestanden vergemakkelijkt.
    Hiervoor werd er op https://github.com/YangModels/yang de nodige module gedownload.
    Er werd een folder aangemaakt in de DEVASC VM waarin lokaal een versie van ietf-interfaces.yang in bewaard werd om mee te werken.

- Task troubleshooting:

    In dit lab was er geen troubleshooting nodig, het lab vroeg om commando's uit te testen door ze te kopieren uit de opgave en te plakken.

- Task verification:

     pyang is reeds geinstalleerd op de VM en kan geupdate worden met `pip3 install pyang --upgrade`. pyang werd hierdoor geupdate van 2.2.1 naar 2.6.0.
     Hierna werd het command `pyang -f tree ietf-interfaces.yang` gerunt om het yang model in een tree format weer te geven.


## Part 3: Use NETCONF to Access an IOS XE Device

- Findings and important commands:

    In dit deel van het lab werd er geleerd om met NETCONF te werken.
    Er werd eerst nagegaan of netconf al draaiende was op de VM via `show platform software yang-management process`, bij mij was dit reeds het geval.
    Er is een SSH verbinding gemaakt met de Devasc VM naar de CSR1000V VM.

    Er werden een reeks XML berichten gekopieerd naar de SSH sessie waaronder onderstaande bericht voor informatie van de interfaces op te halen

    ```
    <rpc message-id="103" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <get>
            filter>
                <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces"/>
            </filter>
        </get>
    </rpc>
    ]]>]]>
    ```

    Voor de output weer te geven in een meer leesbaar formaat heb ik het gekopieerd naar deze web tool: https://www.samltool.com/prettyprint.php

    Hieronder is een screenshot die de werkende SSH verbinding aantoont tussen beide VMs.
    ![SSH tussen VMs](/afbeeldingen/lab7_1.png)


    Na dit deel van de opdracht is er verder gegaan met de ncclient Python module.
    `pip3 list --format=columns | more` uitvoeren in de terminal toonde dat de module al geinstalleerd was.

    Hierna is een script gebruikt mbv ncclient om met de NETCONF service te verbinden.
    Dit wordt aangemaakt in een nieuwe folder en direct getest.

    Hierna wordt met ncclient de configuratie van CSR1kv opgehaald met een get_config() methode en geformatteerd met de xml.dom.minidom module.

    De output is zeer lang en werd met https://www.samltool.com/prettyprint.php weer leesbaarder gemaakt.

    Erna wordt in het lab getoond hoe de output door gebruik van Python leesbaarder gemaakt kan worden ipv een externe tool.
    Dit werd gedaan door onderstaande code toe te voegen.

    ```
    import xml.dom.minidom
    print(xml.dom.minidom.parseString(netconf_reply.xml).toprettyxml())
    ```

    Vervolgens wordt een filter via get_config() gebruikt voor 1 specifiek YANG model op te vragen ipv de volledige configuratie.
    Hierna werd ncclient gebruikt om de hostnaam van CSR1000v te veranderen met de edit_config() methode.

    Als voorlaatste werd ncclient gebruikt voor een nieuwe loopback interface te maken.

    Het allerlaats deel in part 3 was een uitdaging om het geschreven Python programma verder uit te breiden.
    Het originele Python script herhaalde een groot deel van de code om zo een tweede loopback interface aan te maken.

    In plaats van deze herhaling is het script aangepast door eerst aan de gebruiker te vragen hoeveel interfaces er gewenst zijn en met welke data.


```
from ncclient import manager
import xml.dom.minidom

def edit_config(m, config):
    return m.edit_config(target="running", config=config)

def create_loopback_config(name, description, address, mask):
    return f"""
<config>
    <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
        <interface>
            <Loopback>
                <name>{name}</name>
                <description>{description}</description>
                <ip>
                    <address>
                        <primary>
                            <address>{address}</address>
                            <mask>{mask}</mask>
                        </primary>
                    </address>
                </ip>
            </Loopback>
        </interface>
    </native>
</config>
"""

m = manager.connect(
    host="192.168.40.128",
    port=830,
    username="cisco",
    password="cisco123!",
    hostkey_verify=False
)

print("# Supported Capabilities (YANG models):")
for capability in m.server_capabilities:
    print(capability)

netconf_reply = m.get_config(source="running")
print(xml.dom.minidom.parseString(netconf_reply.xml).toprettyxml())

netconf_filter = """
<filter>
    <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native" />
</filter>
"""
netconf_reply = m.get_config(source="running", filter=netconf_filter)
print(xml.dom.minidom.parseString(netconf_reply.xml).toprettyxml())

netconf_hostname = """
<config>
    <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
        <hostname>CSR1kv</hostname>
    </native>
</config>
"""
netconf_reply = edit_config(m, netconf_hostname)
print(xml.dom.minidom.parseString(netconf_reply.xml).toprettyxml())

aantal_interfaces = int(input("Kies het aantal gewenste loopback interfaces : "))

for i in range(aantal_interfaces):
    name = input("Kies een naam: ")
    description = input("Geef de gewenste omschrijving: ")
    address = input("Geef het gewenste IP adres: ")
    mask = input("Geef het subnet masker: ")

    loopback_config = create_loopback_config(name, description, address, mask)
    netconf_reply = edit_config(m, loopback_config)
    print(xml.dom.minidom.parseString(netconf_reply.xml).toprettyxml())
```


## Part 4: Use RESTCONF to Access an IOS XE Device

- Task preparation and implementation:

    In dit deel van het lab werd er geleerd om met het RESTCONF-protocol RESTful API-oproepen te doen naar een IOS XE-apparaat.
    Postman werd gebruikt voor API-requests en achteraf werd een Python script gebruikt.
    


- Task troubleshooting:

    De week ervoor verliep de SSH verbinding zonder problemen, 1 week later kreeg ik de volgende foutmelding:
    ```
    RSA host key for 192.168.40.128 has changed and you have requested strict checking.
    Host key verification failed.
    ```

    Dit was oa op te lossen door de vorige keygen te verwijderen met `ssh-keygen -f "/home/devasc/.ssh/known_hosts" -R "192.168.40.128"`

    Hieronder is een screenshot weergegeven die de foutboodschap en de oplossing toont.
    ![TroubleshootingSSH](/afbeeldingen/lab7_2.png)

    Verder in de taak was er ook een kleine troubleshooting tijdens het gebruik van postman, dit omdat in de Devasc VM het wachtwoord al vooringesteld was maar met een hoofdletter in het begin ipv een kleine letter zoals in de opdracht.





- Task verification:

    Connectiviteit nagegaan `show ip interface brief` uit te voeren in de terminal van de CSR1000v om het IP adres te bekomen en erna te pingen vanuit de devasc VM.
    Erna was een korte troubleshooting nodig, hierboven uitgelegd.


    De HTTPS en RESTCONF service kon als volgt aangezet worden via de terminal van de CSR1000v VM.

    ```
    configure terminal
    restconf
    ip http secure-server
    ip http authentication local
    exit
    show platform software yang-management process
    ```

    Dit gaf de volgende output:

    ![http_restconf_enable_output](/afbeeldingen/lab7_3.png)


    Hierna werd Postman gebruikt voor Get Requests en Put requests.
    De commando's uit het lab werden gekopieerd en het IP adres paste ik telkens aan naar 192.168.40.128.


    Bij het Put Request in Postman werd er een loopback interface aangemaakt met IP 10.1.1.1 als volgt:
    ![put_request_postman](/afbeeldingen/lab7_4.png)




    In het laatste deel van het lab werden er 2 python script gemaakt, eentje voor een Get request en een voor een Put request uit te voeren.

    Achteraf werd er met `show ip interface brief` nagegaan of het gelukt was het gevraagde interface toe te voegen met het Python script.
    Ik heb beide scripts opgeslaan in een nieuw aangemaakte folder op de VM en in VScode uitgevoerd.

    Het laatste Put request via een Python script is hieronder  weergegeven.
    ![put_request_python](/afbeeldingen/lab7_5.png)





   

    

