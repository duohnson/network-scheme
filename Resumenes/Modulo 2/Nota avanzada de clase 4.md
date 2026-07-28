# Nota Avanzada - Clase 4: Spanning Tree Protocol (STP)

## Introducción
La redundancia en redes conmutadas es necesaria para evitar puntos únicos de falla, pero físicamente crea **bucles de Capa 2**. Spanning Tree Protocol (STP) bloquea lógicamente puertos específicos para prevenir estos bucles, manteniendo la redundancia en caso de fallos.

## 1. Problemas de los Bucles de Capa 2
Sin STP, un bucle causa:
- **Tormentas de Broadcast:** Las tramas de broadcast se reenvían infinitamente hasta colapsar la red.
- **Inestabilidad de la Tabla MAC:** Los switches ven la misma dirección MAC desde diferentes puertos, saturando los recursos.
- **Transmisión múltiple de tramas:** Los dispositivos finales reciben copias duplicadas de la misma información.

## 2. Funcionamiento de STP (IEEE 802.1D)
STP construye un árbol lógico sin bucles.

### 2.1 Elección del Puente Raíz (Root Bridge)
1. Los switches envían BPDUs (Bridge Protocol Data Units).
2. Se elige un único **Puente Raíz** para toda la red basándose en el **Bridge ID (BID)** más bajo.
   - `BID = Prioridad (por defecto 32768) + Dirección MAC`.
3. Para forzar un switch como raíz, se modifica su prioridad: `spanning-tree vlan 1 root primary`.

### 2.2 Roles de Puertos
- **Root Port (Puerto Raíz):** Puerto con el camino más corto hacia el Root Bridge (uno por cada switch no raíz).
- **Designated Port (Puerto Designado):** Puerto que envía tráfico en un segmento de red.
- **Alternate/Blocked Port (Puerto Bloqueado):** Puerto que se deshabilita lógicamente para romper el bucle.

## 3. Variantes de Spanning Tree
- **STP Original (802.1D):** Lento para converger (30-50 segundos).
- **PVST+ (Per-VLAN Spanning Tree Plus):** Variante de Cisco. Crea una instancia STP independiente por cada VLAN, permitiendo balanceo de carga.
- **RSTP (Rapid STP - 802.1w):** Convergencia muy rápida. Los puertos asumen roles casi inmediatamente.
- **Rapid PVST+:** Combinación de Cisco de RSTP con instancias por VLAN. (Recomendado).

## 4. PortFast y BPDU Guard
Para puertos conectados a hosts (PCs, impresoras):
- **PortFast:** Pasa el puerto directamente al estado de reenvío, saltándose las fases de escucha y aprendizaje de STP.
- **BPDU Guard:** Apaga (err-disable) un puerto PortFast si recibe una BPDU, previniendo que alguien conecte un switch no autorizado.

```text
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable
```
