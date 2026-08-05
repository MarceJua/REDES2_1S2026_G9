# Registro de Comandos CLI - Práctica 1

## Marcelo Juarez - 202010367

### Sección: Infraestructura Base, VLANs y VTP

#### 1.Configuración de los VTP Servers (Edificio Izquierdo y Derecho)

ejecutamos este bloque en el switch principal del lado izquierdo (SW0_G9) y el switch principal del lado derecho (SW13_G9).

```cisco
enable
configure terminal
enable secret redes2grupo9
vtp domain G9
vtp mode server
vlan 19
 name Primaria_Izq
vlan 29
 name Basicos_Izq
vlan 39
 name Bachillerato_Izq
vlan 69
 name Primaria_Der
vlan 79
 name Basicos_Der
vlan 89
 name Bachillerato_Der
exit
```

#### 2. Configuración de Puertos Troncales en los VTP Servers

```cisco
enable
configure terminal
interface range FastEthernet 0/1 - 3
 switchport mode trunk
exit
```

#### 3. Configuración de Switches Intermedios (Troncales y VTP Client)

Aplicado a todos los switches que solo interconectan a otros switches.

```cisco
enable
configure terminal
! 1. Configuramos Hostname (Ejemplo genérico)
hostname SW1_G9

! 2. Lo volvemos VTP Client para que aprenda las VLANs
vtp domain G9
vtp mode client

! 3. Configuramos SUS PROPIOS puertos (los que suben y los que bajan) como Trunk
interface range FastEthernet 0/1 - 3
 switchport mode trunk
exit
```

#### 4. Switches Inferiores (Conectados a PCs)

Distribución lógica aplicada en la topología según los requerimientos:

Edificio Izquierdo:
🟢 Círculo Verde (Primaria): VLAN 19
🟣 Círculo Rosa (Básicos): VLAN 29
🟠 Círculo Naranja (Bachillerato): VLAN 39

Edificio Derecho:
🟣 Círculo Rosa (PC6 - Primaria): VLAN 60 + 9 = VLAN 69
🟢 Círculo Verde (PC7 y Laptop2 - Básicos): VLAN 70 + 9 = VLAN 79
🟠 Círculo Naranja (PC3 y PC8 - Bachillerato): VLAN 80 + 9 = VLAN 89

Bloque de Comandos Base (Ejemplo para switch de VLAN 29):

```cisco
enable
configure terminal
hostname SW7_G9

! 1. Lo volvemos VTP Client
vtp domain G9
vtp mode client

! 2. Configurar los puertos hacia arriba como Trunk
interface range FastEthernet 0/1 - 2
 switchport mode trunk
exit

! 3. Configurar el puerto hacia el host final como Access
interface FastEthernet 0/10
 switchport mode access
 switchport access vlan 29
exit
```

## Susana Paola González Contreras - 202000576

### Sección: Inter-VLAN, Port-Security y Spanning Tree (STP)

#### 1. Configuración de Enrutamiento Inter-VLAN (Edificio Izquierdo)
Comandos implementados en el Router del lado izquierdo para levantar la interfaz física y configurar las subinterfaces correspondientes a Primaria, Básicos y Bachillerato, utilizando encapsulamiento dot1Q.

```cisco
enable
configure terminal
interface GigabitEthernet 0/1
 no shutdown
 exit

interface GigabitEthernet 0/1.19
 encapsulation dot1Q 19
 ip address 192.178.19.1 255.255.255.0
 exit

interface GigabitEthernet 0/1.29
 encapsulation dot1Q 29
 ip address 192.178.29.1 255.255.255.0
 exit

interface GigabitEthernet 0/1.39
 encapsulation dot1Q 39
 ip address 192.178.39.1 255.255.255.0
 exit
```
#### 2. Configuración de Enrutamiento Inter-VLAN (Edificio Derecho)
Comandos implementados en el Router del lado derecho para las subinterfaces de las VLANs correspondientes a ese edificio.

```cisco
enable
configure terminal
interface GigabitEthernet 0/1
 no shutdown
 exit

interface GigabitEthernet 0/1.69
 encapsulation dot1Q 69
 ip address 192.178.69.1 255.255.255.0
 exit

interface GigabitEthernet 0/1.79
 encapsulation dot1Q 79
 ip address 192.178.79.1 255.255.255.0
 exit

interface GigabitEthernet 0/1.89
 encapsulation dot1Q 89
 ip address 192.178.89.1 255.255.255.0
 exit
```


#### 3. Configuración de Seguridad y STP (Edificio Izquierdo - PVST)
Bloque de comandos utilizado para configurar la seguridad del puerto en modo acceso (VLAN 29) con límite de una dirección MAC dinámica (sticky) y apagado automático en caso de violación. Adicionalmente, se configura la modalidad Per-VLAN Spanning Tree.

```cisco
enable
configure terminal
interface FastEthernet 0/10
 switchport mode access
 switchport access vlan 29
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown
 exit

spanning-tree mode pvst
```
#### 4. Configuración de Seguridad y STP (Edificio Derecho - Rapid PVST)
Bloque de comandos utilizado para configurar la seguridad del puerto en modo acceso (VLAN 79) con la misma política de violación (shutdown). Se configura el protocolo de convergencia rápida Rapid PVST.
```cisco
enable
configure terminal
interface FastEthernet 0/10
 switchport mode access
 switchport access vlan 79
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown
 exit

spanning-tree mode rapid-pvst
```


## Jose David Mota González - 202306077

### Fase: Enrutamiento Dinámico y Pruebas Globales
A continuación se presenta la transcripción de los comandos utilizados para configurar el enrutamiento dinámico (EIGRP, RIP, OSPF) y la redistribución de rutas para garantizar la conectividad de extremo a extremo.

#### **Edificio Izquierdo: Router0 (EIGRP)**
```cisco
enable
config t
! Asignar IP al enlace hacia Router1
interface GigabitEthernet0/0
 ip address 10.10.12.1 255.255.255.0
 no shutdown
exit

! Configurar EIGRP
router eigrp 9
 network 10.10.12.0 0.0.0.255
 network 192.178.19.0 0.0.0.255
 network 192.178.29.0 0.0.0.255
 network 192.178.39.0 0.0.0.255
 no auto-summary
exit
```

#### **Núcleo de Red: Router1 (Traducción EIGRP - RIP)**

```cisco
enable
config t
! Asignar IP al enlace hacia Router0
interface GigabitEthernet0/0
 ip address 10.10.12.2 255.255.255.0
 no shutdown
exit

! Asignar IP al enlace hacia Router4
interface GigabitEthernet0/1
 ip address 10.10.11.1 255.255.255.0
 no shutdown
exit

! Configurar RIP y redistribuir EIGRP
router rip
 version 2
 network 10.10.11.0
 redistribute eigrp 9 metric 2
 no auto-summary
exit

! Configurar EIGRP y redistribuir RIP
router eigrp 9
 network 10.10.12.0 0.0.0.255
 redistribute rip metric 10000 100 255 1 1500
 no auto-summary
exit
```

#### **Núcleo de Red: Router4 (Traducción RIP - OSPF)**

```cisco
enable
config t
! Asignar IP al enlace hacia Router1
interface GigabitEthernet0/0
 ip address 10.10.11.2 255.255.255.0
 no shutdown
exit

! Asignar IP al enlace hacia Router3
interface GigabitEthernet0/1
 ip address 10.10.10.1 255.255.255.0
 no shutdown
exit

! Configurar RIP y redistribuir OSPF
router rip
 version 2
 network 10.10.11.0
 redistribute ospf 1 metric 2
 no auto-summary
exit

! Configurar OSPF y redistribuir RIP
router ospf 1
 network 10.10.10.0 0.0.0.255 area 0
 redistribute rip subnets
exit
```
#### **Edificio Derecho: Router3 (OSPF)**

```cisco
enable
config t
! Asignar IP al enlace hacia Router4
interface GigabitEthernet0/0
 ip address 10.10.10.2 255.255.255.0
 no shutdown
exit

! Configurar OSPF
router ospf 1
 network 10.10.10.0 0.0.0.255 area 0
 network 192.178.69.0 0.0.0.255 area 0
 network 192.178.79.0 0.0.0.255 area 0
 network 192.178.89.0 0.0.0.255 area 0
 passive-interface default
 no passive-interface GigabitEthernet0/0
exit
```

