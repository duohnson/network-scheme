# Network Scheme - Topologías y redes.

Documentación y estudio de redes.

## Mis Topologías de Red:

![Simulación de red actual](/Imagenes/Captura%202.png)
![Simulación de red básica](/Imagenes/test_lab.png)

## Descripción

Este es un repositorio de documentación, estudio, resúmenes y topologías de red.

## Estructura del Proyecto

```text
network-scheme/
├── Documentos/                 Guías de referencia y documentación técnica
├── Escaneo tShark/             Prácticas y capturas de tráfico con tShark
├── Imagenes/                   Referencias visuales y capturas de pantalla
├── Red WAN - Mega Topología/   Topología de red de área amplia (WAN)
├── Resumenes/                  Notas completas de estudio para CCNA
├── Topologías/                 Otras topologías de red en Packet Tracer
├── RED_SIMPLE.MD               Documentación de topología de red simple
├── network-isp.pkt             Simulación de red ISP en Cisco Packet Tracer
└── README.md                   Este archivo
```

## Archivos de Referencia

Los documentos PDF en `Documentos/` contienen comandos y configuraciones CCNA para enrutamiento, VLANs y seguridad básica.

## Requisitos

- Cisco Packet Tracer (versión 7.0 o superior)
- Conocimientos básicos de enrutamiento y NAT

========================================================================================================================================================

# Redes CCNA con CISCO Resumenes de Modulo 1

## Creditos a instructor de CISCO Ken Contreras en UCR.

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

## Filtros utiles en Wireshark:

![Wireshark](/Imagenes/tshark.jpg)