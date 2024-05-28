# Evaluatie: praktijk opdracht

3: Script voor met netmiko shopw ip interfaces te zien van alle apparaten


```
from netmiko import ConnectHandler


# lijst maken met alle IPs in
ip_addresses = ['172.17.1.65','172.17.1.66','172.17.1.67','172.17.1.68', '172.17.1.69''172.17.1.70']

#gewenst commando
command = ['show ip interface']

def send_show_command_to_multiple_devices(ip_addresses, command):
    output = ""
    for ip_address in ip_addresses:
        device = {
            'device_type': 'cisco_ios',
            'host':   '172.17.1.65',  #dit gedeelte klopt niet, hier moet de lijst van IP's komen iv 1 enkel IP
            'username': 'cisco',
            'password': 'cisco',
            'port' : 22,          
            'secret': 'cisco',
        }
        ssh = ConnectHandler(**device)
        ssh.enable()
        for command in commands:
            output += ssh.send_command(command)
        ssh.disconnect()
    return output


output = send_show_commands_to_multiple_devices(ip_addresses, command)
print(output)
```


4: Configureer op de router een loopback interface met adres 200.200.200.200, met RESTCONF

```
import json 
import requests 
requests.packages.urllib3.disable_warnings() 
 
api_url = "https://172.17.1.65/restconf/data/ietf-interfaces:interfaces/interface=Loopback2"   #ip adres van router waarop loopback toegevoegd wordt
 
headers = { "Accept": "application/yang-data+json",  
            "Content-type":"application/yang-data+json" 
           } 
 
basicauth = ("cisco", "secret") 
 
yangConfig = { 
    "ietf-interfaces:interface": { 
        "name": "Loopback2", 
        "description": "RESTCONF loopback", 
        "type": "iana-if-type:softwareLoopback", 
        "enabled": True, 
        "ietf-ip:ipv4": { 
            "address": [ 
                { 
                    "ip": "200.200.200.200",  #gewenst IP adres
                    "netmask": "255.255.255.0" 
                } 
            ] 
        }, 
        "ietf-ip:ipv6": {} 
    } 
} 
 
resp = requests.put(api_url, data=json.dumps(yangConfig), auth=basicauth, 
headers=headers, verify=False) 
 
if(resp.status_code >= 200 and resp.status_code <= 299): 
    print("STATUS OK: {}".format(resp.status_code)) 
else: 
    print('Error. Status Code: {} \nError message: {}'.format(resp.status_code,resp.json()))
```
