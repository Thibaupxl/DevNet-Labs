# Lab 6 oplossing

## Part 1: Connecting to a single iOS device

- Findings and important commands:

    In dit deel van het lab werd er geleerd om met Netmiko te werken. 
    Als eerste moet dit geinstalleerd worden indien het nog niet op de PC staat via `pip install netmiko`.
    Op de VM staat het reeds geinstalleerd.
    
    Om het lab uit te voeren wordt er in vscode gewerkt, in Python kan de Netmiko library geimporteerd worden. 
    Zo is er oa ConnectHandler geimporteerd voor SSH te gebruiken: `from netmiko import ConnectHandler`.

    Erna worden een apparaat of apparaten gekozen om verbinding mee te maken, deze worden hier in een rij opgesomd met een reeks parameters voor elk apparaat.

    ```
    devices = [
        {
        'device_type': 'cisco_ios',
        'host':   'IPAdres', # 172.17.4.69
        'username': 'cisco',
        'password': 'cisco',
        'port' : 22,          
        'secret': 'cisco',
        },
        {
        'device_type': 'cisco_ios',
        'host':   'IPAdres',
        'username': 'cisco',
        'password': 'cisco',
        'port' : 22,          
        'secret': 'cisco',
        }
    ] 
    ```

    Erna kan verbinding gemaakt worden via SSH te gebruiken met behulp van commando:

    ```
    for device in devices:
    ssh = ConnectHandler(**device)
    ssh.enable()
    ```

    Sending single show command kan als volgt:

    ```
    output=sshCli.send_command("show ip interface brief")
    print(output)
    ```

    Sending multiple show commands:

    ```
    show_commands = ["show interfaces", "show version"]
    show_output = send_multiple_show_commands(show_commands)
    print(show_output)
    ```


## Part 2 + 3 : Connect to multiple IOS-XE devices

- Findings and important commands:

    In dit deel van het lab werd er geleerd om met meerdere apparaten verbinding te maken met Netmiko.
    
    Send show commands to multiple devices:

    ```
    def send_show_commands_to_multiple_devices(ip_addresses, commands):
        output = ""
        for ip_address in ip_addresses:
            device = {
                'device_type': 'cisco_ios',
                'host':   'IPAdres',
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
    ```

    Deze functie zou als volgt gerund kunnen worden:
    ```
    ip_addresses = ['172.17.4.69', '172.17.4.70]
    commands = ['show version', 'show interfaces']
    
    output = send_show_commands_to_multiple_devices(ip_addresses, commands)
    print(output)
    ```

    Send configuration commands to multiple devices

    ```
    def configuration_commands(ip_addresses, commands):
    for ip_address in ip_addresses:
        device = {
            'device_type': 'cisco_ios',
            'host':   'IPAdres',
            'username': 'cisco',
            'password': 'cisco',
            'port' : 22,          
            'secret': 'cisco',
        }
        ssh = ConnectHandler(**device)
        output += ssh.send_config_set(commands)
        ssh.disconnect()

        ip_addresses = ['172.17.4.69', '172.17.4.70']
        commands = ['no shutdown', 'no shutdown']
    ```


    Run show commands and save the output
    Bovenstaand commando zou gewijzigd kunnen worden door als commands `commands = ['show interfaces', 'show version']` te gebruiken en erna de output op te slaan als een text bestand met 
    ```
    with open(filename, 'w') as f:
        f.write(output)
    print(f"Output saved to {filename}")
    filename = 'output.txt'
    ```

    Backup the device configurations
    De device config kan bekeken worden doorshow running-config te gebruiken. Hier kan het commando alweer lichtjes aangepast worden en de code `output = ssh.send_command("show running-config")` gebruikt worden.


    Configure a subset of Interfaces

    ```
     for interface in interface_list:
            config_commands = [
                f"interface {interface}",
                "no shutdown",
            ]
            output = net_connect.send_config_set(config_commands)
            print(f"Configuration applied to {interface} on {device_ip}")
    net_connect.disconnect()
    
    device_ip = '172.17.4.69'
    interface_list = ['G0/1', 'G0/2']
    configure_interfaces(device_ip, interface_list)
    ```


    Send device configuration using an external file

    Voor deze vraag heb ik beroep gedaan op de bestanden https://github.com/KathleenHombroeks/PythonExperiments/blob/main/netmiko/iosv_l2-confi.txt en https://github.com/KathleenHombroeks/PythonExperiments/blob/main/netmiko/switch%20config%20script.txt .

    De inhoud van `netmiko/iosv_l2-confi.txt` kan aangepast worden naar een gewenste configuratie.


    ```
    from netmiko import ConnectHandler

    iosv_l2_s1 = {
	    'device_type': 'cisco_ios",
	    'ip': '172.17.4.69',
	    'username': 'cisco',
	    'password': 'cisco',
    }

    iosv_l2_s2 = {
	    'device_type': 'cisco_ios",
	    ip': '172.17.4.69',
	    'username': 'cisco',
	    'password': 'cisco',
    }


    with open('iosv_l2-config') as f:
	    lines = f.read().splitlines()
    print lines

    all_devices = [iosv_l2s_s2, iosv_l2_s1]

    for devices in all_devices
	    net_connect = ConnectHandler(**devices)
	    output = net_connect>send_config_set(lines)
	    print output
    ```


    Execute a script with a Function or classes
    / Er zijn reeks scripten geschreven met functies hierboven.


    Execute a script with a statements (if, ifelse, else)
    We kunnen 1 van de bovenstaande scripts aanpassen door bijvoorbeelde een if else toe te voegen op basis van IP:

    ```
    ip_range = ipaddress.ip_network('172.17.4.0/24')
    
    for device in devices:
    if ipaddress.ip_address(device['host']) in ip_range:
        ssh = ConnectHandler(**device)
        ssh.enable()

        interface_status = send_show_command("show interfaces status")
        print(f"Device {device['host']} - Interface Status:\n{interface_status}")
    else:
        print(f"Device {device['host']} is not in the IP range {ip_range}")
    ```


## Part 4: Create an challenging excited script as a network 


```
from netmiko import ConnectHandler


devices = [
    {
        'device_type': 'cisco_ios',
        'host': '172.17.4.69',
        'username': 'cisco',
        'password': 'cisco',
        'port': 22,
        'secret': 'cisco',
    },
    {
        'device_type': 'cisco_ios',
        'host': '172.17.4.70',
        'username': 'cisco',
        'password': 'cisco',
        'port': 22,
        'secret': 'cisco',
    }
]


ip_range = ipaddress.ip_network('172.17.4.0/24')


def send_show_command(ssh, command):
    output = ssh.send_command(command)
    return output


for device in devices:
    if ipaddress.ip_address(device['host']) in ip_range:
        ssh = ConnectHandler(**device)
        ssh.enable()

        interface_status = send_show_command(ssh, "show interfaces status")
        print(f"Device {device['host']} - Interface Status:\n{interface_status}")

        ssh.disconnect()
    else:
        print(f"Device {device['host']} is not in the IP range {ip_range}")
```






