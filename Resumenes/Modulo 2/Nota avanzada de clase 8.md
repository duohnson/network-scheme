# Nota Avanzada - Clase 8: Conceptos de Seguridad en LAN

## Introducción
Las redes de área local (LAN) son vulnerables a una variedad de ataques de Capa 2 que pueden comprometer la confidencialidad, integridad y disponibilidad de los datos. A diferencia de los ataques de Capa 3, muchos ataques de Capa 2 son difíciles de detectar con firewalls tradicionales porque operan por debajo del nivel IP.

---

## 1. Categorías de Amenazas de Seguridad

| Categoría | Descripción | Ejemplo |
|-----------|-------------|---------|
| **Reconocimiento** | Recopilar información de la red | Escaneo de puertos, CDP/LLDP reconnaissance |
| **Acceso** | Explotar vulnerabilidades para acceso no autorizado | ARP spoofing, DHCP spoofing |
| **Denegación de Servicio (DoS)** | Saturar recursos para negar servicio | MAC flooding, DHCP starvation |

---

## 2. Ataques de Capa 2 en Detalle

### 2.1 MAC Address Table Overflow (MAC Flooding)
El atacante inunda el switch con miles de direcciones MAC falsas usando herramientas como **macof**.

**Cómo funciona:**
1. La tabla MAC del switch tiene capacidad limitada (ej: 8,000 entradas).
2. El atacante genera tramas con MACs de origen aleatorias.
3. La tabla MAC se llena completamente.
4. El switch no puede aprender MACs legítimas → hace **flooding** de todo el tráfico por todos los puertos.
5. El atacante puede capturar todo el tráfico (sniffing).

**Contramedida:** Port Security (limita las MACs por puerto).

### 2.2 VLAN Hopping
Permite al atacante saltar entre VLANs sin autorización.

#### Switch Spoofing
El atacante configura su NIC para negociar un enlace troncal con el switch usando DTP.

```text
Atacante → negocia DTP → Switch cree que es un switch → Trunk activado → Acceso a todas las VLANs
```

**Contramedida:**
```text
Switch(config-if)# switchport mode access          ! Forzar modo acceso
Switch(config-if)# switchport nonegotiate           ! Deshabilitar DTP
```

#### Double Tagging (Doble Etiquetado)
El atacante envía una trama con dos etiquetas 802.1Q. El switch elimina la primera etiqueta (VLAN nativa) y reenvía la trama con la segunda etiqueta hacia la VLAN destino.

**Condiciones para el ataque:**
- El atacante debe estar en la misma VLAN que la VLAN nativa.
- Solo funciona en una dirección (no hay respuesta).

**Contramedida:**
```text
Switch(config-if)# switchport trunk native vlan 999   ! Cambiar VLAN nativa a una no usada
```

### 2.3 Ataques DHCP

#### DHCP Starvation
El atacante envía cientos de mensajes DHCP Discover con MACs falsas, agotando todas las IPs del pool.

#### DHCP Spoofing
El atacante levanta un servidor DHCP falso (rogue) que responde más rápido que el legítimo, asignando:
- Un **gateway falso** → redirige el tráfico (Man-in-the-Middle)
- Un **DNS falso** → redirige a sitios maliciosos

**Contramedida:** DHCP Snooping (ver Clase 9).

### 2.4 ARP Spoofing / ARP Poisoning
El atacante envía respuestas ARP falsas (gratuitous ARP) asociando su MAC con la IP del gateway legítimo.

**Resultado:** Man-in-the-Middle. Todo el tráfico pasa por el atacante antes de llegar al gateway real.

```text
PC1 → "¿Quién tiene 192.168.1.1?" → ARP Request
Atacante → "192.168.1.1 está en AA:BB:CC:DD:EE:FF" → ARP Reply falso
PC1 → Envía todo su tráfico al atacante pensando que es el gateway
```

**Contramedida:** Dynamic ARP Inspection (DAI) (ver Clase 9).

### 2.5 CDP/LLDP Reconnaissance
CDP (Cisco Discovery Protocol) y LLDP (Link Layer Discovery Protocol) envían información en texto plano sobre los dispositivos vecinos: modelo, versión de IOS, IPs, plataforma.

**Contramedida:**
```text
Switch(config)# no cdp run                         ! Deshabilitar CDP globalmente
Switch(config-if)# no cdp enable                    ! Deshabilitar CDP en una interfaz
Switch(config)# no lldp run                         ! Deshabilitar LLDP
```

---

## 3. Mejores Prácticas de Seguridad en la LAN

### 3.1 Deshabilitar puertos no utilizados
```text
Switch(config)# interface range FastEthernet 0/10-24
Switch(config-if-range)# shutdown
Switch(config-if-range)# exit
```

### 3.2 Asignar puertos no usados a una VLAN "parking lot"
```text
Switch(config)# vlan 999
Switch(config-vlan)# name PARKING_LOT
Switch(config-vlan)# exit
Switch(config)# interface range FastEthernet 0/10-24
Switch(config-if-range)# switchport access vlan 999
Switch(config-if-range)# shutdown
```

### 3.3 Deshabilitar DTP en puertos de acceso
```text
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate
```

### 3.4 Cambiar la VLAN nativa
```text
Switch(config-if)# switchport trunk native vlan 99
```

### 3.5 Configurar PortFast y BPDU Guard
```text
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable
```

---

## 4. Defensa en Profundidad

La seguridad de LAN no depende de una sola medida. Se implementan **múltiples capas de protección**:

| Capa | Mecanismo | Protege contra |
|------|-----------|---------------|
| Puerto | Port Security | MAC Flooding |
| DHCP | DHCP Snooping | DHCP Starvation/Spoofing |
| ARP | DAI (Dynamic ARP Inspection) | ARP Spoofing |
| IP | IP Source Guard | IP Spoofing |
| STP | BPDU Guard + PortFast | Switches no autorizados |
| Trunk | Deshabilitar DTP | VLAN Hopping |

---

## Resumen
- Los ataques de Capa 2 son peligrosos porque operan por debajo de los firewalls tradicionales.
- **MAC Flooding** convierte al switch en un hub; se contrarresta con Port Security.
- **VLAN Hopping** se previene desactivando DTP y cambiando la VLAN nativa.
- **DHCP Spoofing** y **ARP Spoofing** permiten ataques Man-in-the-Middle; se contrarrestan con DHCP Snooping y DAI.
- La defensa en profundidad combina múltiples mecanismos de seguridad en diferentes capas.
