# Nota Avanzada - Clase 6: DHCP, SLAAC y DHCPv6

## Introducción
La asignación dinámica de direcciones IP reduce la carga administrativa y previene errores (como direcciones duplicadas). En IPv4 usamos principalmente DHCP, pero en IPv6 existen múltiples métodos, incluyendo SLAAC y variaciones de DHCPv6.

## 1. DHCPv4 (Dynamic Host Configuration Protocol)
Funciona mediante el proceso DORA (Discover, Offer, Request, Acknowledge).

### 1.1 Configuración de Servidor DHCP en Router IOS
```text
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
Router(config)# ip dhcp pool LAN_VENTAS
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8
```

### 1.2 Agente de Retransmisión (DHCP Relay)
Si el servidor DHCP está en una red diferente al cliente, los broadcasts DHCP Discover no pasarán el router. El router local debe configurarse como "Agente de Retransmisión" para convertir el broadcast en unicast hacia el servidor.
```text
Router(config-if)# ip helper-address [IP_del_Servidor_DHCP]
```

## 2. Asignación Dinámica en IPv6

IPv6 utiliza ICMPv6 y su protocolo de descubrimiento de vecinos (NDP).
- **RS (Router Solicitation):** El cliente pide información de red (Mensaje Multicast al grupo de routers FF02::2).
- **RA (Router Advertisement):** El router responde con los parámetros de red (Mensaje Multicast a todos los nodos FF02::1).

### 2.1 SLAAC (Stateless Address Autoconfiguration)
Es la opción por defecto. El router envía el **Prefijo** (ej. /64) en el mensaje RA.
El host genera su propio ID de Interfaz (los últimos 64 bits) usando:
- Generación aleatoria (Privacy Extensions).
- **EUI-64:** Toma la dirección MAC, la parte por la mitad, inserta `FF:FE` en medio e invierte el 7º bit.

### 2.2 DHCPv6 Sin Estado (Stateless)
- Combina SLAAC con DHCPv6.
- El cliente obtiene IP, Prefijo y Gateway mediante SLAAC (mensajes RA).
- El cliente busca un servidor DHCPv6 **solo** para obtener parámetros adicionales (como el servidor DNS o el dominio).

### 2.3 DHCPv6 Con Estado (Stateful)
- Similar al DHCPv4 tradicional.
- El servidor DHCPv6 asigna la dirección IPv6 completa y todos los demás parámetros. Lleva un registro (estado) de qué IP tiene cada cliente.
- El router se configura para decirle al cliente (en el mensaje RA) que no use SLAAC, sino que contacte al DHCPv6.
