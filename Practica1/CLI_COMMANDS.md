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

## Jose David Mota González - 202306077

### Sección: x

## Susana Paola González Contreras - 202000577

### Sección: x
