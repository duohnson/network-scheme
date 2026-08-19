# Nota Avanzada - Clase 11: Conceptos de Enrutamiento

## Introducción
El enrutamiento es el proceso mediante el cual un router determina la mejor ruta para enviar un paquete desde su origen hasta su destino a través de múltiples redes interconectadas. Comprender cómo funcionan las rutas estáticas, los protocolos de enrutamiento dinámico y la tabla de enrutamiento es fundamental para cualquier administrador de red.

---

## 1. Rutas Estáticas vs Protocolos de Enrutamiento Dinámico

### 1.1 Rutas Estáticas
Son rutas configuradas manualmente por el administrador. No envían actualizaciones de enrutamiento a otros routers.

| Aspecto | Detalle |
|---------|---------|
| **Ventajas** | Mayor seguridad (no se comparten rutas), menor consumo de recursos (CPU, RAM, ancho de banda) |
| **Desventajas** | Mayor carga administrativa, no escalan bien en redes grandes |
| **Uso ideal** | Redes pequeñas, rutas por defecto hacia un ISP, rutas de respaldo (flotantes) |

**Configuración básica:**
```text
Router(config)# ip route 192.168.3.0 255.255.255.0 Serial0/0/0        ! Punto a punto
Router(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.2        ! Siguiente salto (multipunto)
```

**Ruta predeterminada (Default Route / Gateway of Last Resort):**
```text
Router(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/0
```

### 1.2 Protocolos de Enrutamiento Dinámico
Los routers intercambian información de enrutamiento automáticamente. Descubren redes remotas y se adaptan a cambios en la topología.

| Aspecto | Detalle |
|---------|---------|
| **Ventajas** | Menor carga administrativa, escalables, se adaptan a fallos automáticamente |
| **Desventajas** | Menos seguros (comparten información de red), consumen más recursos |

---

## 2. Tabla de Enrutamiento

La tabla de enrutamiento contiene todas las rutas conocidas por el router. Para protocolos de enrutamiento dinámico, cada entrada muestra **7 elementos clave**:

| # | Elemento | Descripción | Ejemplo |
|---|----------|-------------|---------|
| 1 | **Letra/Código** | Identifica el protocolo o tipo de ruta | `C` (Conectada), `S` (Estática), `O` (OSPF), `R` (RIP), `D` (EIGRP) |
| 2 | **Red de Destino** | La red a la que se quiere llegar | `172.16.0.0/24` |
| 3 | **Distancia Administrativa (AD)** | Prioridad del protocolo (menor = más confiable) | OSPF = 110 |
| 4 | **Métrica** | Costo de la ruta según el protocolo | OSPF = costo, RIP = saltos |
| 5 | **Siguiente Salto** | IP del router vecino al que se envía el paquete | `10.0.0.2` |
| 6 | **Temporizador** | Tiempo desde que se aprendió la ruta | `00:01:30` |
| 7 | **Interfaz de Salida** | Puerto físico por donde se envía el paquete | `Serial0/0/0` |

### 2.1 Distancias Administrativas Comunes

| Protocolo | AD | Métrica | Algoritmo |
|-----------|-----|---------|-----------|
| **Conectada** | 0 | N/A | N/A |
| **Estática** | 1 | N/A | N/A |
| **EIGRP** | 90 | Ancho de banda + Delay (compuesto) | DUAL |
| **OSPF** | 110 | Costo (10⁸ / Ancho de banda) | Dijkstra (SPF) |
| **RIP** | 120 | Conteo de Saltos (máx. 15) | Bellman-Ford |

> **Regla de oro:** Si un router conoce la misma red por múltiples fuentes, gana la que tenga la **menor Distancia Administrativa**. Si la AD es igual, se comparan las **métricas**.

### 2.2 Balanceo de Carga
Se produce cuando dos o más rutas hacia el mismo destino tienen **la misma métrica**. El router distribuye el tráfico entre ambas rutas equitativamente.

---

## 3. Clasificación de los Protocolos de Enrutamiento

### 3.1 IGP (Interior Gateway Protocol)
Operan **dentro de un mismo Sistema Autónomo (AS)**.

#### Vector Distancia (Distance Vector)
- Más lentos pero consumen menos recursos.
- Envían su tabla de enrutamiento completa a los vecinos periódicamente.

| Protocolo | Classful/Classless | IPv4 | IPv6 |
|-----------|-------------------|------|------|
| RIPv1 | Classful | ✅ | ❌ |
| RIPv2 | Classless | ✅ | ❌ |
| RIPng | Classless | ❌ | ✅ |
| IGRP | Classful | ✅ | ❌ |
| EIGRP | Classless | ✅ | ✅ (EIGRP para IPv6) |

#### Estado de Enlace (Link-State)
- Más rápidos pero consumen más recursos.
- Cada router construye un mapa completo de la topología de la red.

| Protocolo | IPv4 | IPv6 |
|-----------|------|------|
| OSPFv2 | ✅ | ❌ |
| OSPFv3 | ❌ | ✅ |
| IS-IS | ✅ | ✅ |

### 3.2 EGP (Exterior Gateway Protocol)
Opera **entre Sistemas Autónomos diferentes** (por ejemplo, entre ISPs).

| Protocolo | Tipo | Uso |
|-----------|------|-----|
| **BGP-4** | Vector Ruta (Path Vector) | IPv4 entre AS |
| **BGP-MP** | Vector Ruta (Path Vector) | IPv6 entre AS (Multiprotocol) |

> **BGP** es el protocolo que sostiene Internet. Cada ISP es un Sistema Autónomo y BGP intercambia rutas entre ellos.

---

## 4. Componentes de un Protocolo de Enrutamiento

Todo protocolo de enrutamiento dinámico tiene tres componentes fundamentales:

1. **Estructura de Datos:** ¿Qué información voy a publicar? (tablas, bases de datos de topología)
2. **Mensajes del Protocolo:** ¿Cómo lo voy a publicar? (tipo de paquetes que intercambian los routers)
3. **Algoritmo:** El corazón del protocolo. Determina las mejores rutas basándose en las menores métricas.

---

## 5. Configuración de Protocolos de Enrutamiento

### 5.1 RIP (Routing Information Protocol)
```text
Router(config)# router rip
Router(config-router)# network 192.168.0.0
Router(config-router)# network 10.0.0.0
Router(config-router)# version 2
Router(config-router)# passive-interface FastEthernet0/0
Router(config-router)# no auto-summary
Router(config-router)# exit
```

### 5.2 EIGRP (Enhanced Interior Gateway Routing Protocol)
```text
Router(config)# router eigrp 1
Router(config-router)# network 192.168.0.0 255.255.255.0
Router(config-router)# network 10.0.0.0 255.255.255.252
Router(config-router)# passive-interface FastEthernet0/0
Router(config-router)# no auto-summary
Router(config-router)# exit
```
> **Nota:** EIGRP usa **máscara de subred** (no wildcard). El número de AS (1 en el ejemplo) debe coincidir en todos los routers.

### 5.3 OSPF (Open Shortest Path First)
```text
Router(config)# router ospf 1
Router(config-router)# network 192.168.0.0 0.0.0.255 area 0
Router(config-router)# network 10.0.0.0 0.0.0.3 area 0
Router(config-router)# passive-interface FastEthernet0/0
Router(config-router)# exit
```
> **Nota:** OSPF usa **wildcard mask** (inversa de la máscara de subred). El número de proceso (1) es local al router.

---

## 6. Clases de Direccionamiento IP

| Clase | Primer Octeto (binario) | Prefijo por defecto | Rango |
|-------|------------------------|---------------------|-------|
| A | `0xxxxxxx` | /8 | 0.0.0.0 – 127.255.255.255 |
| B | `10xxxxxx` | /16 | 128.0.0.0 – 191.255.255.255 |
| C | `110xxxxx` | /24 | 192.0.0.0 – 223.255.255.255 |
| D (Multicast) | `1110xxxx` | N/A | 224.0.0.0 – 239.255.255.255 |
| E (Experimental) | `1111xxxx` | N/A | 240.0.0.0 – 255.255.255.255 |

### 6.1 Terminología del Direccionamiento

- **Red:** La máscara es igual a la clase por defecto (ej: `192.168.0.0/24` = red Clase C)
- **Subred:** La máscara es **mayor** a la clase (ej: `192.168.0.0/25` = subred de una Clase C)
- **Superred (Sumarizada):** La máscara es **menor** a la clase (ej: `192.168.0.0/22` = superred que agrupa varias redes Clase C)

---

## 7. Niveles de Rutas en la Tabla de Enrutamiento

### Rutas de Nivel 1
- **Red:** Ruta con máscara igual a la classful.
- **Superred:** Ruta sumarizada con máscara menor a la classful.
- **Predeterminada:** Ruta `0.0.0.0/0`.

Se subdividen en:
- **Ruta Final de Nivel 1:** Tiene interfaz de salida o siguiente salto (puede reenviar tráfico directamente).
- **Ruta Principal de Nivel 1:** Es una ruta padre de la que se derivan subredes (Nivel 2).

### Rutas de Nivel 2
- Son **subredes** derivadas de una ruta principal de Nivel 1.
- **Todas son rutas finales** (siempre tienen interfaz de salida o siguiente salto).

---

## 8. Comando `passive-interface`
El comando `passive-interface` evita que un router envíe actualizaciones de enrutamiento por una interfaz específica (normalmente la que conecta a la LAN con dispositivos finales). Esto:
- **Mejora la seguridad:** No expone información de la topología a la red de usuarios.
- **Reduce el tráfico:** Evita enviar actualizaciones innecesarias.

```text
Router(config-router)# passive-interface FastEthernet0/0
```

---

## Resumen
- Las rutas estáticas son más seguras pero no escalan; los protocolos dinámicos escalan pero consumen más recursos.
- La tabla de enrutamiento se construye con rutas conectadas, estáticas y dinámicas.
- La **Distancia Administrativa** determina qué fuente es más confiable; la **métrica** determina la mejor ruta dentro de un mismo protocolo.
- OSPF y EIGRP son los protocolos IGP más utilizados en redes empresariales.
- BGP es el protocolo que interconecta los Sistemas Autónomos de Internet.
