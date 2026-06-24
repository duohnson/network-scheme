# Nota Avanzada - Capítulos 1-3: Fundamentos de Protocolos y Modelos de Red

## Introducción

Los protocolos de red son el corazón de la comunicación moderna. Sin ellos, los dispositivos no podrían comunicarse. Este documento amplia los conceptos básicos de las notas de clase 1-3 con explicaciones más profundas y ejemplos prácticos.

## 1. Elementos Básicos de la Comunicación

Todo método de comunicación tiene tres elementos fundamentales:

### 1.1 Origen del Mensaje
- Entidad que inicia la comunicación
- Puede ser humana o electrónica
- Genera la información a transmitir
- Responsable de codificar el mensaje según el protocolo

**Ejemplo**: Un cliente HTTP iniciando una solicitud a un servidor web.

### 1.2 Destino del Mensaje
- Entidad receptora
- Recibe, decodifica e interpreta el mensaje
- Procesa la información
- Puede generar una respuesta

**Ejemplo**: El servidor web que recibe la solicitud HTTP y envía la página.

### 1.3 Canal de Comunicación
- Camino físico o lógico del mensaje
- Formado por los medios que transportan la información
- Puede ser cobre, fibra óptica o inalámbrico

**Ejemplo**: Internet (múltiples medios conectados).

---

## 2. Requisitos de un Protocolo de Red

### 2.1 Codificación de Mensajes

**Definición**: Conversión de información en una forma aceptable para transmisión.

- **Codificación**: Conversión de datos a formato transmisible
  - Datos en texto → bits (0s y 1s)
  - Código ASCII para caracteres
  - Señales eléctricas en cables de cobre
  - Pulsos de luz en fibra óptica
  - Ondas electromagnéticas en WiFi

- **Decodificación**: Interpretación de datos recibidos
  - Bits recibidos → información legible
  - Reverso del proceso de codificación

**Ejemplo práctico**:
```
Texto: "HOLA"
Codificación ASCII: 01001000 01001111 01001100 01000001
Transmisión por cable
Decodificación: "HOLA"
```

### 2.2 Formato y Encapsulamiento del Mensaje

- Cada protocolo tiene un formato específico
- Los datos se encapsulan (se envuelven) con encabezados
- Cada capa agrega su propio encabezado

**Estructura de una trama Ethernet**:
```
┌─────────┬──────┬──────┬────────┬──────────┬──────┐
│Preámbulo│Start │  MAC │  MAC   │ Tipo/Len │Datos │CRC  │
│         │Frame │Dest  │Origen  │          │      │     │
└─────────┴──────┴──────┴────────┴──────────┴──────┘
```

### 2.3 Tamaño del Mensaje

- **Problema**: Mensajes muy grandes pueden causar errores
- **Solución**: Segmentación en fragmentos pequeños
- **MTU (Maximum Transmission Unit)**: Tamaño máximo típico = 1500 bytes en Ethernet
- **Reassembly**: Reensamble en el destino

### 2.4 Sincronización del Mensaje

La temporización es crítica. Incluye:

#### Control de Flujo
- Gestión de velocidad de transmisión
- Evita saturar el destino
- TCP utiliza ventanas deslizantes (sliding windows)

#### Tiempo de Espera (Timeout)
- Define cuánto esperar respuesta
- Si no llega: retransmitir o descartar
- Previene esperas infinitas

#### Método de Acceso
- Determina **cuándo** puede enviar cada dispositivo
- Evita colisiones
- Ejemplos: CSMA/CD (Ethernet), CSMA/CA (WiFi)

---

## 3. Tipos de Entrega de Mensajes

### 3.1 Unicast (Uno a Uno)
- Un origen envía a **un único destino**
- Ejemplo: Email, SSH, HTTP
- Más directo y privado

### 3.2 Multicast (Uno a Varios)
- Un origen envía a **grupo selectivo de destinos**
- Rango IPv4: 224.0.0.0 a 239.255.255.255
- Ejemplo: Videoconferencia en grupo, IPTV
- Los destinos deben estar suscritos

### 3.3 Broadcast (Uno a Todos)
- Un origen envía a **todos en la red**
- Dirección broadcast IPv4: x.x.x.255
- MAC broadcast: FF:FF:FF:FF:FF:FF
- Ejemplo: ARP, DHCP DISCOVER
- Consumo de ancho de banda significativo

---

## 4. Protocolos de Red - Clasificación

### 4.1 Protocolos de Comunicación de Red
Permiten que dispositivos se comuniquen a través de medios compatibles.

**Familia Ethernet**:
- Capa Física/Enlace: Ethernet
- Capa de Red: IP (IPv4, IPv6)
- Capa de Transporte: TCP, UDP
- Capa de Aplicación: HTTP, HTTPS, FTP

### 4.2 Protocolos de Seguridad
Protegen datos mediante:
- **Autenticación**: Verificar identidad
- **Integridad**: Asegurar que datos no fueron modificados
- **Cifrado**: Volver datos ilegibles sin clave

**Ejemplos**: SSH, SSL/TLS, IPSec

### 4.3 Protocolos de Enrutamiento
Permiten a routers intercambiar información de rutas.

**Ejemplos**:
- **OSPF** (Open Shortest Path First)
- **BGP** (Border Gateway Protocol)
- **EIGRP** (Enhanced Interior Gateway Routing Protocol)

### 4.4 Protocolos de Detección de Servicios
Automatiza búsqueda de dispositivos y servicios.

**Ejemplos**:
- **DHCP**: Asigna IPs automáticamente
- **DNS**: Resuelve nombres de dominio

---

## 5. Funciones de Protocolos de Red

### 5.1 Direccionamiento
- Identifica remitente y destinatario
- IPv4: 192.168.1.1
- IPv6: 2001:0DB8::1
- MAC: 00:1A:2B:3C:4D:5E

### 5.2 Confiabilidad
**TCP (Confiable)**:
- Retransmite datos perdidos
- Garantiza entrega
- Mayor sobrecarga

**UDP (No confiable)**:
- Sin retransmisión
- Mejor velocidad
- Usado en: streaming, videojuegos, VoIP

### 5.3 Control de Flujo
- Asegura velocidad eficiente
- Evita desbordamientos
- TCP: Ventana deslizante

### 5.4 Secuenciación
- Numeración única de segmentos
- Permite reensamble correcto
- Detecta pérdida o duplicación
- TCP proporciona números de secuencia (SEQ)

### 5.5 Detección de Errores
- Determina si datos se dañaron
- **CRC (Cyclic Redundancy Check)**: Ethernet, TCP, UDP
- **Checksum**: IPv4, IPv6

### 5.6 Interfaz de Aplicación
- Comunicación proceso a proceso
- Puertos identifican aplicaciones
- HTTP (puerto 80), HTTPS (443), SSH (22)

---

## 6. Modelo OSI - Visión Detallada

| Capa | Nombre | PDU | Función | Ejemplos |
|------|--------|-----|---------|----------|
| 7 | Aplicación | Datos | Interfaz usuario | HTTP, FTP, SMTP |
| 6 | Presentación | Datos | Formato, cifrado, compresión | JPEG, MPEG |
| 5 | Sesión | Datos | Gestión de diálogos | RPC, SQL |
| 4 | Transporte | Segmento/Datagrama | Entrega confiable | TCP, UDP |
| 3 | Red | Paquete | Enrutamiento | IP, ICMP |
| 2 | Enlace de Datos | Trama | Acceso a medios | Ethernet, PPP |
| 1 | Física | Bits | Transmisión física | Cables, fibra |

---

## 7. Encapsulación - El Proceso Completo

Cuando se envía un mensaje HTTP:

```
Capa 7 (App):  [Página HTML]

Capa 6 (Pres): [Página HTML] → Puede comprimirse

Capa 5 (Ses):  [Página HTML] → Se establece sesión

Capa 4 (Trans): [TCP Header]|[Página HTML] → Segmento
                Puertos (80), flags SYN/ACK

Capa 3 (Red):   [IP Header]|[TCP Header]|[Página HTML] → Paquete
                Dirección origen/destino

Capa 2 (Enl):   [MAC Header]|[IP Header]|[TCP Header]|[Página HTML]|[CRC] → Trama
                Dirección MAC origen/destino

Capa 1 (Fís):   01010101010101010101... → Bits
                Codificados en el medio
```

### Desencapsulación (Lado Receptor)

El proceso inverso:
```
Bits recibidos → Trama → Paquete → Segmento → Datos → Aplicación
```

---

## 8. Modelo TCP/IP vs OSI

### Comparación

| OSI (7 capas) | TCP/IP (4 capas) | Descripción |
|---|---|---|
| Aplicación | Aplicación | Servicios al usuario |
| Presentación | Aplicación | Formato de datos |
| Sesión | Aplicación | Gestión de conexiones |
| Transporte | Transporte | TCP/UDP |
| Red | Internet | IP, routing |
| Enlace de Datos | Acceso a red | Ethernet, WiFi |
| Física | Acceso a red | Medios físicos |

### Ventajas de cada modelo

**OSI**:
- Más detallado
- Mejor para enseñanza
- Referencias en documentación

**TCP/IP**:
- Modelo práctico
- Realmente usado en Internet
- Más simple

---

## 9. Convergencia de Redes

Las redes modernas transportan simultáneamente:
- **Datos**: FTP, Email, Web
- **Voz**: VoIP, Telefonía IP
- **Video**: Streaming, Videoconferencias
- **Multimedia**: Aplicaciones integradas

### Beneficios
- Reducción de cableado
- Mejor utilización de ancho de banda
- Costos menores
- QoS para priorización

---

## 10. Resumen y Conclusiones

### Puntos Clave
1. Todo protocolo debe tener codificación, formato, sincronización
2. Existen tres tipos de entrega: unicast, multicast, broadcast
3. Los protocolos se clasifican por función (seguridad, enrutamiento, etc.)
4. El modelo OSI ayuda a entender las capas de red
5. La encapsulación es el proceso fundamental de comunicación
6. TCP/IP es el modelo práctico usado en Internet

### Para Recordar
- **Codificación**: Datos → bits
- **Encapsulación**: Agregar encabezados en cada capa
- **Sincronización**: Control de flujo, timeouts, acceso al medio
- **Confiabilidad**: TCP es confiable, UDP es rápido

---

## Referencias y Lecturas Recomendadas

- RFC 793 (TCP)
- RFC 768 (UDP)
- ISO/IEC 7498 (Modelo OSI)
- RFC 791 (IPv4)
- RFC 8200 (IPv6)
