# Nota Avanzada - Capítulo 11: Configuración de Dispositivos e Interfaces

## Introducción

Este capítulo cubre cómo configurar routers y switches Cisco, establecer seguridad mediante líneas de consola y acceso remoto (Telnet y SSH). Es fundamental entender estos conceptos para administrar redes.

---

## 1. Configuración de Interfaces

### 1.1 Modo de Configuración en Cisco IOS

```
Jerarquía de modos:

┌─────────────────────────────────────────────────┐
│ User EXEC Mode                                  │
│ Router>                                         │
│ ├─ Ver información (show, ping, traceroute)    │
│ └─ NO se pueden hacer cambios                  │
└─────────────────────────────────────────────────┘
         ↓ enable
┌─────────────────────────────────────────────────┐
│ Privileged EXEC Mode                            │
│ Router#                                         │
│ ├─ Ver configuración completa                   │
│ ├─ Acceso a configuración                       │
│ └─ debug y commandos peligrosos                 │
└─────────────────────────────────────────────────┘
         ↓ configure terminal (o conf t)
┌─────────────────────────────────────────────────┐
│ Global Configuration Mode                       │
│ Router(config)#                                 │
│ ├─ Configurar router globalmente                │
│ ├─ Ingresar a submodos                          │
│ └─ Aplicar cambios                              │
└─────────────────────────────────────────────────┘
         ↓ interface TIPO NÚMERO
┌─────────────────────────────────────────────────┐
│ Interface Configuration Mode                    │
│ Router(config-if)#                              │
│ ├─ Configurar interfaz específica               │
│ └─ Aplicar dirección IP, velocidad, duplex     │
└─────────────────────────────────────────────────┘

Navegación:
Router> enable                    ← Privileged
Router# configure terminal        ← Global config
Router(config)# interface fa 0/0  ← Interface config
Router(config-if)# exit           ← Volver a global
Router(config)# exit              ← Volver a exec
Router# disable                   ← User mode
```

### 1.2 Configurar Interfaz Ethernet (IPv4)

```
Tarea: Configurar interfaz Ethernet en router

Router> enable
Router# configure terminal
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
Router# write memory

Explicación:

interface gigabitethernet 0/0
├─ Selecciona interfaz GigaEthernet 0/0
└─ Puede ser: fa 0/0, gig 0/0, gig 0/1, etc.

ip address 192.168.1.1 255.255.255.0
├─ IP: 192.168.1.1
├─ Máscara: 255.255.255.0 (/24)
└─ Asigna dirección IP a esta interfaz

no shutdown
├─ Activa la interfaz
├─ Por defecto, interfaces están en shutdown (inactivas)
└─ Sin esto, interfaz está down

exit
├─ Sale del modo interface
└─ Vuelve al modo global config

write memory
├─ Guarda configuración
├─ Sin esto, cambios se pierden al reiniciar
├─ Sinónimo: copy running-config startup-config
└─ Sinónimo corto: wr
```

### 1.3 Configurar Interfaz Ethernet (IPv6)

```
Tarea: Agregar IPv6 a la misma interfaz

Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8::1/64
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown
Router(config-if)# exit

Resultado: Dual Stack
├─ IPv4: 192.168.1.1 /24
├─ IPv6: 2001:db8::1 /64
└─ Ambos activos

Verificar:
Router# show interface gig 0/0
├─ Muestra estado de la interfaz
├─ Dirección IP
├─ Estadísticas
└─ Encapsulación
```

### 1.4 Configuración Rápida (Setup)

```
Primer inicio de router - Setup interactivo:

Router> enable
Router# setup

┌─ Wizard configuración
├─ Pregunta SSID (WiFi)
├─ Pregunta interfaz WAN
├─ Pregunta IP WAN
├─ Pregunta protocolo enrutamiento
└─ Genera configuración básica

Útil para configuración inicial rápida
NO recomendado para producción
```

### 1.5 Deshabilitar DNS Lookup

```
Problema: Cuando haces typo en comando, router
intenta resolver DNS y espera timeouts

Solución:

Router(config)# no ip domain-lookup

Resultado:
├─ Los comandos inválidos fallan inmediatamente
├─ Sin espera de DNS
└─ Más rápido trabajar
```

---

## 2. Securización - Línea de Consola

### 2.1 Proteger Acceso Console (Física)

```
Problema: Cualquiera con acceso físico puede:
├─ Conectarse al puerto console
├─ Cambiar configuración
├─ Resetear router
└─ Acceso no autorizado

Solución: Proteger con contraseña la línea console

Router(config)# line console 0
Router(config-line)# password cisco
Router(config-line)# login
Router(config-line)# exit

Explicación:

line console 0
├─ Configura línea de consola
├─ Puede haber línea 0, 1, 2, etc.
└─ Acceso físico directo

password cisco
├─ Establece contraseña
├─ Contraseña: cisco
└─ Se ve en claro en configuración (no seguro)

login
├─ Habilita solicitud de contraseña
├─ Sin esto, el "password" no se usa
└─ Obligatorio para que funcione

Resultado:
├─ Usuario debe ingresar contraseña en consola
├─ Acceso controlado
└─ Mejor seguridad (física aún vulnerable)
```

### 2.2 Proteger Enable (Acceso Privilegiado)

```
Problema: Una vez en modo usuario, acceso a enable
sin protección es posible

Solución: Proteger modo enable

Router(config)# enable secret cisco123

Explicación:

enable secret
├─ Reemplaza "enable password"
├─ MÁS SEGURO (encriptado)
├─ Se almacena con MD5 hash
└─ Contraseña de acceso a modo privilegiado

Sintaxis alternativa menos segura:
enable password cisco123
├─ Se ve PARCIALMENTE encriptado
├─ Débil
└─ No usar

Resultado:
Router> enable
Password: ****
Router#

Router debe ingresar contraseña para acceso privilegiado
```

### 2.3 Proteger Acceso VTY (Telnet/SSH)

```
Problema: Usuarios remotos pueden acceder sin protección
via Telnet/SSH

Solución: Proteger líneas VTY

Router(config)# line vty 0 4
Router(config-line)# password cisco456
Router(config-line)# login
Router(config-line)# exit

Explicación:

line vty 0 4
├─ VTY = Virtual TeleTYpe
├─ 0 4 = líneas 0, 1, 2, 3, 4 (5 conexiones simultáneas)
├─ Típicamente 0-15 para más conexiones
└─ Para acceso Telnet/SSH

password cisco456
├─ Contraseña para Telnet/SSH
├─ Diferente del console
└─ Mejor dejarlo diferente

login
├─ Habilita solicitud de contraseña
└─ Sin esto, no se requiere contraseña

Resultado:
Desde remoto:
telnet 192.168.1.1
Password: ****
Router>

Acceso remoto con contraseña
```

---

## 3. Autenticación Mejorada (Username/Password)

### 3.1 Crear Base de Datos Local de Usuarios

```
Problema: Usar misma contraseña para todos no escala

Solución: Crear usuarios locales

Router(config)# username admin password cisco123
Router(config)# username operator password operator456

Explicación:

username admin password cisco123
├─ Crea usuario "admin"
├─ Contraseña: cisco123
└─ Almacenado localmente

Resultado:
├─ Dos usuarios creados
├─ Diferentes permisos (opcional)
└─ Mejor administración
```

### 3.2 Usar Usuarios en Línea Console

```
Cambiar console para usar usuarios:

Router(config)# line console 0
Router(config-line)# login local
Router(config-line)# exit

Explicación:

login local
├─ Usa base de datos local de usuarios
├─ Reemplaza "login + password"
└─ Requiere username Y password

Resultado anterior (password):
Router con # prompts
Username: [no solicitado]
Password: cisco

Resultado nuevo (login local):
Router con # prompts
Username: admin
Password: ****
Router>

Ahora requiere username específico
```

### 3.3 Usar Usuarios en Línea VTY

```
Cambiar VTY para usar usuarios:

Router(config)# line vty 0 4
Router(config-line)# login local
Router(config-line)# transport input ssh telnet
Router(config-line)# exit

Explicación:

login local
├─ Usa usuarios locales en VTY también
└─ username + password requeridos

transport input ssh telnet
├─ Permite acceso por SSH o Telnet
├─ "transport input ssh" = solo SSH (más seguro)
├─ "transport input none" = no permitir acceso
└─ Controla qué protocolos se permiten

Resultado:
telnet 192.168.1.1
Username: admin
Password: ****
Router>

SSH 192.168.1.1
Username: admin
Password: ****
Router>
```

---

## 4. Telnet - Acceso Remoto (Inseguro)

### 4.1 Configuración Básica Telnet

```
Requisitos previos:

Router(config)# hostname R1
Router(config)# line vty 0 4
Router(config-line)# password telnet123
Router(config-line)# login
Router(config-line)# exit
```

### 4.2 Conectarse via Telnet

```
Desde PC cliente:

telnet 192.168.1.1
Trying 192.168.1.1...
Connected to 192.168.1.1
Escape character is '^]'.

Password: telnet123
R1>

PROBLEMAS CON TELNET:
✗ Contraseña en claro (capturada fácilmente)
✗ Todo el tráfico en claro
✗ Vulnerable a MITM
✗ Obsoleto
✗ NO USAR EN PRODUCCIÓN

Salir:
logout
o
[Ctrl+]
telnet> quit
```

---

## 5. SSH - Acceso Remoto Seguro (Recomendado)

### 5.1 Requisitos para SSH

```
Para que SSH funcione necesitamos:

1. Nombre de host configurado
2. Dominio configurado
3. Pares RSA (claves criptográficas)
4. Usuarios configurados
5. Línea VTY configurada para SSH
```

### 5.2 Configuración SSH Paso a Paso

```
Paso 1: Configurar hostname

Router(config)# hostname R1

Paso 2: Configurar dominio

Router(config)# ip domain-name ccna.com

Paso 3: Generar pares RSA (claves SSH)

Router(config)# crypto key generate rsa
The name for the keys will be: R1.ccna.com
How many bits in the modulus [512]: 1024
Generating RSA keys... [OK]

Explicación:
├─ Genera par de claves RSA
├─ Modulus 512 (débil), 1024 (recomendado), 2048 (fuerte)
└─ Guarda en memoria del router

Paso 4: Crear usuarios SSH

Router(config)# username admin privilege 15 secret cisco123

Explicación:
├─ username: admin
├─ privilege 15: acceso total (máximo)
├─ secret: cifrado MD5
└─ Contraseña: cisco123

Paso 5: Configurar línea VTY para SSH

Router(config)# line vty 0 4
Router(config-line)# login local
Router(config-line)# transport input ssh
Router(config-line)# exit

Explicación:
├─ login local: usa usuarios locales
├─ transport input ssh: solo SSH (no Telnet)
└─ Línea 0-4: 5 conexiones simultáneas

Paso 6: Guardar configuración

Router(config)# exit
Router# write memory
Building configuration...
[OK]
```

### 5.3 Conectarse via SSH desde Cliente

```
Linux/Mac:

ssh -l admin 192.168.1.1
Password: ****
R1>

SSH -L admin 192.168.1.1  (alternativa)

Windows (requiere cliente SSH):
- PuTTY (descargable)
- Windows 10+ tiene openssh

putty.exe -ssh admin@192.168.1.1

Opciones SSH útiles:

ssh -p 2222 admin@192.168.1.1
├─ Puerto personalizado 2222
└─ Si SSH no está en puerto 22

ssh -i id_rsa admin@192.168.1.1
├─ Usar archivo de clave privada
├─ Sin contraseña (autenticación de clave)
└─ Más seguro que contraseña

ssh -v admin@192.168.1.1
├─ Verbose: muestra detalles de conexión
├─ Útil para troubleshooting
└─ -vv para más detalle
```

### 5.4 Ventajas SSH sobre Telnet

```
TELNET (Inseguro):
✗ Autenticación en claro
✗ Datos en claro
✗ Vulnerable a captura
✗ Vulnerable a MITM
✗ Obsoleto

SSH (Seguro):
✓ Autenticación encriptada
✓ Todo encriptado
✓ Muy difícil de interceptar
✓ Seguro contra MITM
✓ Estándar moderno

Comparación:

Telnet puerto 23:
telnet 192.168.1.1
Password: cisco
└─ Capturado por wireshark: "cisco"

SSH puerto 22:
ssh admin@192.168.1.1
Password: ****
└─ Capturado por wireshark: solo ruido cifrado
```

---

## 6. Configuración de Switch

### 6.1 Diferencias Switch vs Router

```
ROUTER:
├─ IP addresses en interfaces
├─ Enrutamiento (toma decisiones Capa 3)
├─ Múltiples protocolos de enrutamiento
└─ Gateway para otras redes

SWITCH:
├─ Normalmente NO tiene IP (salvo VLAN 1)
├─ Conmutación (Capa 2)
├─ Simple switching entre puertos
├─ Acceso a todos los puertos en red local
```

### 6.2 Configurar Switch para Acceso Remoto

```
Problema: Switch no tiene interfaz Ethernet para IPv4
Solución: Usar "interface vlan 1" como interfaz lógica

Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 192.168.1.1
Switch(config)# exit
Switch# write memory

Explicación:

interface vlan 1
├─ VLAN 1 es por defecto en switches
├─ Todos los puertos pertenecen a VLAN 1
├─ Se usa como interfaz de administración
└─ IP se asigna aquí

ip address 192.168.1.2 255.255.255.0
├─ IP del switch: 192.168.1.2
├─ Máscara: 255.255.255.0
└─ Ahora switch es alcanzable en red

ip default-gateway 192.168.1.1
├─ Gateway para salir de la VLAN local
├─ Típicamente IP del router
└─ Necesario para SSH/Telnet remoto

Resultado:
Switch ahora tiene IP 192.168.1.2
└─ Puede recibir Telnet/SSH en esa IP
└─ Puede hacer ping
```

### 6.3 Proteger Switch (Telnet/SSH)

```
Igual que router:

Switch(config)# username admin password switch123
Switch(config)# line vty 0 4
Switch(config-line)# login local
Switch(config-line)# password vty_password
Switch(config-line)# exit
Switch(config)# enable secret enable_password
Switch(config)# line console 0
Switch(config-line)# password console_password
Switch(config-line)# login
Switch(config-line)# exit
Switch# wr

Para SSH (igual que router):

Switch(config)# hostname SW1
Switch(config)# ip domain-name ccna.com
Switch(config)# crypto key generate rsa modulus 1024
Switch(config)# username admin secret cisco
Switch(config)# line vty 0 4
Switch(config-line)# login local
Switch(config-line)# transport input ssh
```

---

## 7. Comandos Útiles de Verificación

### 7.1 Ver Configuración

```
show running-config
├─ Configuración actual en RAM
├─ Cambios recientes no guardados
└─ Completa

show startup-config
├─ Configuración guardada en NVRAM
├─ Lo que se cargará al reiniciar
└─ Completa

show interface gig 0/0
├─ Estado detallado de interfaz
├─ IP, encapsulación, estadísticas
└─ Detecta problemas

show ip interface brief
├─ Resumen rápido de interfaces
├─ IP, estado (up/down)
└─ Muy útil

show version
├─ Información de router/switch
├─ Versión IOS
├─ Uptime
└─ Serial number

show ip route
├─ Tabla de enrutamiento
├─ Redes conectadas
├─ Rutas aprendidas
└─ Métrica

show users
├─ Usuarios conectados
├─ Líneas activas
└─ Sesiones remotas

show line
├─ Líneas disponibles
├─ VTY, console
└─ Configuración
```

### 7.2 Ping y Traceroute

```
ping 192.168.1.1
├─ Verifica conectividad
├─ 4 intentos por defecto
└─ Muestra RTT (latencia)

ping 192.168.1.1 -c 100
├─ 100 intentos (Linux)
└─ Útil para análisis prolongado

traceroute 192.168.1.1 (Linux/Mac)
o
tracert 192.168.1.1 (Windows)
├─ Muestra ruta hacia destino
├─ Cada router en el camino
└─ Detecta donde falla conectividad
```

### 7.3 Debug

```
debug ip icmp
├─ Muestra paquetes ICMP
├─ Útil para troubleshooting ping
└─ Genera mucho output

debug ip packet
├─ Muestra todos los paquetes IP
├─ PELIGRO: genera mucho tráfico de CPU
└─ Usar solo en routers dedicados

undebug all
o
no debug all
├─ Detiene todos los debugs
├─ IMPORTANTE: puede ralentizar router
└─ Siempre desactivar después
```

---

## 8. Guardando Configuración

### 8.1 Guardar de Forma Segura

```
Cambios en RAM (temporal):
Router# configure terminal
Router(config)# ...hacer cambios...

Guardar a NVRAM (permanente):

Opción 1 (más corta):
Router# wr
Building configuration...
[OK]

Opción 2 (más clara):
Router# write memory
Building configuration...
[OK]

Opción 3 (estándar):
Router# copy running-config startup-config
Destination filename [startup-config]?
[OK]

IMPORTANTE:
Después de cualquier cambio:
└─ SIEMPRE hacer write memory
└─ Si no lo haces, cambios se pierden en reinicio
```

### 8.2 Backup de Configuración

```
Guardar en servidor TFTP:

Router# copy running-config tftp://192.168.1.100/R1.bak
Address or name of remote host []? 192.168.1.100
Destination filename [r1.txt]? R1.bak

[OK - 1234 bytes]

Restaurar de backup:

Router# copy tftp://192.168.1.100/R1.bak running-config
Address or name of remote host []? 192.168.1.100
Source filename []? R1.bak

[OK - 1234 bytes]

NOTA: Después de restaurar desde backup
Router# wr
└─ Para guardar cambios
```

---

## 9. Resumen - Capítulo 11

### Seguridad en Capas

```
LÍNEA CONSOLE (Acceso físico):
Router(config)# line console 0
Router(config-line)# password cisco
Router(config-line)# login

ENABLE (Modo privilegiado):
Router(config)# enable secret cisco123

VTY (Acceso remoto):
Router(config)# line vty 0 4
Router(config-line)# password cisco456
Router(config-line)# login

USUARIOS LOCALES (Mejor):
Router(config)# username admin secret cisco123
Router(config)# line console 0
Router(config-line)# login local
```

### Acceso Remoto

```
TELNET (Inseguro):
✗ Datos en claro
✗ NO USAR

SSH (Seguro):
✓ Todo encriptado
✓ USAR SIEMPRE

Configuración SSH:
1. hostname + domain
2. crypto key generate rsa
3. username con secret
4. line vty con login local y transport input ssh
```

### Verificación

```
show running-config      ← Configuración actual
show startup-config      ← Configuración guardada
show ip interface brief  ← Estado de interfaces
show ip route           ← Tabla de enrutamiento
ping destino            ← Verificar conectividad
traceroute destino      ← Ver ruta
```

---

## Referencias

- Cisco IOS Command Reference
- RFC 854 (Telnet)
- RFC 4251 (SSH)
- RFC 3389 (SFTP)
