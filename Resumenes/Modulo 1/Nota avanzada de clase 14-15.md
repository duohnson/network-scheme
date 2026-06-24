# Nota Avanzada - Capítulos 14-15: Capa de Transporte y Capa de Aplicación

## Introducción

Las capas 4 y 7 del modelo OSI son donde ocurre la interacción real con las aplicaciones. Este documento profundiza en TCP, UDP, y los principales protocolos de aplicación que hacen funcionar Internet.

---

## 1. Capa de Transporte (OSI Capa 4)

### 1.1 Funciones Principales

```
┌──────────────────────────────────┐
│ Capa de Transporte (Capa 4)      │
├──────────────────────────────────┤
│ Responsabilidades:               │
│ • Segmentación de datos          │
│ • Entrega confiable o rápida     │
│ • Control de flujo               │
│ • Multiplexación                 │
│                                  │
│ PDU: SEGMENTO (TCP) o DATAGRAMA (UDP)
│ Dispositivos: Computadoras       │
│ Puertos: Identifican aplicaciones│
└──────────────────────────────────┘
```

### 1.2 TCP vs UDP

```
┌──────────────────────┬──────────────────┬──────────────────┐
│   Característica     │       TCP        │       UDP        │
├──────────────────────┼──────────────────┼──────────────────┤
│ Orientación          │ Orientado conexión│ Sin conexión     │
│ Confiabilidad        │ Confiable        │ No confiable     │
│ Retransmisión        │ SÍ               │ NO               │
│ Orden de datos       │ Garantizado      │ NO garantizado   │
│ Control de flujo     │ SÍ (windowing)   │ NO               │
│ Encabezado           │ 20 bytes mínimo  │ 8 bytes          │
│ Velocidad            │ Lenta (overhead) │ Rápida           │
│ Uso                  │ Email, Web, SSH  │ Video, VoIP      │
│ RFC                  │ 793              │ 768              │
└──────────────────────┴──────────────────┴──────────────────┘
```

### 1.3 TCP - Transmission Control Protocol

#### Características TCP

```
✓ Orientado a Conexión
  └─ Establece conexión antes de datos

✓ Entrega Confiable
  └─ Retransmite datos perdidos

✓ Entrega Ordenada
  └─ Reensamblá segmentos en orden correcto

✓ Control de Flujo
  └─ Regula velocidad de transmisión

✓ Con Estado
  └─ Seguimiento de conexión activa

Desventajas:
✗ Mayor overhead (encabezados + ACKs)
✗ Más lento que UDP
✗ Mayor uso de CPU/memoria
```

#### Estructura del Segmento TCP

```
┌──────────────────────────────────────────────┐
│ Puerto Origen (16 bits)                      │
├──────────────────────────────────────────────┤
│ Puerto Destino (16 bits)                     │
├──────────────────────────────────────────────┤
│ Número de Secuencia (32 bits)                │
├──────────────────────────────────────────────┤
│ Número de Confirmación (32 bits)             │
├──────────────────────────────────────────────┤
│ Offset │ Reservado │ Flags │ Tamaño Ventana │
├──────────────────────────────────────────────┤
│ Checksum (16 bits)                           │
├──────────────────────────────────────────────┤
│ Puntero Urgente (16 bits)                    │
├──────────────────────────────────────────────┤
│ Opciones (variable)                          │
├──────────────────────────────────────────────┤
│ Datos                                        │
└──────────────────────────────────────────────┘

Puertos:
├─ Puerto Origen: ≥ 1024 (cliente)
├─ Puerto Destino: Bien conocido (≤ 1023)
└─ Combinación: socket único

Número de Secuencia:
├─ Identifica bytes específicos
├─ Comienza aleatorio
└─ Se incrementa con cada byte enviado

Número de Confirmación:
├─ Próximo byte esperado
├─ Confirma recepción
└─ Ejemplo: si recibí bytes 0-99, confirmo 100

Flags (Banderas):
├─ SYN: Inicio de conexión
├─ ACK: Confirmación de datos
├─ FIN: Fin de conexión
├─ RST: Reset (error)
├─ PSH: Datos urgentes
└─ URG: Puntero urgente válido

Tamaño de Ventana (Window Size):
├─ Cuántos bytes más puedo recibir
├─ Control de flujo dinámico
├─ Ejemplo: 65535 bytes
└─ Receptor ajusta según capacidad
```

#### Three-Way Handshake (Establecimiento de Conexión)

```
Cliente                      Servidor
   │                            │
   │    SYN (seq=100)           │
   │──────────────────────────→│
   │  "Quiero conectar, mi SEQ=100"
   │                            │
   │  SYN-ACK (seq=300, ack=101)│
   │←──────────────────────────│
   │  "OK, mi SEQ=300, confirmo tu 100+1"
   │                            │
   │   ACK (seq=101, ack=301)   │
   │──────────────────────────→│
   │  "Confirmo tu 300+1, iniciamos"
   │                            │
   ╔════ CONEXIÓN ESTABLECIDA ════╗
   │                              │
   │  [Envío de Datos]            │
   │  Data (seq=101, ack=301)     │
   │──────────────────────────→│
   │                    Procesa
   │   ACK (seq=301, ack=111)   │
   │←──────────────────────────│
   │  "Recibí tus 10 bytes"
   │
   ╚════ FIN DE DATOS ════╝
   │
   │   FIN (seq=111, ack=301)   │
   │──────────────────────────→│
   │  "Termino la conexión"
   │                            │
   │   FIN-ACK (seq=301, ack=112)│
   │←──────────────────────────│
   │  "OK, termino también"
   │                            │
   │   ACK (seq=112)            │
   │──────────────────────────→│
   │  "Confirmado"
   │                            │
   ╚════ CONEXIÓN CERRADA ════╝
```

#### Control de Flujo TCP (Ventanas Deslizantes)

```
Escenario:
- Emisor puede enviar rápido
- Receptor está ocupado, no puede procesar rápido

Solución: TCP envía "Ventana" (cuántos bytes puede recibir)

Antes de enviar datos:
Emisor recibe: "Mi ventana es 1000 bytes"
└─ Significa: "Puedes enviarme hasta 1000 bytes"

Envía 500 bytes (datos):
Emisor queda: "Puedo enviar 500 más"
└─ Ventana se reduce de 1000 a 500

Receptor procesa datos y actualiza ventana:
Emisor recibe nuevo ACK: "Mi ventana ahora es 800 bytes"
└─ Ventana se expande de 500 a 800

Resultado:
✓ Emisor nunca desborda receptor
✓ Ajuste dinámico según carga
✓ Optimización automática de velocidad
```

### 1.4 UDP - User Datagram Protocol

#### Características UDP

```
✗ Sin Conexión
  └─ Envía datos sin establecer conexión

✗ No Confiable
  └─ No retransmite perdidas

✗ Sin Orden Garantizado
  └─ Paquetes pueden llegar fuera de orden

✗ Sin Control de Flujo
  └─ Envía a la máxima velocidad

✗ Sin Estado
  └─ Cada paquete independiente

Ventajas:
✓ Encabezado pequeño (8 bytes)
✓ Muy rápido
✓ Bajo overhead
✓ Ideal para tiempo real
```

#### Estructura del Datagrama UDP

```
┌──────────────────────────────────────┐
│ Puerto Origen (16 bits)              │
├──────────────────────────────────────┤
│ Puerto Destino (16 bits)             │
├──────────────────────────────────────┤
│ Longitud (16 bits)                   │
├──────────────────────────────────────┤
│ Checksum (16 bits)                   │
├──────────────────────────────────────┤
│ Datos (variable)                     │
└──────────────────────────────────────┘

Total mínimo: 28 bytes (8 UDP + 20 IPv4)
Muy simple, sin conexión
```

#### Comparación Uso UDP vs TCP

```
UDP se usa en:
├─ VoIP (Skype, WhatsApp) → Velocidad más importante
├─ Videoconferencia → Pérdida ocasional aceptable
├─ Online Gaming → Latencia baja crítica
├─ Streaming de Video → Puede perder frames
├─ DHCP → Configuración rápida
├─ DNS → Consultas rápidas
└─ SNMP → Monitoreo simple

TCP se usa en:
├─ Email → Todos los datos deben llegar
├─ Web (HTTP/HTTPS) → Integridad crítica
├─ FTP → Transferencia exacta de archivos
├─ SSH/Telnet → Control remoto confiable
├─ Bases de datos → Transacciones
└─ Cualquier cosa donde perdida = problema
```

### 1.5 Puertos

#### Rango de Puertos

```
Puertos bien conocidos:    0 - 1023
├─ Reservados para servicios estándar
├─ Requieren permisos especiales (root/admin)
└─ Ejemplos: HTTP(80), HTTPS(443), SSH(22)

Puertos Registrados:       1024 - 49151
├─ Asignados por IANA para aplicaciones
├─ Disponibles para usuarios
└─ Ejemplos: 3306(MySQL), 5432(PostgreSQL)

Puertos Dinámicos/Efímeros: 49152 - 65535
├─ Asignados dinámicamente por SO
├─ Para conexiones cliente salientes
└─ Temporal, dura lo que la conexión
```

#### Puertos Comunes

```
PROTOCOLOS DE APLICACIÓN:

HTTP:           80/TCP      Navegación web
HTTPS:          443/TCP     Web segura (TLS/SSL)
SSH:            22/TCP      Control remoto seguro
Telnet:         23/TCP      Control remoto (inseguro)
SMTP:           25/TCP      Envío de email
IMAP:           143/TCP     Lectura de email (servidor)
POP3:           110/TCP     Descarga de email (servidor)

INFRAESTRUCTURA:

DNS:            53/TCP+UDP  Resolución de nombres
DHCP:           67,68/UDP   Configuración IP
FTP:            20,21/TCP   Transferencia de archivos
TFTP:           69/UDP      Transferencia trivial
SNMP:           161/UDP     Monitoreo de red
Kerberos:       88/TCP+UDP  Autenticación
LDAP:           389/TCP     Directorio

BASES DE DATOS:

MySQL:          3306/TCP
PostgreSQL:     5432/TCP
SQL Server:     1433/TCP
Oracle:         1521/TCP

SERVIDORES:

Apache/Nginx:   80/443
IIS:            80/443
FTP Server:     21
```

#### Socket

```
Combinación de IP + Puerto

Ejemplo: 192.168.1.100:8080
├─ IP: 192.168.1.100
├─ Puerto: 8080
└─ Socket único identifica la conexión

En un servidor pueden estar:
├─ 192.168.1.100:80   → Cliente A navegando
├─ 192.168.1.100:443  → Cliente B descargando
├─ 192.168.1.100:3306 → Aplicación consultando BD
└─ Cada uno es un socket diferente, mismo servidor

En TCP (orientado conexión):
TCP se identifica por: (IP origen, Puerto origen, IP destino, Puerto destino)
└─ Ejemplo: (10.0.0.5, 54321) → (8.8.8.8, 53)
```

---

## 2. Capa de Aplicación (OSI Capa 7)

### 2.1 Funciones

```
┌──────────────────────────────────┐
│ Capa de Aplicación (Capa 7)      │
├──────────────────────────────────┤
│ Responsabilidades:               │
│ • Interfaz con usuario           │
│ • Servicios de red               │
│ • Representación de datos        │
│ • Cifrado/Compresión            │
│ • Autenticación                  │
│                                  │
│ PDU: DATOS (genérico)            │
│ Dispositivos: Clientes/Servidores
│ Usuarios: Humanos                │
└──────────────────────────────────┘
```

### 2.2 HTTP/HTTPS

#### HTTP - HyperText Transfer Protocol

```
Protocolo de Solicitud-Respuesta:

Cliente                      Servidor
   │                            │
   │    HTTP GET /index.html    │
   │──────────────────────────→│
   │  "Dame la página index.html"
   │                            │
   │    HTTP 200 OK             │
   │    [HTML de página]         │
   │←──────────────────────────│
   │  "Aquí está la página"
   │                            │

Estructura HTTP:

REQUEST:
┌─────────────────────────────────┐
│ GET /index.html HTTP/1.1        │ ← Línea de solicitud
│ Host: www.example.com           │ ← Encabezados
│ User-Agent: Chrome              │
│ Accept: text/html               │
│                                 │
│ [Cuerpo vacío en GET]           │
└─────────────────────────────────┘

RESPONSE:
┌─────────────────────────────────┐
│ HTTP/1.1 200 OK                 │ ← Línea de estado
│ Content-Type: text/html         │ ← Encabezados
│ Content-Length: 1234            │
│                                 │
│ <!DOCTYPE html>                 │ ← Cuerpo (contenido)
│ <html>...                       │
└─────────────────────────────────┘
```

#### Métodos HTTP

```
GET:     Solicitar recurso (sin datos en cuerpo)
POST:    Enviar datos al servidor (datos en cuerpo)
PUT:     Actualizar recurso completo
PATCH:   Actualizar parte de recurso
DELETE:  Eliminar recurso
HEAD:    GET pero sin cuerpo (solo encabezados)
OPTIONS: Consultar opciones permitidas
TRACE:   Diagnóstico (raramente usado)
```

#### Códigos de Respuesta HTTP

```
1XX - Informativos
100 Continue        "Continúa enviando"

2XX - Éxito
200 OK              "Exitoso"
201 Created         "Recurso creado"
204 No Content      "Sin contenido (pero OK)"

3XX - Redirección
301 Moved Permanently  "Página movida permanentemente"
302 Found           "Redirecciona temporalmente"
304 Not Modified    "Usa caché local"

4XX - Error del Cliente
400 Bad Request     "Solicitud inválida"
401 Unauthorized    "Requiere autenticación"
403 Forbidden       "No permitido"
404 Not Found       "Página no existe"
429 Too Many Requests "Demasiadas solicitudes (rate limit)"

5XX - Error del Servidor
500 Internal Server Error   "Error servidor"
503 Service Unavailable     "Servidor no disponible"
```

#### HTTPS - HTTP Seguro

```
HTTP + TLS/SSL Encryption

Cliente                      Servidor
   │                            │
   │  TLS Handshake            │
   │  Negocia cifrado          │
   │←────────────────────────→│
   │                            │
   ╔════ Conexión Cifrada ════╗
   │                            │
   │    GET /secure/page       │
   │    (cifrado con TLS)      │
   │──────────────────────────→│
   │                            │
   │    HTTP 200 OK            │
   │    (cifrado con TLS)      │
   │←──────────────────────────│
   │                            │
   ╚════════════════════════════╝

Certificado SSL/TLS:
├─ Verifica identidad del servidor
├─ Cifra datos en tránsito
├─ Protege contra MITM (Man In The Middle)
└─ Típicamente válido 1 año
```

### 2.3 DNS - Domain Name System

#### Propósito

```
Convierte nombres legibles en IPs:

Entrada:  www.google.com
Salida:   142.251.32.142 (IPv4) o 2607:f8b0:4004:809::200e (IPv6)
```

#### Proceso de Resolución

```
Usuario digita: www.google.com

1. Cliente consulta Resolver Local (típicamente ISP)
   └─ ¿Quién es www.google.com?

2. Resolver consulta Nameserver raíz
   └─ ¿Dónde está .com?
   Respuesta: "Pregunta a nameserver de .com"

3. Resolver consulta Nameserver de .com
   └─ ¿Dónde está google.com?
   Respuesta: "Pregunta a nameserver de Google"

4. Resolver consulta Nameserver de Google
   └─ ¿Quién es www.google.com?
   Respuesta: "142.251.32.142"

5. Resolver responde al cliente
   └─ www.google.com = 142.251.32.142

Resultado:
✓ Cliente se conecta a 142.251.32.142
✓ Recibe página de Google

Caché DNS:
├─ Resolver cachea resultados
├─ Cliente cachea resultados
├─ TTL (Time To Live) = cuánto tiempo cachear
└─ Típicamente 3600 segundos (1 hora)
```

#### Registros DNS Comunes

```
A Record:        Dirección IPv4
└─ www.google.com  A  142.251.32.142

AAAA Record:     Dirección IPv6
└─ www.google.com  AAAA  2607:f8b0::142

CNAME Record:    Alias (Canonical Name)
└─ mail.google.com  CNAME  google.com

MX Record:       Mail Exchange (servidores email)
└─ google.com    MX  10  smtp.google.com

NS Record:       Nameserver
└─ google.com    NS  ns1.google.com

TXT Record:      Texto (SPF, DKIM, etc.)
└─ google.com    TXT  "v=spf1 include:_spf.google.com"

SOA Record:      Start of Authority
└─ Información del dominio
```

#### nslookup - Herramienta DNS

```
Comando: nslookup www.google.com

Resultado:
Server:  8.8.8.8            ← Nameserver usado
Address: 8.8.8.8#53

Non-authoritative answer:   ← No es respuesta oficial (cached)
Name:    www.google.com
Addresses: 142.251.32.142
          2607:f8b0:4004:809::200e

Consulta de tipo específico:
nslookup -type=MX google.com
└─ Busca servidores de mail
```

### 2.4 DHCP - Dynamic Host Configuration Protocol

#### Propósito

```
Asignación automática de IPs:

Problema: ¿Cómo conecta un PC nuevo a la red?
Necesita: IP, Máscara, Gateway, DNS

Sin DHCP: Administrador asigna manualmente
Con DHCP: PC obtiene automáticamente
```

#### Proceso DHCP (DORA)

```
Cliente (PC nuevo)      DHCP Server
   │                         │
   │   DISCOVER (Broadcast)  │
   │  "¿Hay un servidor DHCP?"
   │────────────────────────→│ (Broadcast)
   │                         │
   │                    Procesa
   │                         │
   │   OFFER (Unicast)       │
   │  "Aquí hay una IP libre: 10.0.0.100"
   │←────────────────────────│
   │                         │
   │   REQUEST (Broadcast)   │
   │  "Quiero usar 10.0.0.100"
   │────────────────────────→│
   │                         │
   │                    Registra
   │                         │
   │   ACKNOWLEDGE (Unicast) │
   │  "OK, es tuya. Válida por 24h"
   │  Incluye: IP, Máscara, Gateway, DNS
   │←────────────────────────│
   │                         │
   ╔════ CONFIGURACIÓN COMPLETA ════╗

Duración (Lease):
├─ Típicamente 24 horas
├─ Cliente renueva al 50% del tiempo
├─ Si expira sin renovación → IP se libera
└─ PC nuevo puede obtenerla
```

#### Información DHCP Típica

```
Lo que DHCP asigna:

IP Address:         10.0.0.100
Subnet Mask:        255.255.255.0
Default Gateway:    10.0.0.1
DNS Servers:        8.8.8.8, 8.8.4.4
Domain Name:        example.com
Lease Time:         86400 segundos (24h)

Opcional:
NTP Server:         pool.ntp.org
WINS Server:        (antiguado)
Broadcast Address:  10.0.0.255
```

### 2.5 FTP/SFTP - File Transfer

#### FTP - File Transfer Protocol

```
Transferencia de archivos en claro (inseguro):

Cliente                      Servidor
   │                            │
   │  CONNECT (puerto 21)       │
   │──────────────────────────→│
   │  Control                   │
   │                            │
   │  USER admin                │
   │  PASS password             │
   │──────────────────────────→│
   │  Autenticación             │
   │                            │
   │  LIST (puerto 20)          │
   │  (datos)                   │
   │←────────────────────────│
   │  Datos                     │
   │                            │

Puertos:
├─ 21/TCP: Canal de control (comandos)
└─ 20/TCP: Canal de datos (archivos)

Problemas:
✗ Contraseña en claro
✗ Datos en claro
✗ Vulnerabilidades conocidas
└─ OBSOLETO, USE SFTP
```

#### SFTP - SSH File Transfer Protocol

```
Transferencia segura sobre SSH:

Todo encriptado:
├─ Autenticación
├─ Comandos
├─ Datos

Puerto: 22/TCP (mismo que SSH)

Ventajas:
✓ Todo encriptado
✓ Autenticación fuerte (SSH keys)
✓ Más seguro
✓ Recomendado
```

### 2.6 SMTP/POP3/IMAP - Correo Electrónico

#### Arquitectura de Email

```
Cliente de Email           Servidores
(Outlook, Gmail)           (Gmail, Yahoo)

     SMTP ←─────────────────→ SMTP Server
     (envío)                 (envío)
     
     Almacenado en servidor
     
     POP3 ←─────────────────→ POP3 Server
     (recepción)             (recepción)
     
     O
     
     IMAP ←─────────────────→ IMAP Server
     (sincronización)        (sincronización)
```

#### SMTP - Simple Mail Transfer Protocol

```
Envío de email:

Función: Cliente → Servidor (envío)

Proceso:
1. Conecta a servidor SMTP (puerto 25 o 587)
2. EHLO (identifica cliente)
3. MAIL FROM: (remitente)
4. RCPT TO: (destinatarios)
5. DATA (cuerpo del mensaje)
6. QUIT (desconecta)

Seguro:
├─ SMTP STARTTLS: Encriptación
├─ Puerto 587 (estándar moderno)
└─ Autenticación requerida
```

#### POP3 - Post Office Protocol

```
Recepción de email (descarga):

Función: Servidor → Cliente (recepción)

Características:
├─ Descarga emails a PC local
├─ Los elimina del servidor (por defecto)
├─ Usuario ve solo lo que descargó
├─ Bajo uso de almacenamiento servidor
├─ No sincroniza entre dispositivos

Puerto: 110/TCP (inseguro) o 995 (SSL)

Problema:
✗ Si cambias dispositivo, pierdes emails viejos
✗ No sincroniza
└─ Mejor usar IMAP
```

#### IMAP - Internet Message Access Protocol

```
Acceso a email (sincronización):

Función: Servidor ↔ Cliente (sincronización)

Características:
├─ Sincroniza entre dispositivos
├─ Emails permanecen en servidor
├─ Usuario ve lo mismo en PC, tablet, móvil
├─ Mayor uso de almacenamiento servidor
├─ Más lento que POP3 (menor ancho de banda)

Puerto: 143/TCP (insecuro) o 993 (SSL)

Ventajas:
✓ Acceso desde múltiples dispositivos
✓ Sincronización automática
✓ Moderno y recomendado
✓ Mejor experiencia usuario

Estructura:
├─ Carpeta Entrada
├─ Carpeta Salida
├─ Carpeta Borradores
└─ Carpetas personalizadas (Proyecto A, etc.)
```

### 2.7 SSH - Secure Shell

#### Propósito

```
Control remoto seguro de un servidor:

Sin SSH (Telnet - INSEGURO):
┌─────────────────────────────┐
│ telnet 192.168.1.100        │
│ login: admin                │
│ password: cisco123          │ ← En claro!
│ # ls                        │
│ Total inseguro ❌           │
└─────────────────────────────┘

Con SSH (SEGURO):
┌─────────────────────────────┐
│ ssh admin@192.168.1.100     │
│ password: ****              │ ← Cifrado
│ # ls                        │
│ Total seguro ✓              │
└─────────────────────────────┘
```

#### Autenticación SSH

**Por Contraseña**
```
ssh user@host
Prompt: Enter password: ****
```

**Por Clave (Más Seguro)**
```
Generar par de claves:
ssh-keygen -t rsa -b 4096

Resultado:
├─ id_rsa (clave privada - guardar local)
└─ id_rsa.pub (clave pública - copiar al servidor)

Conectar:
ssh user@host
└─ Autentica automáticamente con claves
└─ No pide contraseña
```

#### Tunneling SSH

```
Seguridad para otros servicios:

Servicio inseguro en localhost:8080

Problema: ¿Cómo acceder de forma segura?

SSH Tunnel:
ssh -L 3000:localhost:8080 user@remote

├─ Localport (3000): En tu PC
├─ Remote (localhost:8080): En el servidor remoto
└─ Accesa: localhost:3000 en tu PC
            ↓ (encriptado)
            Conecta a remoto:8080

Resultado:
✓ Todo encriptado
✓ Tráfico seguro
✓ Acceso remoto seguro
```

---

## 3. Flujo de Datos Completo

### 3.1 Ejemplo: Acceder a www.google.com

```
Paso 1: RESOLUCIÓN DNS (Capa 7)
├─ Browser pregunta: ¿IP de www.google.com?
├─ Cliente resuelve: 142.251.32.142
└─ Se cachea

Paso 2: TCP HANDSHAKE (Capa 4)
├─ Browser → Google: SYN (seq=1000)
├─ Google → Browser: SYN-ACK (seq=5000, ack=1001)
├─ Browser → Google: ACK (seq=1001, ack=5001)
└─ Conexión TCP establecida

Paso 3: TLS HANDSHAKE (HTTPS - Capa 7)
├─ Browser → Google: ClientHello
├─ Google → Browser: ServerHello + Certificado
├─ Negociación de cifrado
└─ Conexión encriptada

Paso 4: HTTP REQUEST (Capa 7)
GET / HTTP/1.1
Host: www.google.com
User-Agent: Chrome
Accept: text/html
├─ Envuelto en TLS
├─ Encapsulado en TCP segmento
├─ Encapsulado en IPv4 paquete
├─ Encapsulado en Ethernet trama
└─ Transmitido por red

Paso 5: ENRUTAMIENTO (Capas 3-2)
├─ Paquete pasa por múltiples routers
├─ Cada router verifica IP destino
├─ Encamina al siguiente salto
├─ En cada salto, MAC se reemplaza
└─ IPv4 original permanece intacto

Paso 6: HTTP RESPONSE (Capa 7)
Google → Browser:
HTTP/1.1 200 OK
Content-Type: text/html
[HTML de Google]

├─ Encriptado con TLS
├─ Encapsulado en TCP segmento
├─ Encapsulado en IPv4 paquete
└─ Llega a tu PC

Paso 7: DESENCAPSULACIÓN (Capas 2-7 inverso)
├─ Ethernet desencapsula → IPv4 paquete
├─ IPv4 desencapsula → TCP segmento
├─ TCP desencapsula → TLS frame
├─ TLS desencapsula → HTTP respuesta
└─ Browser recibe HTML

Paso 8: RENDERIZADO (Capa 7)
├─ Browser procesa HTML
├─ Carga CSS, JavaScript, imágenes
├─ Múltiples conexiones HTTP/2
└─ Página se muestra en pantalla
```

---

## 4. Resumen - Capítulos 14-15

### Capa de Transporte
1. **TCP**: Confiable, ordenado, con estado
2. **UDP**: Rápido, sin conexión, sin garantías
3. **Puertos**: Identifican aplicaciones
4. **Sockets**: IP + Puerto = conexión única

### Capa de Aplicación
1. **HTTP/HTTPS**: Web
2. **DNS**: Nombres → IPs
3. **DHCP**: Configuración automática
4. **SMTP/POP3/IMAP**: Email
5. **FTP/SFTP**: Transferencia de archivos
6. **SSH**: Control remoto seguro

### Decisiones de Diseño
```
¿TCP o UDP?
├─ TCP: Datos críticos (web, email, archivos)
└─ UDP: Tiempo real (voz, video, gaming)

¿HTTP o HTTPS?
├─ HTTP: Desarrollo local
└─ HTTPS: SIEMPRE en producción

¿POP3 o IMAP?
├─ IMAP: Moderno, múltiples dispositivos
└─ POP3: Simple, cliente único

¿FTP o SFTP?
├─ SFTP: SIEMPRE, es seguro
└─ FTP: NUNCA (obsoleto)

¿SSH o Telnet?
├─ SSH: SIEMPRE
└─ Telnet: NUNCA (inseguro)
```

---

## Referencias

- RFC 793 (TCP)
- RFC 768 (UDP)
- RFC 7230-7237 (HTTP/1.1)
- RFC 7540 (HTTP/2)
- RFC 1035 (DNS)
- RFC 2131 (DHCP)
- RFC 5321 (SMTP)
- RFC 3501 (IMAP)
- RFC 1939 (POP3)
- RFC 4251 (SSH)
