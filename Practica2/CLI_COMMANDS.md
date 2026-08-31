
## CONFIGURACIÓN SWITCH MULTICAPA DATACENTER

```cisco
enable
configure terminal
ip routing

interface FastEthernet0/1
 no switchport
 ip address 10.2.9.21 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 10.2.9.25 255.255.255.252
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 10.2.9.0 0.0.0.255
 exit
 
do write
```

## CONFIGURACIÓN SWITCH MULTICAPA PISO 1

```cisco
enable
configure terminal
ip routing

interface FastEthernet0/1
 no switchport
 ip address 10.2.9.1 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 10.2.9.5 255.255.255.252
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 10.2.9.0 0.0.0.255
 exit

do write
```

## CONFIGURACIÓN SWITCH MULTICAPA PISO 2

```cisco
enable
configure terminal
ip routing

interface range FastEthernet0/13 - 16
 no switchport
 channel-protocol lacp
 channel-group 1 mode active
 exit

interface Port-channel 1
 ip address 10.2.9.9 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/1
 no switchport
 ip address 192.198.29.1 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 192.198.29.129 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 192.198.29.0 0.0.0.255
 network 10.2.9.0 0.0.0.255
 exit

do write
```
## CONFIGURACIÓN SWITCH MULTICAPA PISO 3

```cisco
enable
configure terminal
ip routing

interface range FastEthernet0/17 - 20
 no switchport
 channel-protocol lacp
 channel-group 2 mode active
 exit

interface Port-channel 2
 ip address 10.2.9.13 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/1
 no switchport
 ip address 192.198.39.1 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 192.198.39.129 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 192.198.39.0 0.0.0.255
 network 10.2.9.0 0.0.0.255
 exit

do write
```

## CONFIGURACIÓN ROUTER1 (PISO 1) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.2 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.19
 encapsulation dot1Q 19
 ip address 192.198.19.66 255.255.255.240
 ip helper-address 192.198.100.130
 exit

interface GigabitEthernet0/1.29
 encapsulation dot1Q 29
 ip address 192.198.19.2 255.255.255.192
 ip helper-address 192.198.100.130
 exit

router eigrp 9
 network 192.198.19.0 0.0.0.255
 network 10.2.9.0 0.0.0.3
 exit

do write
```

## CONFIGURACIÓN ROUTER2 (PISO 1) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.6 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.19
 encapsulation dot1Q 19
 ip address 192.198.19.67 255.255.255.240
 ip helper-address 192.198.100.130
 exit

interface GigabitEthernet0/1.29
 encapsulation dot1Q 29
 ip address 192.198.19.3 255.255.255.192
 ip helper-address 192.198.100.130
 exit

router eigrp 9
 network 192.198.19.0 0.0.0.255
 network 10.2.9.4 0.0.0.3
 exit

do write
```

## CONFIGURACIÓN ROUTER1 (DATACENTER / BIBLIOTECA CENTRAL) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.22 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.39
 encapsulation dot1Q 39
 ip address 192.198.100.3 255.255.255.128
 exit

interface GigabitEthernet0/1.49
 encapsulation dot1Q 49 
 ip address 192.198.100.131 255.255.255.128
 exit

router eigrp 9
 network 192.198.100.0 0.0.0.255
 network 10.2.9.20 0.0.0.3
 exit

do write
```

## CONFIGURACIÓN ROUTER2 (DATACENTER / BIBLIOTECA CENTRAL) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.26 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.39
 encapsulation dot1Q 39
 ip address 192.198.100.4 255.255.255.128
 exit

interface GigabitEthernet0/1.49
 encapsulation dot1Q 49
 ip address 192.198.100.132 255.255.255.128
 exit

router eigrp 9
 network 192.198.100.0 0.0.0.255
 network 10.2.9.24 0.0.0.3
 exit

do write
```