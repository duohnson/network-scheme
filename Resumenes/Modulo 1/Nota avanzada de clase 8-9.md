# Nota Avanzada - Capítulos 8-9: Capa de Red (IPv4 e IPv6)

## Introducción

La Capa de Red (Capa 3 del modelo OSI) es donde ocurre la "magia" del enrutamiento y la conectividad global. Este documento profundiza en IPv4 e IPv6, dos protocolos que hacen posible Internet.

---

## 1. Capa de Red (OSI Capa 3)

### 1.1 Funciones Principales

```
┌──────────────────────────────────┐
│ Capa de Red (Capa 3)             │
├──────────────────────────────────┤
│ Procesos Básicos:                │
│ • Direccionamiento (Asignar IP)  │
│ • Encapsulación (Agregar header) │
│ • Enrutamiento (Dirigir tráfico) │
│ • Desencapsulación (Quitar hdrs) │
│                                  │
│ PDU: PAQUETE                     │
│ Dispositivos: Routers            │
│ Protocolos: IP, ICMP, Enrutamiento
└──────────────────────────────────┘
```

### 1.2 Relación con Otras Capas

```
┌─────────────────────────┐
│ Capa 4 (Transporte)     │
│ TCP/UDP + Datos         │
└────────────┬────────────┘
             ↓ Encapsulación
┌─────────────────────────┐
│ Capa 3 (Red)            │
│ IP + TCP/UDP + Datos    │
│ [IP Header agregado]    │
└────────────┬────────────┘
             ↓ Encapsulación
┌─────────────────────────┐
│ Capa 2 (Enlace)         │
│ MAC + IP + TCP + Datos  │
│ [MAC Header agregado]   │
└─────────────────────────┘
```

---

## 2. IPv4 - Protocolo de Internet Versión 4

### 2.1 Estructura de una Dirección IPv4

```
32 bits totales / 4 octetos

Decimal:    192.168.1.1
Binario:    11000000.10101000.00000001.00000001
Hexadecimal: C0.A8.01.01

cada octeto representa valores de 0-255
```

### 2.2 Conceptos Fundamentales

#### Red, Máscara y Broadcast

```
Ejemplo: 192.168.1.0 /24

Dirección de Red:    192.168.1.0
┌─ Identifica la red
└─ Todos los hosts en esta parte están en la misma red local

Máscara de Subred:   255.255.255.0  (en notación decimal)
                     11111111.11111111.11111111.00000000 (binario)
┌─ Primeros 24 bits = RED (no se pueden cambiar)
└─ Últimos 8 bits = HOSTS (se pueden cambiar)

Dirección de Broadcast: 192.168.1.255
┌─ Todos los bits de host = 1
└─ Se envía a TODOS los dispositivos de la red

Rango de Hosts Utilizables:
1° Utilizable:  192.168.1.1    (red + 1)
Últimas Utilizables: 192.168.1.254  (broadcast - 1)

Direcciones que NO se usan:
- 192.168.1.0   (dirección de red)
- 192.168.1.255 (dirección de broadcast)

Total de direcciones: 256 (2^8)
Útiles: 254 (256 - 2)
```

### 2.3 Clases de Direccionamiento IPv4

#### Visión Histórica (Obsoleto, pero importante conocer)

```
Basado en primeros bits de la dirección:

CLASE A:  0xxxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
         ├─ Rango: 0.0.0.0 a 127.255.255.255
         ├─ Máscara por defecto: /8 (255.0.0.0)
         ├─ Máx hosts por red: 16,777,214
         ├─ Ejemplo: 10.0.0.0 (privada)
         └─ Uso: Redes muy grandes

CLASE B:  10xxxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
         ├─ Rango: 128.0.0.0 a 191.255.255.255
         ├─ Máscara por defecto: /16 (255.255.0.0)
         ├─ Máx hosts por red: 65,534
         ├─ Ejemplo: 172.16.0.0 (privada)
         └─ Uso: Redes empresariales

CLASE C:  110xxxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
         ├─ Rango: 192.0.0.0 a 223.255.255.255
         ├─ Máscara por defecto: /24 (255.255.255.0)
         ├─ Máx hosts por red: 254
         ├─ Ejemplo: 192.168.0.0 (privada)
         └─ Uso: Redes pequeñas/PYMES

CLASE D:  1110xxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
         ├─ Rango: 224.0.0.0 a 239.255.255.255
         ├─ Uso: MULTICAST
         └─ Ejemplo: 239.255.255.255

CLASE E:  1111xxxx.xxxxxxxx.xxxxxxxx.xxxxxxxx
         ├─ Rango: 240.0.0.0 a 255.255.255.255
         ├─ Uso: EXPERIMENTAL/RESERVADO
         └─ No se usa en Internet
```

#### Direcciones Privadas (RFC 1918)

```
Redes destinadas para uso privado (no ruteables en Internet):

Clase A privada:     10.0.0.0/8
                     De 10.0.0.0 a 10.255.255.255
                     (16,777,216 direcciones)

Clase B privada:     172.16.0.0/12
                     De 172.16.0.0 a 172.31.255.255
                     (1,048,576 direcciones)

Clase C privada:     192.168.0.0/16
                     De 192.168.0.0 a 192.168.255.255
                     (65,536 direcciones)

Loopback:            127.0.0.0/8
                     De 127.0.0.1 a 127.255.255.255
                     Usado para ping a sí mismo
                     127.0.0.1 = localhost

Link-local (APIPA):  169.254.0.0/16
                     Asignado automáticamente si no hay DHCP
                     De 169.254.0.1 a 169.254.255.254
```

### 2.4 CIDR (Classless Inter-Domain Routing)

```
Notación moderna que ignora clases:

Formato: IP/PREFIX_LENGTH

Ejemplos:
192.168.1.0/24    ← 254 hosts (para PYMES)
192.168.1.0/25    ← 126 hosts (mitad de red)
192.168.1.0/26    ← 62 hosts (cuarta parte)
10.0.0.0/8        ← 16,777,214 hosts (para ISPs)
10.0.0.0/16       ← 65,534 hosts (redes grandes)
10.0.0.0/24       ← 254 hosts (subnets dentro de 10.0)

Ventajas sobre clases:
✓ Flexibilidad completa
✓ Uso eficiente de direcciones
✓ Facilita agregación de rutas
✓ Estándar moderno (RFC 4632)
```

### 2.5 Subneteo (VLSM)

#### ¿Por qué subneteamos?

```
Problema: Tenemos una red grande que necesita dividir
Solución: Subneteamos = dividimos en redes más pequeñas

Ejemplo real de empresa:

Red asignada: 10.0.0.0/16

Necesitamos:
- Piso 1: 50 hosts
- Piso 2: 100 hosts
- Piso 3: 30 hosts
- WiFi: 20 hosts
```

#### Proceso de Subneteo

```
PASO 1: Ordenar requisitos de mayor a menor
100 hosts  ← Mayor
50 hosts
30 hosts
20 hosts   ← Menor

PASO 2: Calcular cuántos bits necesito para cada una
Para 100 hosts:   2^n ≥ 100 + 2 (red + broadcast)
                  2^7 = 128 ✓  (7 bits para hosts)
                  Máscara: 32 - 7 = /25

Para 50 hosts:    2^n ≥ 50 + 2
                  2^6 = 64 ✓  (6 bits para hosts)
                  Máscara: 32 - 6 = /26

Para 30 hosts:    2^n ≥ 30 + 2
                  2^5 = 32 ✓  (5 bits para hosts)
                  Máscara: 32 - 5 = /27

Para 20 hosts:    2^n ≥ 20 + 2
                  2^5 = 32 ✓  (5 bits para hosts)
                  Máscara: 32 - 5 = /27

PASO 3: Asignar subredes secuencialmente

Red 1 (100 hosts):
├─ Dirección: 10.0.0.0/25
├─ Máscara: 255.255.255.128
├─ Rango: 10.0.0.0 - 10.0.0.127
├─ 1° util: 10.0.0.1
├─ Última util: 10.0.0.126
└─ Broadcast: 10.0.0.127
        ↓ Siguiente = Broadcast + 1 = 10.0.0.128

Red 2 (50 hosts):
├─ Dirección: 10.0.0.128/26
├─ Máscara: 255.255.255.192
├─ Rango: 10.0.0.128 - 10.0.0.191
├─ 1° util: 10.0.0.129
├─ Última util: 10.0.0.190
└─ Broadcast: 10.0.0.191
        ↓ Siguiente = 10.0.0.192

Red 3 (30 hosts):
├─ Dirección: 10.0.0.192/27
├─ Máscara: 255.255.255.224
├─ Rango: 10.0.0.192 - 10.0.0.223
├─ 1° util: 10.0.0.193
├─ Última util: 10.0.0.222
└─ Broadcast: 10.0.0.223
        ↓ Siguiente = 10.0.0.224

Red 4 (20 hosts):
├─ Dirección: 10.0.0.224/27
├─ Máscara: 255.255.255.224
├─ Rango: 10.0.0.224 - 10.0.0.255
├─ 1° util: 10.0.0.225
├─ Última util: 10.0.0.254
└─ Broadcast: 10.0.0.255
```

#### Tabla de Referencia Rápida

```
Hosts Necesarios    Bits Necesarios    Máscara           /Notación
1-2                 1                  255.255.255.254   /31
3-6                 2                  255.255.255.252   /30
7-14                3                  255.255.255.248   /29
15-30               4                  255.255.255.240   /28
31-62               5                  255.255.255.224   /27
63-126              6                  255.255.255.192   /26
127-254             7                  255.255.255.128   /25
255-510             8                  255.255.255.0     /24
```

### 2.6 Campos del Paquete IPv4

```
Estructura del encabezado IPv4 (20 bytes mínimo):

┌────────────────────────────────────────────────────┐
│ Vers │ IHL  │ DSCP │ ECN │ Longitud Total (16 bits)│
├────────────────────────────────────────────────────┤
│ Identificación (16 bits) │ Flags │ Offset Fragto  │
├────────────────────────────────────────────────────┤
│ TTL (8 bits) │ Protocolo (8 bits) │ Checksum (16) │
├────────────────────────────────────────────────────┤
│ Dirección IP Origen (32 bits)                      │
├────────────────────────────────────────────────────┤
│ Dirección IP Destino (32 bits)                     │
├────────────────────────────────────────────────────┤
│ Opciones (variable, generalmente vacío)            │
└────────────────────────────────────────────────────┘

Versión (4 bits):
├─ Identifica versión de IP
└─ IPv4 = 4

IHL - Internet Header Length (4 bits):
├─ Longitud del encabezado en palabras de 32 bits
├─ Mínimo: 5 (20 bytes)
├─ Máximo: 15 (60 bytes con opciones)
└─ Se multiplica por 4 para obtener bytes

DSCP - Differentiated Services Code Point (6 bits):
├─ QoS (Calidad de Servicio)
├─ Define prioridad del paquete
└─ 0 = mejor esfuerzo, 63 = máxima prioridad

TTL - Time To Live (8 bits):
├─ Número de saltos máximo
├─ Comienza típicamente en 64 o 128
├─ Se decrementa en 1 por cada router
├─ Si llega a 0 → se descarta
└─ Previene loops infinitos en Internet

Protocolo (8 bits):
├─ 1 = ICMP (ping, traceroute)
├─ 6 = TCP
├─ 17 = UDP
└─ Otros valores para otros protocolos

Checksum (16 bits):
├─ Detección de errores del encabezado
├─ Se recalcula en cada router (TTL cambia)
└─ Si no coincide → se descarta el paquete

Opciones (variable):
├─ No obligatorio
├─ Ejemplos: timestamp, grabación de ruta
└─ Aumenta tamaño del encabezado
```

### 2.7 Fragmentación de Paquetes IPv4

```
Problema: MTU (Maximum Transmission Unit)
┌─────────────────────────────────────┐
│ Cada red tiene un MTU máximo:        │
├─────────────────────────────────────┤
│ Ethernet:    1500 bytes             │
│ WiFi:        1500 bytes (típico)    │
│ PPP:         1492 bytes             │
│ Jumbo:       9000 bytes             │
└─────────────────────────────────────┘

Solución 1: Fragmentación por router
Router recibe paquete de 2000 bytes, pero MTU es 1500
├─ Fragmenta en 2 paquetes
├─ Paquete 1: 1500 bytes
├─ Paquete 2: 500 bytes
└─ Se reensamblá en destino

Campo Identificación: Agrupa fragmentos del mismo paquete
Campo Flags: More Fragments (MF)
├─ MF = 1: Hay más fragmentos
└─ MF = 0: Este es el último

Solución 2: Path MTU Discovery
├─ Envía paquete con DF (Don't Fragment) = 1
├─ Si es muy grande, router responde con ICMP
├─ Emisor reduce tamaño
└─ Más eficiente que fragmentación
```

---

## 3. IPv6 - Protocolo de Internet Versión 6

### 3.1 ¿Por qué IPv6?

```
Problema con IPv4:
├─ Solo 4.3 mil millones de direcciones (2^32)
├─ Agotamiento de direcciones globales
├─ Requiere NAT como parche temporal
├─ No escalable para IoT

Solución IPv6:
├─ 340 mil trillones de direcciones (2^128)
├─ Una dirección por dispositivo
├─ Simplifica enrutamiento y configuración
├─ Mejor seguridad (IPSec integrado)
└─ Mejor QoS y multimedia
```

### 3.2 Estructura de Dirección IPv6

```
128 bits totales / 8 hextetos (grupos de 4 hex)

Notación estándar:
2001:0DB8:0000:1111:0000:0000:0000:0200

Simplificación 1: Eliminar ceros a la izquierda de cada hexteto
2001:DB8:0:1111:0:0:0:200

Simplificación 2: Remplacer ceros consecutivos con ::
2001:DB8::1111:0:0:0:200
    ↓
2001:DB8::1111::200  ← INCORRECTO (máximo una vez por dirección)
    ↓
Debe ser:
2001:DB8::1111:0:0:0:200  ← Correcto

Caso especial - Address con muchos ceros:
::1  = 0000:0000:0000:0000:0000:0000:0000:0001 (Loopback)
::   = 0000:0000:0000:0000:0000:0000:0000:0000 (Sin especificar)
```

### 3.3 Estructura de una Dirección IPv6

```
128 bits divididos en 3 partes:

2001:0DB8:ACAD:0001:0000:0000:0000:1000/64

├─ Prefijo de Enrutamiento Global (Primeros 48 bits)
│  ├─ Asignado por RIR (Regional Internet Registry)
│  ├─ Asignado a ISP o corporación
│  └─ Ejemplo: 2001:0DB8:ACAD::/48
│
├─ ID de Subred (Siguientes 16 bits)
│  ├─ Asignado por la empresa
│  ├─ Permite crear hasta 65536 subredes
│  └─ Ejemplo: :0001:/64
│
└─ ID de Interfaz (Últimos 64 bits)
   ├─ Identifica el host específico
   ├─ Puede ser automático (basado en MAC)
   └─ Ejemplo: :0000:0000:0000:1000

Ejemplo de Estructura Jerárquica:
2001:0DB8:ACAD: │  │  │  │  │  │  │
ISP    REGION   │  │  │  │  │  │  │
               /48│  │  │  │  │  │
                  /64 (empresa asigna subredes)
                     /80 (departamentos)
                        /96 (pisos)
                           /100 (secciones)
```

### 3.4 Tipos de Direcciones IPv6

#### GUA (Global Unicast Address) - Públicas

```
Rango: 2000::/3 (2000 a 3FFF)

Características:
├─ Direcciones públicas globales
├─ Ruteables en Internet
├─ Equivalente a IPv4 público
└─ Ejemplo: 2001:0DB8:ACAD:0001:0000:0000:0000:0100

Formato registrado:
├─ ISP obtiene de RIR: /32 a /35
├─ Asigna a cliente: /48 a /56
├─ Cliente asigna: /64 a /80 para subredes
```

#### ULA (Unique Local Address) - Privadas

```
Rango: FC00::/7 (FC00 a FDFF)

Características:
├─ Direcciones privadas
├─ NO ruteables en Internet público
├─ Equivalente a 10.0.0.0/8 en IPv4
├─ Globalmente únicas (muy probable)
└─ Ejemplo: FD12:3456:789A:0001::1

Comparación con RFC 1918 (IPv4):
IPv4 Privada          IPv6 Privada (ULA)
10.0.0.0/8     ←→    FC00::/7
172.16.0.0/12  ←→    FC00::/7
192.168.0.0/16 ←→    FC00::/7
```

#### LLA (Link-Local Address)

```
Rango: FE80::/10

Características:
├─ Dirección más privada
├─ Válida SOLO en el enlace local (misma subnet)
├─ No ruteables
├─ Automáticamente configurada
├─ Usada para DHCP, vecinos
└─ Ejemplo: FE80::1

Derivación automática:
├─ Parte 1: FE80::0000:0000:0000
├─ Parte 2: Basada en MAC del dispositivo
│           Ejemplo MAC: 00:1A:2B:3C:4D:5E
│           Genera:      0000:021A:2BFF:FE3C:4D5E
│           Resultado:   FE80::021A:2BFF:FE3C:4D5E
```

#### Multicast

```
Rango: FF00::/8

Características:
├─ Uno a varios
├─ Todos comienzan con FF
└─ Ejemplo: FF02::1 (todos en enlace local)

Multicast Especiales:
FF02::1  = Todos los nodos del enlace local
FF02::2  = Todos los routers del enlace local
FF02::1:FF00:0 a FF02::1:FFFF:FFFF = Solicitud Vecinos (similar ARP)
```

#### Loopback

```
Dirección: ::1/128

Características:
├─ Ping a sí mismo
├─ Equivalente a 127.0.0.1 en IPv4
```

#### Sin Especificar

```
Dirección: ::/128

Características:
├─ Dirección nula
├─ Usado solo como origen temporalmente
├─ Equivalente a 0.0.0.0 en IPv4
```

### 3.5 NDP (Neighbor Discovery Protocol) - IPv6

```
Reemplaza ARP de IPv4

Componentes (ICMP v6 tipos específicos):

1. Router Solicitation (RS) - Tipo 133
   ├─ "¿Hay algún router en esta red?"
   └─ Enviado por host al iniciar

2. Router Advertisement (RA) - Tipo 134
   ├─ Router responde anunciando su presencia
   ├─ Incluye información de configuración
   └─ Periódicamente multicast

3. Neighbor Solicitation (NS) - Tipo 135
   ├─ "¿Quién tiene la IPv6 2001:DB8::100?"
   ├─ Similar a ARP Request de IPv4
   └─ Usado para descubrir MAC de vecinos

4. Neighbor Advertisement (NA) - Tipo 136
   ├─ Respuesta a NS
   ├─ "Soy yo, mi MAC es 00:1A:2B:3C:4D:5E"
   └─ Usado también para notificar cambios

Ventajas sobre ARP (IPv4):
✓ Integrado en IP
✓ Soporte multicast nativo
✓ Menos broadcast
✓ Mejor escalabilidad
```

### 3.6 Configuración de Interfaces IPv6

#### Configuración Estática

```
Cisco IOS:

configure terminal
interface GigabitEthernet0/0
  ipv6 address 2001:DB8:ACAD:0001::1/64
  no shutdown
exit

Resultado:
├─ GUA: 2001:DB8:ACAD:0001::1/64
└─ LLA: Generada automáticamente (FE80::...)
```

#### Configuración Automática (SLAAC)

```
Stateless Address Auto Configuration:

Router anuncia: "Usa 2001:DB8:ACAD:0001::/64"
               "Default gateway soy yo: 2001:DB8:ACAD:0001::1"

Host automáticamente:
├─ Toma el prefijo: 2001:DB8:ACAD:0001:
├─ Genera ID de interfaz a partir de MAC
├─ Resultado: 2001:DB8:ACAD:0001::[ID generado]
└─ Configuración lista sin DHCP

Ventaja: Plug-and-play
```

#### DHCPv6

```
Stateful configuration:

1. Host envía Router Solicitation
2. Router responde con Router Advertisement
3. Host envía DHCP REQUEST
4. Servidor DHCP asigna dirección
5. Host configura IPv6

Usado cuando:
├─ Necesitas control centralizado
├─ Necesitas registros de asignación
└─ Ambiente corporativo
```

### 3.7 Campos del Encabezado IPv6

```
Estructura simplificada (40 bytes fijo):

┌────────────────────────────────────────┐
│ Vers │ Clase Tráfico │ Etiqueta Flujo │
├────────────────────────────────────────┤
│ Longitud de Carga (16 bits)            │
├────────────────────────────────────────┤
│ Próximo Encabezado (8) │ Límite Saltos │
├────────────────────────────────────────┤
│ Dirección IPv6 Origen (128 bits)       │
├────────────────────────────────────────┤
│ Dirección IPv6 Destino (128 bits)      │
└────────────────────────────────────────┘

Versión (4 bits): 6
Clase de Tráfico (8 bits): Similar a DSCP en IPv4
Etiqueta de Flujo (20 bits):
├─ Identifica flujos que requieren tratamiento especial
├─ QoS
└─ 0 = sin tratamiento especial

Longitud de Carga: Tamaño de datos + encabezados opcionales
Próximo Encabezado:
├─ Tipo de contenido
├─ 6 = TCP
├─ 17 = UDP
├─ 58 = ICMPv6
└─ 59 = Sin datos

Límite de Saltos: TTL de IPv6
├─ Comienza en 64, 255, o valor configurado
├─ Se decrementa por cada router
└─ Si llega a 0: se descarta

Ventajas sobre IPv4:
✓ Tamaño fijo 40 bytes (IPv4 20+ opciones)
✓ Sin fragmentación en routers (debe ser en origen)
✓ Sin checksum de encabezado (confía en enlace inferior)
```

### 3.8 Técnicas de Migración IPv4 → IPv6

#### Dual Stack

```
Fase de transición: Ambos protocolos en el mismo dispositivo

┌──────────────────────┐
│ Dispositivo          │
├──────────────────────┤
│ IPv4: 192.168.1.100  │
│ IPv6: 2001:DB8::100  │
│ MAC: 00:1A:2B:3C:... │
└──────────────────────┘

Ventajas:
├─ Máxima compatibilidad
├─ Transición gradual
└─ Sin interrupciones

Desventajas:
├─ Mayor complejidad
├─ Doble administración
└─ Consumo de recursos
```

#### Tunneling

```
Sobreencapsulación: Envía IPv6 dentro de IPv4

Escenario: Red IPv4 antigua entre dos sitios IPv6

Sitio A (IPv6) --- Tunel IPv4 --- Sitio B (IPv6)

Paquete IPv6 original:
┌──────────────────────┐
│ [IPv6 Header]        │
│ [Datos]              │
└──────────────────────┘

Se encapsula en IPv4:
┌──────────────────────────┐
│ [IPv4 Header nuevo]      │
│ [IPv6 Header original]   │
│ [Datos]                  │
└──────────────────────────┘

En destino:
└─ Se quita encabezado IPv4
└─ Se procesa IPv6

Tipos de tunel:
├─ 6to4: Automático basado en IPv4
├─ Teredo: Para atravesar NAT
└─ Tunnel Broker: Configuración manual
```

#### NAT64

```
Traducción bidireccional entre IPv6 e IPv4

IPv6 Red     NAT64     IPv4 Internet
  Host    ←→ Translator ←→ Servidor

Funciona así:
1. Cliente IPv6 quiere acceder a servidor IPv4
2. NAT64 traduce dirección y encabezados
3. Servidor IPv4 responde en IPv4
4. NAT64 traduce respuesta de vuelta a IPv6

Problema: Requiere configuración
Uso: Convivencia durante transición
```

---

## 4. Tabla de Enrutamiento

### 4.1 Estructura

```
Tabla en Router (show ip route):

C    10.0.0.0/24 is directly connected, GigabitEthernet0/1
O    10.1.0.0/24 [110/50] via 10.0.0.2, 00:05:23, GigabitEthernet0/0
S*   0.0.0.0/0 [1/0] via 10.0.0.1

Componentes:

Letra (Protocolo):
├─ C = Connected (directamente conectada)
├─ S = Static (ruta estática manual)
├─ O = OSPF (protocolo de enrutamiento)
├─ B = BGP (protocolo de enrutamiento)
└─ R = RIP (protocolo de enrutamiento antiguo)

Red Destino: 10.0.0.0/24
Distancia Administrativa: 110 (prioridad del protocolo)
Métrica: 50 (distancia calculada)
Próximo Salto: 10.0.0.2 (dónde enviar el paquete)
Interfaz: GigabitEthernet0/0 (por dónde salir)
Tiempo: 00:05:23 (cuánto conoce la ruta)
```

### 4.2 Ruta Estática

```
Comando:

ip route [RED DESTINO] [MÁSCARA] [PRÓXIMO SALTO / INTERFAZ]

Ejemplos:

ip route 192.168.1.0 255.255.255.0 192.168.0.1
├─ Destino: 192.168.1.0/24
├─ Máscara: 255.255.255.0
└─ Próximo salto: 192.168.0.1

ip route 10.1.0.0 255.255.0.0 GigabitEthernet0/0
├─ Destino: 10.1.0.0/16
├─ Por interfaz: GigabitEthernet0/0
└─ Punto a punto

Ruta por Defecto:

ip route 0.0.0.0 0.0.0.0 192.168.0.1
├─ Destino: Cualquier dirección (0.0.0.0/0)
├─ Máscara: 0 bits específicos
└─ "Si no sé dónde va, envía a 192.168.0.1"
```

---

## 5. ICMP - Internet Control Message Protocol

### 5.1 Funciones

```
ICMP proporciona diagnóstico y reporte de errores:

┌───────────────────────────────────┐
│ Echo Request (Ping)   │ Tipo 8    │
├───────────────────────────────────┤
│ Echo Reply (Ping)     │ Tipo 0    │
│ "Estoy vivo y te escucho"         │
├───────────────────────────────────┤
│ Destination Unreachable │ Tipo 3  │
│ "No puedo alcanzar esa dirección" │
├───────────────────────────────────┤
│ Time Exceeded │ Tipo 11           │
│ "TTL llegó a 0"                   │
├───────────────────────────────────┤
│ Redirect │ Tipo 5                 │
│ "Usa otra ruta, es más corta"     │
└───────────────────────────────────┘
```

### 5.2 Ping

```
Diagrama:

Host A                     Host B
  │                          │
  ├─ Echo Request (ICMP 8)  ─→
  │                          │
  │                   Procesado
  │                          │
  │ ← Echo Reply (ICMP 0) ──┤
  │                          │
  └─ Resultado: Host B vivo ✓

Información del Ping:
├─ Round Trip Time (RTT): ms de ida y vuelta
├─ TTL al recibir: indica cuántos saltos
├─ Pérdida: % de paquetes que no retornaron
└─ Bandwidthth: uso estimado de ancho de banda
```

### 5.3 Traceroute

```
Descubre ruta hacia destino:

Proceso:
1. Envía UDP con TTL=1
2. Primer router responde con ICMP Time Exceeded
3. Envía UDP con TTL=2
4. Segundo router responde
5. Continúa hasta alcanzar destino

Resultado:

traceroute 8.8.8.8

traceroute to 8.8.8.8 (8.8.8.8), 30 hops max

 1  192.168.1.1      1.234 ms     1.456 ms     1.389 ms
 2  10.5.5.1         5.123 ms     5.456 ms     5.234 ms
 3  201.45.1.1      12.456 ms    12.567 ms    12.678 ms
 4  8.8.8.8         15.123 ms    15.234 ms    15.345 ms

Cada línea = un salto (router)
Tres valores = tres intentos
Si aparece * = router no responde (silencioso)
```

---

## 6. Resumen - Capítulos 8-9

### Conceptos Clave IPv4
1. **Direccionamiento**: Identifica dispositivos en la red
2. **Clases históricas**: A, B, C (hoy obsoletas)
3. **CIDR**: Notación moderna /prefix
4. **Subneteo**: Dividir redes en subredes
5. **Routing**: Enviar paquetes al destino correcto

### Conceptos Clave IPv6
1. **Mayor tamaño**: 128 bits (vs 32 bits IPv4)
2. **Estructura jerárquica**: GRE/Subnet/Interface
3. **Tipos**: GUA (pública), ULA (privada), LLA (local), Multicast
4. **NDP**: Reemplaza ARP
5. **SLAAC**: Configuración automática

### Decisiones de Diseño
```
¿IPv4 o IPv6?
├─ Hoy: Probablemente ambos (Dual Stack)
├─ IPv4: Más común aún
├─ IPv6: Futuro a largo plazo
└─ Recomendación: Planificar IPv6 ahora

¿Qué máscara de subred?
├─ /24: 254 hosts (estándar PYMES)
├─ /25-/28: Redes medianas
├─ /30: Solo 2 hosts (enlaces router-router)
└─ VLSM: Ajustar a necesidades reales
```

---

## Referencias

- RFC 791 (IPv4)
- RFC 8200 (IPv6)
- RFC 1918 (Direcciones Privadas)
- RFC 4632 (CIDR)
- RFC 4291 (Formato IPv6)
- RFC 3315 (DHCPv6)
