## Diseño de Arquitectura y Subnetting (VLSM y FLSM)

| Ubicación | VLAN | Subred Asignada | Máscara / CIDR | Rango de Hosts Utilizables |
| :--- | :--- | :--- | :--- | :--- |
| Piso 1 | Estudiantes (29) | 192.198.19.0 | 255.255.255.192 (/26) | 192.198.19.1 - 192.198.19.62 |
| Piso 1 | Admin (19) | 192.198.19.64 | 255.255.255.240 (/28) | 192.198.19.65 - 192.198.19.78 |
| Piso 2 | WLAN1 | 192.198.29.0 | 255.255.255.128 (/25) | 192.198.29.1 - 192.198.29.126 |
| Piso 2 | WLAN2 | 192.198.29.128 | 255.255.255.128 (/25) | 192.198.29.129 - 192.198.29.254 |
| Piso 3 | WLAN1 | 192.198.39.0 | 255.255.255.128 (/25) | 192.198.39.1 - 192.198.39.126 |
| Piso 3 | WLAN2 | 192.198.39.128 | 255.255.255.128 (/25) | 192.198.39.129 - 192.198.39.254 |
| Datacenter | Web (39) | 192.198.100.0 | 255.255.255.128 (/25) | 192.198.100.1 - 192.198.100.126 |
| Datacenter | DHCP (49) | 192.198.100.128 | 255.255.255.128 (/25) | 192.198.100.129 - 192.198.100.254 |

## Tabla de Asignación de Subredes (/30)
| Enlace Punto a Punto | Subred Asignada | Rango Utilizable | Configuración de IPs |
| :--- | :--- | :--- | :--- |
| Multicapa Piso 1 - Router0 | 10.2.9.0/30 | 10.2.9.1 - 10.2.9.2 | Multicapa: 10.2.9.1 / Router0: 10.2.9.2 |
| Multicapa Piso 1 - Router1 | 10.2.9.4/30 | 10.2.9.5 - 10.2.9.6 | Multicapa: 10.2.9.5 / Router1: 10.2.9.6 |
| Multicapa Datacenter - Router2 | 10.2.9.20/30 | 10.2.9.21 - 10.2.9.22 | Multicapa: 10.2.9.21 / Router2: 10.2.9.22 |
| Multicapa Datacenter - Router3 | 10.2.9.24/30 | 10.2.9.25 - 10.2.9.26 | Multicapa: 10.2.9.25 / Router3: 10.2.9.26 |