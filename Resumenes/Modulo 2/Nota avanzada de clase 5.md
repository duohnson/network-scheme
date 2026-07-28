# Nota Avanzada - Clase 5: EtherChannel

## Introducción
EtherChannel es una tecnología de agregación de enlaces que permite combinar múltiples enlaces físicos (hasta 8) entre switches en un único enlace lógico.

## 1. Ventajas de EtherChannel
- **Incremento de ancho de banda:** Si combinas cuatro puertos de 1 Gbps, obtienes un enlace de 4 Gbps.
- **Balanceo de carga:** El tráfico se distribuye entre los enlaces físicos.
- **Redundancia:** Si un enlace falla, el tráfico se redirige instantáneamente a los enlaces restantes.
- **Visión STP:** Spanning Tree ve el EtherChannel como un solo puerto, evitando que bloquee los enlaces individuales y maximizando el ancho de banda.

## 2. Protocolos de Negociación

### 2.1 PAgP (Port Aggregation Protocol)
Protocolo propietario de Cisco.
- **Modos:** `desirable` (inicia la negociación activamente) y `auto` (espera a que el otro lado inicie).
- Para formar un canal: `desirable - desirable` o `desirable - auto`.

### 2.2 LACP (Link Aggregation Control Protocol)
Estándar IEEE 802.3ad. Compatible con equipos que no son Cisco.
- **Modos:** `active` (inicia activamente) y `passive` (espera a que el otro lado inicie).
- Para formar un canal: `active - active` o `active - passive`.

### 2.3 Modo "On" (Estático)
Fuerza la creación del canal sin protocolo de negociación. Ambos extremos deben estar en modo `on`. No recomendado porque no detecta errores de configuración.

## 3. Configuración

**Ejemplo de LACP (Capa 2):**
```text
Switch(config)# interface range gigabitEthernet 0/1-2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit
Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
```

## 4. Requisitos para formar un EtherChannel
Todos los puertos físicos involucrados deben tener configuraciones idénticas:
- Misma velocidad y dúplex.
- Mismo estado de puerto (modo acceso o troncal).
- Si es troncal: misma VLAN nativa y rango de VLANs permitidas.
