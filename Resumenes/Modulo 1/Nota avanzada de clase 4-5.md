# Nota Avanzada - Capítulos 4-5: Ethernet Switching y Control de Acceso al Medio

## Introducción

Este documento expande los conceptos de las clases 4 y 5 sobre conmutación Ethernet y control de acceso al medio. Exploraremos en profundidad cómo funcionan las redes LAN modernas, desde las direcciones MAC hasta los mecanismos para evitar colisiones.

---

## 1. Fundamentos de Dirección MAC

### 1.1 Estructura de la Dirección MAC

Una dirección MAC (Media Access Control) tiene:
- **Longitud**: 48 bits / 12 dígitos hexadecimales / 6 octetos (bytes)
- **Formato**: XX-XX-XX-XX-XX-XX

**Ejemplo**: 00:1A:2B:3C:4D:5E

### 1.2 Componentes de la MAC

```
├─ Primeros 24 bits (3 octetos) = OUI (Organizationally Unique Identifier)
│  └─ Identifica el fabricante (Cisco, Dell, Apple, etc.)
│     └─ Ejemplo: 00-1A-2B = Cisco
│
└─ Últimos 24 bits (3 octetos) = Código único
   └─ Identificador del dispositivo específico dentro de ese fabricante
      └─ Ejemplo: 3C-4D-5E = Serial único de ese switch/interfaz
```

### 1.3 Formatos de Representación

La misma MAC se escribe de diferentes formas:

```
Separado por guiones:        00-1A-2B-3C-4D-5E    (Windows)
Separado por dos puntos:     00:1A:2B:3C:4D:5E    (Linux/Mac)
Puntos (IOS de Cisco):       001A.2B3C.4D5E       (Routers/Switches)
```

### 1.4 Direcciones MAC Especiales

| Dirección MAC | Significado | Uso |
|---|---|---|
| FF:FF:FF:FF:FF:FF | Broadcast | Se envía a TODOS en la red |
| 01:00:5E:XX:XX:XX | Multicast | Se envía a grupo selectivo |
| 00:00:00:00:00:00 | Reservada | No se usa |
| **Primeros 3 bytes iguales** | Mismo fabricante | Identificar dispositivos |

### 1.5 MAC vs IP - Diferencias Fundamentales

| Característica | MAC | IP |
|---|---|---|
| **Capa OSI** | 2 (Enlace de Datos) | 3 (Red) |
| **Alcance** | Local (LAN) | Global (Internet) |
| **Cambio** | NO cambia | Sí, según ubicación |
| **Analogía** | Nombre de persona | Dirección de persona |
| **Asignación** | Fabricante (quemada) | Administrador |
| **Longitud** | 48 bits | 32 bits (IPv4) |
| **Formato** | Hexadecimal | Decimal punteado |

**Analogía práctica**:
- MAC es como tu **nombre completo** (siempre igual)
- IP es como tu **dirección** (cambia si te mudas)

---

## 2. ARP (Address Resolution Protocol)

### 2.1 ¿Por qué existe ARP?

Los dispositivos en una LAN conocen:
- Las IPs (direcciones lógicas) ✓
- Pero necesitan MACs (direcciones físicas) para enviar tramas ✗

**ARP resuelve este problema**: IP → MAC

### 2.2 Proceso ARP (Mapeo IP-MAC)

```
Escenario: PC1 necesita comunicarse con PC2 en la misma red

IP PC1: 192.168.1.10  MAC: AA:BB:CC:11:22:33
IP PC2: 192.168.1.20  MAC: ?? (desconocida)

Paso 1: PC1 envía ARP REQUEST (BROADCAST)
┌─────────────────────────────────────────────┐
│ "¿Quién tiene la IP 192.168.1.20?"          │
│ Remitente: 192.168.1.10 / AA:BB:CC:11:22:33 │
│ Destino: 255.255.255.255 (BROADCAST)        │
└─────────────────────────────────────────────┘
           ↓ (Todos lo reciben)

Paso 2: PC2 reconoce su IP, envía ARP REPLY (UNICAST)
┌──────────────────────────────────────────────┐
│ "Soy 192.168.1.20, mi MAC es DD:EE:FF:44:55:66" │
│ Envío de vuelta a: AA:BB:CC:11:22:33        │
└──────────────────────────────────────────────┘

Paso 3: PC1 actualiza su tabla ARP
Tabla ARP en PC1:
┌──────────────────┬─────────────────────┐
│ IP               │ MAC                 │
├──────────────────┼─────────────────────┤
│ 192.168.1.20     │ DD:EE:FF:44:55:66   │
└──────────────────┴─────────────────────┘

Paso 4: Ahora PC1 puede enviar datos a PC2
```

### 2.3 Tabla ARP en Dispositivos

Los switches, routers y PCs mantienen **tablas ARP** (caché):

```
# Ver tabla ARP en Windows:
arp -a

# Ver en Linux/Mac:
arp -a
o
ip neigh show

# Ver en Cisco IOS:
show arp
```

**Entrada típica**:
```
Internet Address    Hardware Address    Type
192.168.1.20        dd:ee:ff:44:55:66   dynamic
```

---

## 3. Ethernet Switching en Profundidad

### 3.1 ¿Qué es un Switch?

Un switch es un dispositivo de Capa 2 que:
- Recibe tramas Ethernet en puertos
- Aprende las MACs de los dispositivos conectados
- Reenvía tramas **solo al puerto necesario** (no broadcast)
- Crea conexiones punto a punto entre dispositivos

### 3.2 Tabla de Direcciones MAC del Switch

El switch mantiene una **tabla de conmutación** (MAC table):

```
Situación inicial (sin conocimiento):
┌──────────┐
│ Switch 1 │
├──────────┤
│ Puerto 1 │ ← PC A (MAC: AA:AA:AA:AA:AA:AA)
│ Puerto 2 │ ← PC B (MAC: BB:BB:BB:BB:BB:BB)
│ Puerto 3 │ ← PC C (MAC: CC:CC:CC:CC:CC:CC)
└──────────┘

Tabla MAC vacía:
┌──────┬────────┐
│ Port │ MAC    │
├──────┼────────┤
│ -    │ -      │
└──────┴────────┘

PC A envía una trama a PC B:
1. Switch recibe en Puerto 1 → Aprende que Puerto 1 tiene MAC AA:AA:AA:AA:AA:AA
2. Busca destino BB:BB:BB:BB:BB:BB en tabla → No conoce
3. Reenvía por TODOS los puertos excepto el que recibió (flooding)

Tabla MAC ahora:
┌──────┬──────────────────────┐
│ Port │ MAC                  │
├──────┼──────────────────────┤
│ 1    │ AA:AA:AA:AA:AA:AA    │
└──────┴──────────────────────┘

PC B recibe y responde:
1. Switch recibe en Puerto 2 → Aprende que Puerto 2 tiene MAC BB:BB:BB:BB:BB:BB
2. Busca destino AA:AA:AA:AA:AA:AA en tabla → ¡ENCONTRADO! Puerto 1
3. Reenvía SOLO por Puerto 1

Tabla MAC ahora:
┌──────┬──────────────────────┐
│ Port │ MAC                  │
├──────┼──────────────────────┤
│ 1    │ AA:AA:AA:AA:AA:AA    │
│ 2    │ BB:BB:BB:BB:BB:BB    │
└──────┴──────────────────────┘

Comunicaciones futuras entre A-B son directas (sin flooding)
```

### 3.3 Envejecimiento de Tabla MAC

Las entradas en tabla MAC tienen **TTL (Time To Live)**:
- Por defecto: 300 segundos (5 minutos)
- Si el dispositivo no envía nada en ese tiempo → entrada se elimina
- Razón: Dispositivos pueden cambiar de puerto físico

---

## 4. Conmutación vs Enrutamiento

### 4.1 Diferencias

| Aspecto | Conmutación (Switch) | Enrutamiento (Router) |
|---|---|---|
| **Capa OSI** | Capa 2 (Enlace) | Capa 3 (Red) |
| **Dirección** | Usa MAC | Usa IP |
| **Alcance** | LAN local | WAN global |
| **Decisión** | Tabla MAC | Tabla de rutas |
| **PDU** | Trama | Paquete |
| **Velocidad** | Más rápido | Más lento |

### 4.2 Ejemplo Práctico

```
Red empresarial con 2 switches conectados a 1 router:

        ┌─────────────┐
        │   Router    │
        │   (Capa 3)  │
        └─────────────┘
              │
     ┌────────┴────────┐
     │                 │
  ┌──────────┐    ┌──────────┐
  │ Switch 1 │    │ Switch 2 │
  │ (Capa 2) │    │ (Capa 2) │
  └──────────┘    └──────────┘
   │  │  │        │  │  │
  PC1 PC2 PC3    PC4 PC5 PC6

Comunicación dentro del mismo switch (PC1 → PC2):
- Usa conmutación (Capa 2)
- Usa tablas MAC
- No pasa por router
- Rápido

Comunicación entre switches (PC1 → PC4):
- Pasa por router (necesita Capa 3)
- Router toma decisión basada en IPs
- Más lento, pero puede ir por WAN
```

---

## 5. Control de Acceso al Medio (MAC)

### 5.1 El Problema: Colisiones en Medios Compartidos

En una red con **múltiples dispositivos compartiendo el mismo medio**:

```
┌──────┐  ┌──────┐  ┌──────┐
│ PC A │  │ PC B │  │ PC C │
└──────┘  └──────┘  └──────┘
    \        │        /
     \       │       /
      └──────────────┘  ← Medio compartido (cable o aire)

Problema:
PC A y PC B intentan transmitir al mismo tiempo
                ↓
         COLISIÓN ❌
                ↓
         Datos se dañan
                ↓
         Ambos deben retransmitir
```

### 5.2 CSMA/CD (Carrier Sense Multiple Access with Collision Detection)

Usado por **Ethernet cableado (802.3)**:

```
┌────────────────────────────────────┐
│ CSMA/CD - Algoritmo de Transmisión │
└────────────────────────────────────┘

Paso 1: CARRIER SENSE (Escuchar primero)
        ¿Hay alguien usando el medio?
        SI  → Esperar
        NO  → Proceder

Paso 2: MULTIPLE ACCESS (Múltiples pueden escuchar)
        Si está libre, ENVIAR datos

Paso 3: COLLISION DETECTION (Detectar colisiones)
        Mientras se envía, ¿se detecta colisión?
        SI  → Enviar jamming signal
            → Todos detectan y se retractan
            → Esperar tiempo aleatorio
            → Volver a intentar (Backoff exponencial)
        NO  → Transmisión exitosa
```

**Problema**: En redes modernas, CSMA/CD es **innecesario** porque:
- Los switches crean conexiones punto-a-punto (no compartidas)
- No hay colisiones en switchs modernos
- Cada puerto es una conexión dedicada

### 5.3 CSMA/CA (Collision Avoidance)

Usado por **WiFi (802.11)**:

```
┌────────────────────────────────────┐
│ CSMA/CA - Prevención (no detección)│
└────────────────────────────────────┘

Paso 1: ESCUCHAR medio
        ¿Hay señal de datos?
        SI  → Esperar + tiempo aleatorio
        NO  → Proceder

Paso 2: ENVIAR RTS (Request To Send)
        "Voy a enviar, por favor no interfieran"

Paso 3: RECIBIR CTS (Clear To Send)
        "Adelante, la frecuencia está libre"

Paso 4: ENVIAR DATOS

Paso 5: RECIBIR ACK (Acknowledgment)
        "Datos recibidos correctamente"

Ventaja: Previene colisiones ANTES de ocurrir
Desventaja: Más overhead (tráfico de control)
```

### 5.4 Full Duplex vs Half Duplex

| Modo | Descripción | Uso |
|---|---|---|
| **Full Duplex** | Envía Y recibe simultáneamente | Switches, routers actuales |
| **Half Duplex** | Envía O recibe (alternado) | WiFi, repeaters antiguos |

**Implicación**: 
- Full Duplex elimina colisiones (conexión dedicada)
- Half Duplex requiere CSMA/CD o CSMA/CA

---

## 6. Estructura de Trama Ethernet

### 6.1 Campos de la Trama Ethernet (802.3)

```
┌──────────┬─────────────┬──────────┬──────────┬───────────┬─────────────┬───────┐
│Preámbulo │ Delimitador │MAC Dest  │MAC Origen│Tipo/Longitud│Datos (Payload)│CRC   │
│ 7 bytes  │  1 byte     │ 6 bytes  │ 6 bytes  │  2 bytes   │46-1500 bytes│4bytes│
└──────────┴─────────────┴──────────┴──────────┴───────────┴─────────────┴───────┘
```

### 6.2 Función de cada campo

**Preámbulo (7 bytes)**
- Patrón 10101010... alterno
- Permite que receptores se sincronicen
- No es contado como parte de la trama

**Delimitador de inicio de trama (1 byte)**
- Patrón especial: 10101011
- Marca el **inicio oficial** de la trama
- Parte del preámbulo

**MAC Destino (6 bytes)**
- Dirección MAC del receptor
- FF:FF:FF:FF:FF:FF = broadcast

**MAC Origen (6 bytes)**
- Dirección MAC del transmisor
- Siempre es unicast (un dispositivo)

**Tipo/Longitud (2 bytes)**
- Si valor > 1500: Tipo de protocolo
  - 0x0800 = IPv4
  - 0x0806 = ARP
  - 0x86DD = IPv6
- Si valor < 1500: Longitud de datos

**Datos (Payload) (46-1500 bytes)**
- Los datos reales que se transmiten
- Mínimo 46 bytes (relleno si es necesario)
- Máximo 1500 bytes (MTU)

**CRC (4 bytes)**
- Cyclic Redundancy Check
- Detección de errores
- Recalculado en cada salto
- Si no coincide → trama descartada

### 6.3 Tamaños de Trama Ethernet

```
Mínimo: 64 bytes
├─ 14 bytes (MAC dest + MAC origen + Tipo)
├─ 46 bytes (datos mínimos)
├─ 4 bytes (CRC)
└─ Total: 64 bytes

Máximo (estándar): 1518 bytes
├─ 14 bytes (encabezado)
├─ 1500 bytes (datos - MTU)
├─ 4 bytes (CRC)
└─ Total: 1518 bytes

Excepciones:
- VLAN tagged: 1522 bytes
- Jumbo frames: hasta 9216 bytes
```

### 6.4 Tramas especiales (errores)

| Nombre | Tamaño | Causa |
|---|---|---|
| **Runt Frame** | < 64 bytes | Colisión detectada |
| **Giant Frame** | > 1518 bytes | Configuración incorrecta |
| **CRC Error** | Cualquiera | Datos dañados |

---

## 7. Funciones de la Capa MAC

### 7.1 Delimitación de Tramas
- Define dónde empieza y termina cada trama
- Preámbulo: inicio
- CRC: final
- Sincroniza entre transmisor y receptor

### 7.2 Direccionamiento
- Cada encabezado Ethernet tiene MAC destino y origen
- Permite entrega local en la LAN
- Switch usa esta información para conmutación

### 7.3 Detección de Errores
- **CRC (Cyclic Redundancy Check)**
- Función matemática sobre todos los bits de la trama
- Receptor recalcula y compara
- Si no coincide → descarta trama
- Nota: No corrige errores, solo detecta

---

## 8. Conmutación a Nivel 3 (CEF)

### 8.1 ¿Por qué switches hacen enrutamiento?

Switches Layer 3 pueden:
- Ejecutar protocolos de enrutamiento
- Tomar decisiones basadas en IPs
- Usar **tablas de enrutamiento**

### 8.2 CEF (Cisco Express Forwarding)

Mecanismo de forwarding rápido en Cisco:

**Componentes**:
1. **FIB (Forward Information Base)**
   - Tabla de enrutamiento simplificada
   - Optimizada para búsqueda rápida

2. **Tabla de Adyacencia**
   - Lista de próximos saltos conocidos
   - MAC del siguiente salto pre-calculada
   - Información de interfaz de salida

**Ventaja**: Forwarding ultra-rápido sin búsquedas complejas

---

## 9. VLAN (Virtual LAN) - 802.1Q

### 9.1 ¿Qué es una VLAN?

Una red lógica dentro de un switch físico:

```
┌─────────────────────────────┐
│        Switch Físico        │
├─────────────────────────────┤
│  ┌────────────────────────┐ │
│  │ VLAN 10 (Contabilidad) │ │
│  │ Puerto 1-5             │ │
│  └────────────────────────┘ │
│  ┌────────────────────────┐ │
│  │ VLAN 20 (Ventas)       │ │
│  │ Puerto 6-10            │ │
│  └────────────────────────┘ │
│  ┌────────────────────────┐ │
│  │ VLAN 30 (IT)           │ │
│  │ Puerto 11-15           │ │
│  └────────────────────────┘ │
└─────────────────────────────┘
```

### 9.2 Beneficios
- Seguridad: VLANs no se comunican entre sí (sin router)
- Flexibilidad: Agregar dispositivo solo con cambio de software
- Administración: Grupos lógicos
- Eficiencia: Limita broadcast

---

## 10. Resumen y Conceptos Clave

### Puntos Fundamentales
1. **MAC** identifica dispositivos en LAN local
2. **ARP** traduce IPs a MACs
3. **Switches** aprenden MACs y reenvían eficientemente
4. **Conmutación** es Capa 2, **Enrutamiento** es Capa 3
5. **CSMA/CD** (Ethernet) y **CSMA/CA** (WiFi) evitan colisiones
6. **Full Duplex** en switches modernos elimina colisiones
7. **Tramas Ethernet** tienen estructura definida con CRC
8. **CEF** optimiza forwarding en routers modernos

### Analogía Final

Imagina el correo postal:
- **MAC** = Tu nombre (nunca cambia)
- **IP** = Tu dirección (cambia si te mudas)
- **Switch** = Cartero local (entrega dentro del pueblo)
- **Router** = Sistema postal (entrega entre ciudades)

---

## Referencias

- IEEE 802.3 (Ethernet Standard)
- IEEE 802.1Q (VLAN Standard)
- RFC 826 (ARP)
- Cisco IOS Command Reference
