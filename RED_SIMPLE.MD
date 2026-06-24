## Descripción

Este esquema de red implementa:
- Router ISP (1941) simulando un proveedor de internet
- Servidores DNS y WEB conectados al ISP
- Router empresarial con PC cliente
- Enrutamiento dinámico y NAT para acceso a internet

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