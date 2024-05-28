# Lab 4 oplossing

## Part 1: Network Infrastructure

 - Install and configure your lab test environment according to the network drawing.

    Voor de configuratie ben ik begonnen bij de switches te configureren en erna ben ik overgegaan tot de 2 routers.
    Doorheen de opdracht heb ik notities van vorige jaren erbij genomen voor commando's op te zoeken.
    Router 1 en 2 worden in poort 10 en 11 gepatched.
    
    Info voor patching in main rack:
    LAB-RA01-C02-R01 – Gig 0/0/1 - LAB-BR-A-SW08 – Fa 0/2
    LAB-RA01-C02-R02 – Gig 0/0/1 - LAB-BR-A-SW13 – Fa 0/2


 

Switch 1 Commando's:

```
Enable
configure terminal
hostname LAB-RA01-A02-SW01
no ip domain-lookup
enable secret cisco
ip domain name data.labnet.local
banner motd #
Unauthorized access is prohibited. #

crypto key generate rsa
2048
username cisco privilege 15 secret secret
line vty 0 15
transport input ssh
login local
exit

line console 0
password console
exit
enable secret secret
end

ip shh version 2
ip shh time-out 120
ip ssh authentication-retries 3
login block-for 180 attempts 3 within 50


vlan 15
name Management
vlan 16
name Data_Users
vlan 17
name Voice_Users
vlan 18
name Reserved
vlan 99
name Native
exit

interface range gigabitEthernet 1/0/1 - 19
switchport mode access
switchport access vlan 15
spanning-tree portfast
spanning-tree bpduguard enable
shutdown
exit
Interface gigabitEthernet 1/0/20
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15,16,17,18,99
no shutdown
exit

Interface range gigabitEthernet 1/0/21 - 22
Channel-group 1 mode active
exit

interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15,16,17,18,99
no shutdown
exit

Interface range gigabitEthernet 1/0/23 - 24
Channel-group 2 mode active
exit

interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15
no shutdown
exit

interface vlan 15
ip address 172.17.1.68 255.255.255.240
no shutdown
exit

spanning-tree VLAN 15 priority 4096
spanning-tree mode rapid-pvst
ip default-gateway 172.17.1.65
```



Switch 2 commando's:

```
Enable
configure terminal
no ip domain-lookup
hostname LAB-RA01-A01-SW02
enable secret cisco

ip domain name data.labnet.local

banner motd #
Unauthorized access is prohibited. #

crypto key generate rsa
2048
username cisco privilege 15 secret secret
line vty 0 15
transport input ssh
login local
exit

line console 0
password console
exit
enable secret secret
end

ip shh version 2
ip shh time-out 120
ip ssh authentication-retries 3
login block-for 180 attempts 3 within 50


interface vlan 15
ip address 172.17.1.69 255.255.255.240
no shutdown

interface range gigabitEthernet 1/0/23-24
channel-group 3 mode active
interface range gigabitEthernet 1/0/21-22
channel-group 2 mode active

interface port-channel 3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15,16,17,18
exit

interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15,16,17,18
exit

spanning-tree VLAN 15-18 priority 28672
spanning-tree mode rapid-pvst



Switch 3 is een oudere switch, gebruikte commando's zijn als volgt

```
Enable
configure terminal
no ip domain-lookup
hostname LAB-RA01-A01-SW03
enable secret cisco

ip domain name data.labnet.local

banner motd #
Unauthorized access is prohibited. #


username cisco privilege 15 secret secret
line vty 0 15
transport input ssh
login local
exit

line console 0
password console
exit
enable secret secret
end

ip shh version 2
ip shh time-out 120
ip ssh authentication-retries 3
login block-for 180 attempts 3 within 50


vlan 15
name Management
vlan 16
name Data_Users
vlan 17
name Voice_Users
vlan 18
name Reserved
vlan 99
name Native
exit

interface range fastEthernet 0/1 - 20
switchport mode access
switchport access vlan 15
spanning-tree portfast
spanning-tree bpduguard enable
shutdown
exit

interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 15
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown
exit

interface gigabitEthernet 0/2
switchport mode access
switchport access vlan 15
spanning-tree portfast
spanning-tree bpduguard enable
no shutdown
exit

Interface range fastEthernet 0/21 - 22
Channel-group 2 mode active
exit

interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15,16,17,18,99
no shutdown
exit

Interface range fastEthernet 0/23 - 24
Channel-group 3 mode active
exit

interface port-channel 3
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 15,16,17,18,99
no shutdown
exit

interface vlan 15
ip address 172.17.1.70 255.255.255.240
no shutdown
exit

spanning-tree VLAN 15 priority 32768
spanning-tree mode rapid-pvst
ip default-gateway 172.17.1.65


```




Router 1 commando's:

```
enable
conf t
hostname LAB-RA01-C02-R01

ip domain name data.labnet.local
crypto key generate rsa
2048
username cisco privilege 15 secret secret
line vty 0 4
transport input ssh

ip shh version 2
ip shh time-out 120
ip ssh authentication-retries 3
login block-for 180 attempts 3 within 50

interface gigabitEthernet 0/0/0
no shutdown
exit

interface gigabitEthernet 0/0/1
ip address 10.199.65.102 255.255.255.224
no shutdown
exit

interface G0/0/0.15
description vlan 15
encapsulation dot1q 15
ip add 172.17.1.65 255.255.255.240

standy 110 ip 172.17.1.65
standby 110 priority 110
standby 110 preempt
no shutdown
exit

interface G0/0/0.16
encapsulation dot1q 16
ip add 172.17.1.81 255.255.255.240
no shutdown
exit

interface G0/0/0.17
encapsulation dot1q 17
ip add 172.17.1.97 255.255.255.240
no shutdown
exit

interface G0/0/0.18
encapsulation dot1q 18
ip add 172.17.1.113 255.255.255.240
no shutdown
exit

interface G0/0/0.99
encapsulation dot1q 99 native
exit


ip route 172.17.1.64 255.255.255.240 10.199.65.200
ip route 172.17.1.80 255.255.255.240 10.199.65.200
ip route 172.17.1.96 255.255.255.240 10.199.65.200
ip route 172.17.1.112 255.255.255.240 10.199.65.200
#test ip route 0.0.0.0 0.0.0.0 10.199.65.200


```

Router 2 commando's:
```
enable
conf t
hostname LAB-RA01-C02-R02

ip domain name data.labnet.local
crypto key generate rsa
2048
username cisco privilege 15 secret secret
line vty 0 4
transport input ssh

ip shh version 2
ip shh time-out 120
ip ssh authentication-retries 3
login block-for 180 attempts 3 within 50



interface gigabitEthernet 0/0/0
no shutdown
exit

interface gigabitEthernet 0/0/1
ip address 10.199.65.202 255.255.255.224
no shutdown
exit


interface G0/0/0.15
encapsulation dot1q 15
ip address 172.17.1.67 255.255.255.240
no shutdown
exit

interface G0/0/0.16
encapsulation dot1q 16
ip address 172.17.1.83 255.255.255.240
no shutdown
exit

interface G0/0/0.17
encapsulation dot1q 17
ip address 172.17.1.99 255.255.255.240
no shutdown
exit

interface G0/0/0.18
encapsulation dot1q 18
ip address 172.17.1.115 255.255.255.240
no shutdown
exit

interface G0/0/0.99
encapsulation dot1q 99 native
no shutdown
exit


ip route 172.17.1.64 255.255.255.240 10.199.65.200
ip route 172.17.1.80 255.255.255.240 10.199.65.200
ip route 172.17.1.96 255.255.255.240 10.199.65.200
ip route 172.17.1.112 255.255.255.240 10.199.65.200
#test ip route 0.0.0.0 0.0.0.0 10.199.65.200

```



client instellingen:
```
IPV4: 172.17.1.71
SUBNETMASK: 255.255.255.240
DEFAULT-GATEWAY: 172.17.1.65
DNS: 10.199.64.66
```