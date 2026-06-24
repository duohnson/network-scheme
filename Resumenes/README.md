# Notas de Estudio CCNA - Resumenes

Este repositorio contiene mis notas completas del curso CCNA (Cisco Certified Network Associate), incluyendo tanto los resúmenes de clase como notas avanzadas ampliadas con más profundidad.

## Estructura del Repositorio

```
Resumenes/
└── Modulo 1/
    ├── General.md                           (Conceptos generales)
    ├── Subneteo_Conceptos.md               (Subneting avanzado)
    ├── Notas de clase 1-11.txt             (Notas originales por clase)
    └── Nota avanzada de clase [X-Y].md    (Versiones expandidas)
```

---

## Módulo 1: Fundamentos de Redes

### Conceptos Introductorios
- **[Definición de Redes](/Resumenes/Modulo%201/General.md)** - Dispositivos, IPs, QoS, tipos de red, convergencia
- **[Conceptos de Subneteo](/Resumenes/Modulo%201/Subneteo_Conceptos.md)** - VLSM, CIDR, cálculo de subredes

### Capítulos 1-3: Protocolos y Modelos de Red
**Notas Originales:**
- [Clase 1 - Protocolos y Capa OSI](/Resumenes/Modulo%201/Notas%20de%20clase%201.txt)
- [Clase 2 - Suite de Protocolos](/Resumenes/Modulo%201/Notas%20de%20clase%202.txt)
- [Clase 3 - Modelo OSI y Encapsulación](/Resumenes/Modulo%201/Notas%20de%20clase%203.txt)

**Nota Avanzada:**
- [**Nota Avanzada - Capítulos 1-3**](/Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%201.md)
  - Elementos de comunicación profundizados
  - Requisitos de protocolos explicados
  - Tipos de entrega (unicast, multicast, broadcast)
  - Protocolo OSI vs TCP/IP
  - Convergencia de redes

### Capítulos 4-5: Capa de Enlace de Datos
**Notas Originales:**
- [Clase 4 - Ethernet Switching](/Resumenes/Modulo%201/Notas%20de%20clase%204%20-%20ETHERNET%20SWITCHING.txt)
- [Clase 5 - Control de Acceso al Medio](/Resumenes/Modulo%201/Notas%20de%20clase%205%20-%20ACCESO%20AL%20MEDIO.txt)

**Nota Avanzada:**
- [**Nota Avanzada - Capítulos 4-5**](/Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%204-5.md)
  - Direcciones MAC en profundidad
  - ARP (Address Resolution Protocol) con ejemplos
  - Tabla de conmutación en switches
  - Conmutación vs Enrutamiento
  - CSMA/CD y CSMA/CA
  - Estructura de tramas Ethernet
  - VLAN introducción

### Capítulos 6-7: Capa Física y Medios de Transmisión
**Notas Originales:**
- [Clase 6 - Medios Inalámbricos](/Resumenes/Modulo%201/Notas%20de%20clase%206.txt)
- [Clase 6 Extendida - Capa de Red](/Resumenes/Modulo%201/Notas%20de%20clase%206%20parte%202%20mas%20extendido.txt)

**Nota Avanzada:**
- [**Nota Avanzada - Capítulos 6-7**](/Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%206-7.md)
  - Capa Física detallada
  - Cobre: tipos de cables, PoE
  - Fibra Óptica: monomodo vs multimodo
  - Inalámbrico: bandas, estándares WiFi
  - Codificación de tramas
  - Conectores y interfaces
  - Velocidades de transmisión

### Capítulos 8-9: Capa de Red (IPv4 e IPv6)
**Notas Originales:**
- [Clase 7 - IPv6 y Direccionamiento IPv4](/Resumenes/Modulo%201/Notas%20de%20clase%207.txt)
- [Clase 8 - IPv6 Avanzado](/Resumenes/Modulo%201/Notas%20de%20clase%208.txt)

**Nota Avanzada:**
- [**Nota Avanzada - Capítulos 8-9**](/Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%208-9.md)
  - IPv4: estructura y clases
  - CIDR y subneteo VLSM
  - Ejemplo práctico de subneteo
  - IPv6: estructura jerárquica
  - Tipos de direcciones IPv6 (GUA, ULA, LLA)
  - NDP (Neighbor Discovery Protocol)
  - Técnicas de migración IPv4→IPv6
  - Tabla de enrutamiento
  - ICMP, Ping, Traceroute

### Capítulos 14-15: Capa de Transporte y Aplicación
**Notas Originales:**
- [Clase 9 - TCP/UDP](/Resumenes/Modulo%201/Notas%20de%20clase%209.txt)
- [Clase 10 - Capa de Aplicación](/Resumenes/Modulo%201/Notas%20de%20clase%2010.txt)

**Nota Avanzada:**
- [**Nota Avanzada - Capítulos 14-15**](/Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%2014-15.md)
  - TCP vs UDP comparación completa
  - Three-Way Handshake
  - Ventanas deslizantes (TCP control de flujo)
  - Puertos: bien conocidos, registrados, dinámicos
  - HTTP/HTTPS: métodos, códigos de respuesta, TLS
  - DNS: proceso de resolución, registros
  - DHCP: proceso DORA
  - FTP/SFTP: diferencias
  - SMTP/POP3/IMAP: correo electrónico
  - SSH: autenticación segura
  - Flujo de datos completo (ejemplo: acceder a Google)

### Capítulo 11: Configuración de Dispositivos
**Notas Originales:**
- [Clase 11 - Interfaz, Telnet, SSH](/Resumenes/Modulo%201/Notas%20de%20clase%2011.txt)

**Nota Avanzada:**
- [**Nota Avanzada - Capítulo 11**](/Resumenes/Modulo%201/Nota%20avanzada%20de%20clase%2011.md)
  - Modos de Cisco IOS
  - Configuración de interfaces IPv4/IPv6
  - Securización: console, enable, VTY
  - Usuarios locales y autenticación
  - Telnet vs SSH
  - Configuración SSH paso a paso
  - Switch: configuración de acceso remoto
  - Comandos de verificación útiles
  - Guardado de configuración
  - Backup y restauración

---

## Cómo Usar Estas Notas

### Para Principiantes
1. Lee primero **General.md** para entender conceptos básicos
2. Lee las **notas de clase originales** en orden (Clase 1 → 11)
3. Consulta **notas avanzadas** cuando necesites más profundidad

### Para Revisión Rápida
- Usa **notas de clase originales** para recordar conceptos
- Usa **notas avanzadas** para profundizar en un tema

### Para Preparación CCNA
1. Lee todas las **notas avanzadas** en orden
2. Realiza ejercicios prácticos con Cisco Packet Tracer
3. Verifica con comandos reales en laboratorio
4. Consulta **notas originales** si necesitas aclaración

---

## Complementos

### Archivos en el Repositorio
- **Topologías/** - Archivos .pkt para Cisco Packet Tracer
- **Documentos/** - Guías de referencia CCNA
- **Imagenes/** - Diagramas y referencias visuales

### Recursos Externos Recomendados
- Cisco Learning Network
- Cisco Documentation Library
- Packet Tracer (simulador oficial Cisco)
- Professor Messer (YouTube)

---

## Estructura de las Notas Avanzadas

Cada nota avanzada sigue este formato:

1. **Introducción** - Contexto del tema
2. **Conceptos fundamentales** - Base teórica
3. **Explicaciones profundas** - Detalles técnicos
4. **Diagramas y ejemplos** - Visualización
5. **Tablas de referencia** - Comparativas
6. **Resumen** - Puntos clave
7. **Referencias** - RFCs y estándares

---

xd