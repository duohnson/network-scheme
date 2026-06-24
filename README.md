# Network Scheme - Topologías y redes.

Documentación y estudio de redes.

## Mis Topologías de Red:

![Simulación de red actual](/Imagenes/Captura%202.png)
![Simulación de red básica](/Imagenes/test_lab.png)

## Descripción

Este esquema de red implementa:
- Router ISP (1941) simulando un proveedor de internet
- Servidores DNS y WEB conectados al ISP
- Router empresarial con PC cliente
- Enrutamiento dinámico y NAT para acceso a internet

## Estructura del Proyecto

```
network-scheme/
├── network-isp.pkt          Simulación principal en Cisco Packet Tracer
├── test_lab/
│   └── test.pkt             Archivo de prueba
├── Documentos/              Guías de referencia CCNA
│   ├── CCNA-Comandos.pdf
│   ├── CCNA-Configuraciones.pdf
│   ├── CCNA-Enrutamientos y VLANs.pdf
│   └── CCNA-Configuraciones basicas y seguridad.pdf
└── Imagenes/                Referencias visuales
    ├── Tipos de cable.png
    └── Capturas de pantalla
etc.
```

## Configuración Básica

### 1. Tipos de Cable a Usar

- **Copper Straight-Through**: PC-Switch, Server-Switch, Switch-Router
- **Serial DCE**: Router-Router (con especificación de velocidad de reloj) (Opcional, yo no lo use.)

Para ver los tipos de cable, ![Tipos de cables](/Imagenes/Tipos%20de%20cable.png)

Y su documento de referencia en el siguiente link: [Tipos de Cable](Documentos/Tipos_de_cable.pdf).

### 2. Configurar Router ISP

```
conf t
interface g0/1
 ip address 56.23.17.1 255.255.255.0
 no shutdown

interface g0/0
 ip address 200.100.50.1 255.255.255.0
 no shutdown

ip route 0.0.0.0 0.0.0.0 200.100.50.1
```

*Nota: La interfaz g0/1 es el gateway para los servidores DNS/WEB y el router empresa. Configurar DHCP si es necesario para asignar IPs a los servidores.*

### 3. Configurar NAT en Router Empresa

```
conf t
interface g0/1
 ip address 10.64.20.1 255.255.255.0
 ip nat inside
 no shutdown

interface g0/0
 ip address 56.23.17.x 255.255.255.0
 ip nat outside
 no shutdown

access-list 1 permit 10.64.20.0 0.0.0.255
ip nat inside source list 1 interface g0/0 overload
ip route 0.0.0.0 0.0.0.0 56.23.17.1
```

*Nota: Cambiar `x` por la IP asignada y las direcciones 10.64.20.0 si hay conflictos*

### 4. Configurar DHCP en Router Empresa

```
conf t
ip dhcp pool POOL-EMPRESA
 network 10.64.20.0 255.255.255.0
 default-router 10.64.20.1
 dns-server 56.23.17.2
 exit

ip dhcp excluded-address 10.64.20.1 10.64.20.10
```

*Nota: El servidor DNS (56.23.17.2) será asignado a los clientes DHCP*

### 5. Configurar Servidor DNS

En Cisco Packet Tracer, en el servidor DNS:
- IP Address: 56.23.17.2
- Subnet Mask: 255.255.255.0
- Default Gateway: 56.23.17.1

Servicios habilitados:
- DNS: activado
- Agregar registros DNS según sea necesario

### 6. Configurar Servidor WEB

En Cisco Packet Tracer, en el servidor WEB:
- IP Address: 56.23.17.3
- Subnet Mask: 255.255.255.0
- Default Gateway: 56.23.17.1

Servicios habilitados:
- HTTP: activado
- HTTPS: activado (opcional)

### 7. Configurar PC en modo DHCP

En Cisco Packet Tracer, en la PC (PC1):
1. Desktop tab
2. IP Configuration
3. Seleccionar DHCP
4. Esperar a que reciba IP automáticamente del router

O configurar manualmente si lo prefieres:
- IP Address: 10.64.20.x (donde x es un número disponible)
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.64.20.1
- DNS Server: 56.23.17.2

### 8. Topología de Red

```
        [ISP ROUTER 1941]
             |
         [SW-ISP]
        /        \
     DNS        WEB

    [EMPRESA ROUTER]
          |
    [SW-EMPRESA]
          |
         PC1
```

## Como nos quedaría con la configuracion basica:

![Simulación de red](/Imagenes/Captura%202.png)

## Archivos de Referencia

Los documentos PDF en `Documentos/` contienen comandos y configuraciones CCNA para enrutamiento, VLANs y seguridad básica.

## Requisitos

- Cisco Packet Tracer (versión 7.0 o superior)
- Conocimientos básicos de enrutamiento y NAT

Una vez completado esto, puedes agregar más dispositivos, configurar VLANs o implementar seguridad adicional según tus necesidades.

========================================================================================================================================================

# Redes CCNA con CISCO Resumenes de Modulo 1

## Creditos a profesor de Cisco Ken Contreras en UCR.

## Notas de Estudio CCNA 

En la carpeta de Resumenes encontrarás mis notas completas del curso CCNA, tanto las notas originales de clase como versiones avanzadas expandidas con mucho más detalle.

### Módulo 1 - Fundamentos de Redes CCNA

#### Inicio Rápido
- **[Definición de Redes](/Resumenes/Modulo%201/General.md)** - Conceptos iniciales: dispositivos, IPs, QoS, tipos de red
- **[Conceptos de Subneteo](/Resumenes/Modulo%201/Subneteo_Conceptos.md)** - VLSM, CIDR y cálculo práctico de subredes

#### Capítulos 1-3: Protocolos y Modelos de Red
| Recurso | Descripción |
|---------|-------------|
| [Clase 1](./Resumenes/Modulo%201/Notas%20de%20clase%201.txt) | Protocolos, Capa OSI, TCP/IP |
| [Clase 2](./Resumenes/Modulo%201/Notas%20de%20clase%202.txt) | Suite de Protocolos |
| [Clase 3](./Resumenes/Modulo%201/Notas%20de%20clase%203.txt) | Modelo OSI, PDU, Encapsulación |
| **[→ Nota Avanzada](./Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%201.md)** | **Elementos de comunicación, requisitos de protocolos, tipos de entrega, convergencia** |

#### Capítulos 4-5: Capa de Enlace de Datos
| Recurso | Descripción |
|---------|-------------|
| [Clase 4](./Resumenes/Modulo%201/Notas%20de%20clase%204%20-%20ETHERNET%20SWITCHING.txt) | Ethernet, ARP, Direcciones MAC |
| [Clase 5](./Resumenes/Modulo%201/Notas%20de%20clase%205%20-%20ACCESO%20AL%20MEDIO.txt) | Control de Acceso, CSMA |
| **[→ Nota Avanzada](./Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%204-5.md)** | **Direcciones MAC en profundidad, ARP con ejemplos, tabla de conmutación, CSMA/CD vs CSMA/CA, VLAN** |

#### Capítulos 6-7: Capa Física y Medios
| Recurso | Descripción |
|---------|-------------|
| [Clase 6](./Resumenes/Modulo%201/Notas%20de%20clase%206.txt) | Medios inalámbricos, cables |
| [Clase 6 Ext.](./Resumenes/Modulo%201/Notas%20de%20clase%206%20parte%202%20mas%20extendido.txt) | Modelo OSI, Capa de Red |
| **[→ Nota Avanzada](./Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%206-7.md)** | **Cobre (tipos, PoE), Fibra (monomodo/multimodo), WiFi (bandas, estándares), codificación, conectores** |

#### Capítulos 8-9: Capa de Red (IPv4/IPv6)
| Recurso | Descripción |
|---------|-------------|
| [Clase 7](./Resumenes/Modulo%201/Notas%20de%20clase%207.txt) | IPv6, NDP, Direccionamiento IPv4 |
| [Clase 8](./Resumenes/Modulo%201/Notas%20de%20clase%208.txt) | IPv6 avanzado |
| **[→ Nota Avanzada](./Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%208-9.md)** | **IPv4 completo, CIDR, subneteo VLSM con ejemplos, IPv6 estructura jerárquica, NDP, migración, routing, ICMP** |

#### Capítulos 14-15: Capa de Transporte y Aplicación
| Recurso | Descripción |
|---------|-------------|
| [Clase 9](./Resumenes/Modulo%201/Notas%20de%20clase%209.txt) | TCP/UDP, puertos |
| [Clase 10](./Resumenes/Modulo%201/Notas%20de%20clase%2010.txt) | Protocolos de aplicación |
| **[→ Nota Avanzada](./Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%2014-15.md)** | **TCP vs UDP, handshake, control de flujo, HTTP/HTTPS, DNS, DHCP, SMTP/POP3/IMAP, FTP/SFTP, SSH, flujo completo** |

#### Capítulo 11: Configuración de Dispositivos
| Recurso | Descripción |
|---------|-------------|
| [Clase 11](./Resumenes/Modulo%201/Notas%20de%20clase%2011.txt) | Interfaz, Telnet, SSH |
| **[→ Nota Avanzada](./Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%2011.md)** | **Modos IOS, IPv4/IPv6, seguridad (console/enable/VTY), usuarios, Telnet vs SSH, backup, comandos útiles** |

#### Ver todas las notas completas
→ **[Acceder a Resumenes/README.md](./Resumenes/README.md)** para guía completa y estructura detallada

========================================================================================================================================================
