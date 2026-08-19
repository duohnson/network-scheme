# Nota Avanzada - Clase 7: FHRP (First Hop Redundancy Protocols)

## Introducción
En una red típica, los hosts tienen configurado un único **Default Gateway** (puerta de enlace predeterminada). Si ese router falla, todos los dispositivos de esa red pierden conectividad hacia otras redes. Los protocolos FHRP solucionan este problema creando un **gateway virtual** compartido entre múltiples routers, proporcionando redundancia transparente para los hosts.

---

## 1. El Problema del Gateway Único

Sin redundancia:
```text
[PC1] ─── [Switch] ─── [Router A] ─── Internet
                            ↓
                     Si Router A falla,
                     PC1 pierde Internet
```

Con FHRP:
```text
                         ┌── [Router A: ACTIVO] ──┐
[PC1] ─── [Switch] ─────┤   Gateway Virtual:     ├── Internet
                         │   192.168.1.1          │
                         └── [Router B: STANDBY] ─┘
```

Los hosts configuran la **IP virtual** como su Default Gateway. Si el router activo falla, el standby asume automáticamente sin que los hosts necesiten cambiar nada.

---

## 2. Conceptos Clave

| Concepto | Descripción |
|----------|-------------|
| **IP Virtual** | Dirección IP compartida entre routers. Es el gateway de los hosts |
| **MAC Virtual** | Dirección MAC asociada a la IP virtual |
| **Router Activo/Master** | El que reenvía tráfico normalmente |
| **Router Standby/Backup** | Espera en caso de fallo del activo |
| **Preemption** | Permite que un router con mayor prioridad recupere el rol activo al volver |

---

## 3. Protocolos FHRP

### 3.1 HSRP (Hot Standby Router Protocol)
Protocolo **propietario de Cisco**. Es el más utilizado en redes Cisco.

**Características:**
- **Estados:** Active, Standby, Listen, Learn, Init
- **Prioridad por defecto:** 100 (mayor = mejor)
- **Preempt:** Debe habilitarse explícitamente
- **Mensajes Hello:** Cada 3 segundos por defecto
- **Hold time:** 10 segundos (3 hellos perdidos = fallo)
- **Multicast:** 224.0.0.2 (HSRPv1), 224.0.0.102 (HSRPv2)
- **MAC Virtual:** `0000.0C07.ACxx` (HSRPv1) donde xx = grupo

**Configuración:**
```text
! Router A (Activo)
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip address 192.168.1.2 255.255.255.0
Router(config-if)# standby 1 ip 192.168.1.1
Router(config-if)# standby 1 priority 110
Router(config-if)# standby 1 preempt
Router(config-if)# exit

! Router B (Standby)
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip address 192.168.1.3 255.255.255.0
Router(config-if)# standby 1 ip 192.168.1.1
Router(config-if)# exit
```

> **Nota:** La IP virtual (192.168.1.1) es diferente a las IPs reales de ambos routers. Los PCs usan 192.168.1.1 como gateway.

**HSRPv2 vs HSRPv1:**

| Característica | HSRPv1 | HSRPv2 |
|---------------|--------|--------|
| Grupos | 0-255 | 0-4095 |
| Multicast | 224.0.0.2 | 224.0.0.102 |
| Soporte IPv6 | No | Sí |
| Temporizadores | Segundos | Milisegundos |

### 3.2 VRRP (Virtual Router Redundancy Protocol)
Protocolo de **estándar abierto** (RFC 5798). Funcionalidad similar a HSRP.

**Diferencias con HSRP:**
- Estados: **Master** y **Backup** (en lugar de Active y Standby)
- Preempt habilitado **por defecto**
- La IP virtual **puede** ser la IP real de uno de los routers

**Configuración:**
```text
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip address 192.168.1.2 255.255.255.0
Router(config-if)# vrrp 1 ip 192.168.1.1
Router(config-if)# vrrp 1 priority 110
Router(config-if)# exit
```

### 3.3 GLBP (Gateway Load Balancing Protocol)
Protocolo **propietario de Cisco**. La gran diferencia es que permite **balanceo de carga** entre routers.

**Roles:**
- **AVG (Active Virtual Gateway):** Asigna MACs virtuales a los otros routers
- **AVF (Active Virtual Forwarder):** Cada router reenvía una porción del tráfico

**Ventaja:** En HSRP/VRRP, el router Standby/Backup no maneja tráfico; en GLBP, **todos los routers participan**.

---

## 4. Comparación de Protocolos FHRP

| Característica | HSRP | VRRP | GLBP |
|---------------|------|------|------|
| **Propietario** | Cisco | Estándar (IEEE) | Cisco |
| **Balanceo de carga** | No | No | Sí |
| **Preempt por defecto** | No | Sí | Sí |
| **Prioridad default** | 100 | 100 | 100 |
| **Routers activos** | 1 | 1 | Múltiples |
| **Soporte IPv6** | HSRPv2 | Sí | Sí |

---

## 5. Comandos de Verificación

```text
! HSRP
Router# show standby
Router# show standby brief
Router# show standby GigabitEthernet 0/0

! VRRP
Router# show vrrp
Router# show vrrp brief

! GLBP
Router# show glbp
Router# show glbp brief
```

---

## Resumen
- FHRP elimina el punto único de fallo del Default Gateway.
- **HSRP** es el más común en entornos Cisco; VRRP es la alternativa de estándar abierto.
- **GLBP** es el único que balancea carga entre routers activos.
- Los hosts no necesitan cambiar su configuración; usan la IP virtual como gateway.
- El preempt permite que el router principal recupere su rol automáticamente tras una falla temporal.
