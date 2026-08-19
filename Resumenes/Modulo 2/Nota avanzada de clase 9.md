# Nota Avanzada - Clase 9: Configuración de Seguridad en Switches

## Introducción
Esta nota cubre la implementación práctica de las contramedidas de seguridad de Capa 2: Port Security, DHCP Snooping, Dynamic ARP Inspection (DAI) e IP Source Guard.

---

## 1. Port Security

Limita las direcciones MAC permitidas en un puerto del switch.

### 1.1 Tipos de Aprendizaje de MACs

| Tipo | Descripción | Persistencia |
|------|-------------|-------------|
| **Estática** | MAC configurada manualmente | Se guarda en config |
| **Dinámica** | MAC aprendida automáticamente | Se pierde al reiniciar |
| **Sticky** | Aprendida dinámicamente, se adhiere a config | Persiste en running-config |

### 1.2 Configuración
```text
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation shutdown
```

### 1.3 Modos de Violación

| Modo | Descarta | Log | Apaga puerto |
|------|:-:|:-:|:-:|
| **Shutdown** | ✅ | ✅ | ✅ (err-disabled) |
| **Restrict** | ✅ | ✅ | ❌ |
| **Protect** | ✅ | ❌ | ❌ |

### 1.4 Recuperación de err-disabled
```text
! Manual
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

! Automática
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 600
```

---

## 2. DHCP Snooping

Clasifica puertos como **trusted** o **untrusted** y filtra mensajes DHCP maliciosos.

- **Trusted:** Hacia servidor DHCP o trunk. Permite todo.
- **Untrusted:** Hacia hosts. Solo permite Discover/Request, bloquea Offer/Ack.

### Configuración
```text
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10,20

Switch(config-if)# ip dhcp snooping trust           ! Puerto confiable

Switch(config-if-range)# ip dhcp snooping limit rate 6  ! Limitar tasa
```

### Tabla de Binding
DHCP Snooping construye una tabla que asocia MAC, IP, lease, VLAN y puerto. Esta tabla la usan DAI e IP Source Guard.

---

## 3. Dynamic ARP Inspection (DAI)

Valida paquetes ARP contra la tabla de binding de DHCP Snooping.

```text
Switch(config)# ip arp inspection vlan 10,20

Switch(config-if)# ip arp inspection trust     ! Puerto confiable (uplink)
```

> **Prerequisito:** DHCP Snooping debe estar habilitado.

Para hosts con IP estática, crear ARP ACLs manualmente:
```text
Switch(config)# arp access-list STATIC_HOSTS
Switch(config-arp-nacl)# permit ip host 192.168.1.100 mac host 00A1.B2C3.D4E5
Switch(config)# ip arp inspection filter STATIC_HOSTS vlan 10
```

---

## 4. IP Source Guard

Previene IP Spoofing verificando la IP de origen contra la tabla de binding.

```text
Switch(config-if)# ip verify source
```

---

## 5. Cadena de Seguridad
```text
DHCP Snooping → tabla de binding → DAI (valida ARP) → IP Source Guard (valida IP) → Port Security (limita MACs)
```

---

## 6. Verificación
```text
Switch# show port-security
Switch# show port-security interface FastEthernet 0/1
Switch# show ip dhcp snooping binding
Switch# show ip arp inspection
Switch# show ip verify source
```
