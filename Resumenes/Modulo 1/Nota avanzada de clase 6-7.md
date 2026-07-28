# Nota Avanzada - Capítulos 6-7: Capa Física y Medios de Transmisión

## Introducción

Los capítulos 6-7 exploran la capa física y los medios por los cuales viajan nuestros datos a través de las redes. Es fundamental entender cómo el software se convierte en señales físicas y cómo diferentes medios manejan estas transmisiones.

---

## 1. Capa Física (OSI Capa 1)

### 1.1 Funciones de la Capa Física

```
┌──────────────────────────────────────────────┐
│        Capa Física (Capa 1)                  │
├──────────────────────────────────────────────┤
│ • Transmisión y recepción de bits            │
│ • Definición de medios (cable, fibra, aire)  │
│ • Definición de voltajes y señales           │
│ • Definición de conectores                   │
│ • Definición de velocidades                  │
│ • Control de lineas de comunicación          │
└──────────────────────────────────────────────┘
```

### 1.2 PDU en Capa Física

- **PDU**: Bits (01010101...)
- **Forma**: Señales eléctricas, pulsos de luz, ondas electromagnéticas
- **Velocidad**: Medida en baudios (símbolos por segundo) o bps (bits por segundo)

---

## 2. Medios de Transmisión

### 2.1 Cobre (Twisted Pair)

#### Características

```
┌─────────────────┐
│  Twisted Pair   │
├─────────────────┤
│ Pares de hilos  │
│ retorcidos      │
│                 │
│ ════════════    │
│ ════════════    │
│ ════════════    │
│ ════════════    │
│ (8 hilos total) │
└─────────────────┘
```

**Ventajas**:
- Económico
- Fácil de instalar
- Flexible
- Ampliamente disponible

**Desventajas**:
- Susceptible a interferencia electromagnética (EMI)
- Distancia limitada (max 100 metros)
- Velocidad limitada

#### Categorías de Cobre

| Categoría | Velocidad | Rango | Uso |
|---|---|---|---|
| Cat5 | 100 Mbps | 100 m | Obsoleto |
| Cat5e | 1 Gbps | 100 m | Hogares/PYMES |
| Cat6 | 10 Gbps | 55 m | Centros de datos |
| Cat6a | 10 Gbps | 100 m | Empresas modernas |
| Cat7 | 10 Gbps+ | 100 m | Data centers |
| Cat8 | 40 Gbps | 30 m | Ultra-rápido |

#### Tipos de Cables de Cobre

**Straight-Through (Directo)**
```
Ambos extremos = 1,2,3,4,5,6,7,8
┌─────────┬─────────┐
│ A ⬚    │ A ⬚    │
│ B     │ B     │
│ C     │ C     │
│ D      │ D      │
└─────────┴─────────┘

Uso: PC  Switch, Server  Switch
```

**Cross-Over**
```
Un extremo = A, otro extremo = B
┌─────────┬─────────┐
│ A ⬚    │ B ⬚    │
│ B     │ A     │
│ C     │ C     │
│ D      │ D      │
└─────────┴─────────┘

Uso: PC  PC, Switch  Switch, Router  Router
```

**MDIX (Auto-MDI/MDIX)**
- Algunos equipos autodetectan el tipo de cable necesario
- Pueden usar directo o cross-over automáticamente
- Muy útil en equipamiento moderno

### 2.2 Fibra Óptica

#### Estructura

```
┌─────────────────────────────────┐
│  Estructura de Fibra Óptica     │
├─────────────────────────────────┤
│ Cubierta protectora (plástico) │
│ ↓                               │
│ Revestimiento (cladding)        │
│ ↓                               │
│ Núcleo (core) ← Luz viaja aquí  │
│                                 │
│ Diámetro total: 125 μm          │
└─────────────────────────────────┘
```

#### Modos de Fibra

**Monomodo**
```
┌───────────────────────┐
│ Monomodo (Single-mode)│
├───────────────────────┤
│ Un rayo de luz        │
│ │                     │
│ │                     │
│ │ → Rayo directo      │
│                       │
│ • Núcleo pequeño: 8μm │
│ • Mayor distancia     │
│ • Más costoso         │
│ • Uso: Backbone WAN   │
│ • Velocidad: Luz LÁSER│
└───────────────────────┘
```

**Multimodo**
```
┌───────────────────────┐
│ Multimodo (Multi-mode)│
├───────────────────────┤
│ Múltiples rayos       │
│ ╱ ╲ ╱ ╲ ╱ ╲            │
│ ╱ ╲ ╱ ╲ ╱ ╲            │
│ ╱ ╲ ╱ ╲ ╱ ╲            │
│ → Rayos rebotando    │
│                       │
│ • Núcleo grande: 50μm │
│ • Menor distancia     │
│ • Más económico       │
│ • Uso: LAN/Campus     │
│ • Velocidad: LED      │
└───────────────────────┘
```

#### Ventajas de Fibra Óptica

- **Distancia**: Hasta 2 km (monomodo), 500 m (multimodo)
- **Velocidad**: No limitada
- **Inmunidad**: Totalmente inmune a EMI
- **Seguridad**: Muy difícil de interceptar
- **Futuro**: Escalable a velocidades extremas

#### Desventajas

- Muy costosa
- Difícil de instalar
- Requiere equipamiento especializado
- Frágil

#### Conectores de Fibra

| Tipo | Tamaño | Uso |
|---|---|---|
| SC | Medio | Telecomunicaciones |
| LC | Pequeño | Moderno, data centers |
| ST | Grande | Antiguo |
| MTP/MPO | Multi-fibra | Cables de varios hilos |

### 2.3 Inalámbrico

#### Bandas de Frecuencia

```
Espectro de Frecuencias Inalámbricas:

2.4 GHz    ← WiFi, Bluetooth, microondas
           ├─ 802.11b/g/n/ax
           ├─ Alcance: ~100 metros
           ├─ Menos costoso
           ├─ Más interferencia
           └─ 13 canales en la mayoría de países

5 GHz      ← WiFi moderno, WiFi 6/6E
           ├─ 802.11a/n/ac/ax
           ├─ Alcance: ~50 metros
           ├─ Menos interferencia
           ├─ Mayor velocidad
           └─ Muchos más canales

60 GHz     ← WiFi 6E
           ├─ 802.11ay
           ├─ Velocidad: Extremadamente alta
           ├─ Alcance: Muy corto
           └─ Penetra poco

Comparación:

2.4 GHz vs 5 GHz
┌─────────────┬──────────┬──────────┐
│Característica │ 2.4 GHz  │ 5 GHz    │
├─────────────┼──────────┼──────────┤
│ Velocidad   │ 150 Mbps │ 1.3 Gbps │
│ Alcance     │ 100 m    │ 50 m     │
│ Penetración │ Buena    │ Pobre    │
│ Canales     │ 13       │ 25+      │
│ Interferencia│ Alta     │ Baja     │
└─────────────┴──────────┴──────────┘
```

#### Estándares WiFi

| Estándar | Año | Banda | Velocidad | Características |
|---|---|---|---|---|
| 802.11b | 1999 | 2.4 GHz | 11 Mbps | Pionero |
| 802.11g | 2003 | 2.4 GHz | 54 Mbps | Compatible con 11b |
| 802.11n | 2009 | 2.4/5 GHz | 600 Mbps | MIMO |
| 802.11ac | 2013 | 5 GHz | 3.5 Gbps | WiFi 5 |
| 802.11ax | 2019 | 2.4/5/6 GHz | 11+ Gbps | WiFi 6 |
| 802.11be | 2024 | 2.4/5/6 GHz | 30+ Gbps | WiFi 7 |

#### Limitaciones Inalámbricas

```
Problema 1: Área de Cobertura Limitada
┌────────────────────────┐
│       Router WiFi      │
│         ↓ 50-100m      │
│      ╱ ╲ ╱ ╲ ╱ ╲       │
│   (alcance del router) │
│                        │
│ Pared de hormigón      │
│ Piso de metal          │
│ Espejos                │
│ Agua (muy absorbente)  │
│ → Reducen alcance 50%+ │
└────────────────────────┘

Problema 2: Interferencia
┌───────────────────────┐
│ Fuentes de Interferencia:
│ • Microondas           │
│ • Teléfonos inalámbricos
│ • Otros routers WiFi   │
│ • Dispositivos Bluetooth
│ • Monitores inalámbricos
│ • Câmaras inalámbricas │
│ • Sistemas de videovigilancia
└───────────────────────┘

Problema 3: Seguridad
┌──────────────────────────┐
│ WiFi abierto = Vulnerable
│ • Todos ven el tráfico   │
│ • Suplantación fácil     │
│ • Solución: Cifrado WPA2/3
│   └─ Requiere contraseña │
└──────────────────────────┘

Problema 4: Medio Compartido
┌───────────────────────┐
│ Todos comparten el    │
│ mismo medio de aire   │
│ • Colisiones posibles │
│ • Ancho de banda      │
│   compartido entre    │
│   múltiples usuarios  │
└───────────────────────┘
```

#### Tecnologías Inalámbricas

```
Rango de frecuencias inalámbricas modernos:

Nombre            Estándar    Frecuencia    Usos
─────────────────────────────────────────────────
Bluetooth         802.15.1    2.4 GHz       Auriculares, mouses
BLE               802.15.4    2.4 GHz       IoT, wearables
WiFi (2.4)        802.11b/g   2.4 GHz       Conectividad
WiFi (5)          802.11ac    5 GHz         Conectividad rápida
WiFi (6)          802.11ax    2.4/5/6 GHz   Ultra-rápido
WiMAX             802.16      2-6 GHz       Último km ISP
5G                3GPP        700MHz-28GHz  Telefonía móvil
```

### 2.4 Cable Telefónico

- Transmisión de voz
- Muy limitado para datos
- Considerado obsoleto para redes
- Velocidad: kbps (kilobits por segundo)

### 2.5 Coaxial

- Uso histórico: Topologías antiguas (bus)
- Hoy en día: Cable TV, CATV
- 10Base5/10Base2 (Ethernet histórico)
- No usado en redes modernas

---

## 3. Codificación de Tramas en Capa Física

### 3.1 ¿Por qué es necesaria la codificación?

```
Capa 2 (Enlace de Datos):
"Tengo una trama con bits: 01010101"

Capa 1 (Física):
"¿Cómo transmito esto por el cable?"
Respuesta: CODIFICACIÓN
```

### 3.2 Métodos de Codificación

**Manchester Encoding (histórico)**
```
Bit 0 → Transición alta-baja (H→B)
Bit 1 → Transición baja-alta (B→H)

Ejemplo: 10110
┌─┐   ┌─┐     ┌─┐
│ └─┐ │ └─ ┌─┐│ │
└───┘ └────┘ └─┘

Ventajas: Sincronización integrada
Desventajas: Requiere mayor ancho de banda
```

**4B5B Encoding (Fast Ethernet)**
```
Agrupa 4 bits de datos en código de 5 bits
0101 → 01101

Ventajas: Uso eficiente de ancho de banda
Desventajas: Más complejo
```

**8B10B Encoding (Gigabit Ethernet)**
```
Cada 8 bits de datos → Código de 10 bits
Mucha redundancia para sincronización y detección de errores

Ventajas: Muy robusto
Desventajas: 25% overhead
```

### 3.3 Modulación (Inalámbrico)

```
Onda Portadora (sin datos):
    ╱╲    ╱╲    ╱╲
   ╱  ╲  ╱  ╲  ╱  ╲
  ╱    ╲╱    ╲╱    ╲

Modulación de Amplitud (AM):
Datos: 01101
     ╱╲╱  ╱╲ ╱  ╱╲
    ╱  ╲╱  ╲╱  ╱  ╲

Modulación de Frecuencia (FM):
   (frecuencia varía)

Modulación de Fase (PM):
   (fase cambia)

Técnicas Modernas:
• QPSK (4 estados)
• 16-QAM (16 estados)
• 256-QAM (256 estados)
• MU-MIMO (múltiples usuarios)
```

---

## 4. Conectores y Interfaces

### 4.1 Conectores de Cobre (RJ-45)

```
┌─────────────────────────┐
│ Conector RJ-45          │
├─────────────────────────┤
│  1. Blanco/Naranja      │
│  2. Naranja             │
│  3. Blanco/Verde        │
│  4. Azul                │
│  5. Blanco/Azul         │
│  6. Verde               │
│  7. Blanco/Marrón       │
│  8. Marrón              │
│                         │
│  Total: 8 pines         │
│  Datos: pins 1,2,3,6    │
│  Poder: pins 4,5,7,8    │
│  (PoE - Power over Ethernet)
└─────────────────────────┘
```

**PoE (Power over Ethernet)**
- Transmite poder + datos en el mismo cable
- Estándares: 802.3af (15W), 802.3at (30W), 802.3bt (100W)
- Uso: Teléfonos IP, cámaras IP, puntos de acceso WiFi
- Ventaja: Una sola instalación de cable

### 4.2 Interfaces Seriales

```
Console Cable (Rollover)
┌──────────────────────┐
│ Conexión de consola: │
├──────────────────────┤
│ De: PC USB/Serial    │
│ A: Router/Switch     │
│ Puerto: Console      │
│ Velocidad: 9600 bps  │
│ Propósito:           │
│ • Configuración      │
│ • Recuperación       │
│ • Troubleshooting    │
└──────────────────────┘
```

### 4.3 Interfaz de Red (NIC)

```
Tarjeta de Red (Network Interface Card)
┌──────────────────────────┐
│ Componentes:             │
├──────────────────────────┤
│ • Buffer de memoria      │
│ • Circuitos de envío     │
│ • Circuitos de recepción │
│ • Clock (reloj)          │
│ • ROM (dirección MAC)    │
│ • Connector (RJ-45)      │
│ • LED (indicadores)      │
└──────────────────────────┘

LEDs comunes:
 Link LED    = Conexión activa
 Activity LED = Datos enviándose
 Speed LED   = Velocidad de conexión
```

---

## 5. Velocidades de Transmisión

### 5.1 Clasificación de Tecnologías

```
Históricamente (Ethernet):

10 Mbps    ← 10Base-T (Twisted Pair)
           ← Cat3, velocidad lenta

100 Mbps   ← Fast Ethernet (100Base-TX)
           ← Cat5, velocidad estándar años 2000

1 Gbps     ← Gigabit Ethernet (1000Base-T)
           ← Cat5e/Cat6, velocidad moderna

10 Gbps    ← 10 Gigabit Ethernet
           ← Cat6a, ultra-moderno

100 Gbps   ← 100 Gigabit Ethernet
           ← Cat8, data centers futuros
```

### 5.2 Relación entre velocidad y medio

```
Cobre (Twisted Pair)
- 10 m @ 10 Gbps (distancia corta)
- 55 m @ 10 Gbps (Cat6)
- 100 m @ 1 Gbps (límite OSI)
- 100 m @ 100 Mbps

Fibra Óptica
- 100 m @ 100 Mbps (multimodo)
- 2 km @ Mbps (monomodo)
- Escalable: 400 Gbps+ teóricamente
```

---

## 6. Control de Línea (Capa Física)

### 6.1 Señalización en Cobre

```
Señal digital:
V +5V ┌─┐     ┌─┐
     │ │     │ │
   0V├─┴─────┴─┴─
     │
     ├─ Bit 1 (5V)
     └─ Bit 0 (0V)

Ruido en la línea:
V +5V ┌─┐∿∿∿∿∿∿∿∿┌─┐
     │ │∿∿∿∿∿∿∿∿│ │
   0V├─┴─∿∿∿∿∿∿┴─┴─
      Interferencia electromagnética

Solución:
- Twisted Pair (reduce interferencia)
- Shielding (blindaje)
- Ground (puesta a tierra)
```

### 6.2 Sincronización de Reloj

```
Transmisor → Clock (reloj)
              ↓
         Define velocidad de bits

Receptor debe sincronizarse con reloj del transmisor
            ↓
    Captura bits en momentos correctos

Codificación ayuda con sincronización:
- Transiciones frecuentes = fácil sincronización
- Línea plana = pérdida de sincronización
```

---

## 7. Relación entre Capas 1 y 2

### 7.1 Cómo Trabajan Juntas

```
┌──────────────────────────────────────┐
│ Capa 2: Enlace de Datos              │
├──────────────────────────────────────┤
│ Genera tramas:                       │
│ [MAC Dest][MAC Origen][Datos][CRC]  │
│                 ↓                    │
│ "Aquí hay una trama lista"          │
└──────────────────────────────────────┘
         ↓ (en la interfaz)
┌──────────────────────────────────────┐
│ Capa 1: Física                       │
├──────────────────────────────────────┤
│ Toma bits de la trama                │
│ Codifica los bits                    │
│ Convierte a señales (voltaje, luz)   │
│ Las transmite por el medio           │
│                 ↓                    │
│ [Bits en voltaje/luz/radio]          │
└──────────────────────────────────────┘
         ↓ (por el cable/aire)
    [Transmisión Física]
         ↓
┌──────────────────────────────────────┐
│ Receptor Capa 1: Física              │
├──────────────────────────────────────┤
│ Recibe señales (voltaje, luz)        │
│ Decodifica a bits                    │
│ Envía bits a Capa 2                  │
└──────────────────────────────────────┘
         ↓
┌──────────────────────────────────────┐
│ Receptor Capa 2: Enlace              │
├──────────────────────────────────────┤
│ Verifica CRC                         │
│ Comprueba MAC destino                │
│ Procesa la trama                     │
│ Envía a Capa 3 si es para nosotros   │
└──────────────────────────────────────┘
```

---

## 8. Resumen de Capítulos 6-7

### Conceptos Clave

1. **Capa Física** convierte bits en señales transmisibles
2. **Cobre**: Económico, limitado en distancia, susceptible a interferencia
3. **Fibra**: Rápido, alcance extendido, costoso
4. **Inalámbrico**: Flexible, corta distancia, interferencia posible
5. **Codificación**: Manchester, 4B5B, 8B10B para sincronización
6. **Conectores**: RJ-45 para cobre, LC/SC para fibra
7. **PoE**: Poder + datos en un cable
8. **Velocidades**: 10 Mbps → 100 Gbps+ con tecnología correcta

### Decisiones de Diseño

```
¿Cobre o Fibra?
├─ Cobre si: Corta distancia, presupuesto limitado
└─ Fibra si: Larga distancia, velocidad, futuro-proof

¿WiFi o Ethernet?
├─ WiFi si: Movilidad, flexibilidad, instalación
└─ Ethernet si: Velocidad, confiabilidad, distancia

¿Qué categoría de cobre?
├─ Cat5e: Hogares, PYMES (1 Gbps)
├─ Cat6: Oficinas (10 Gbps a 55 m)
└─ Cat6a: Data centers (10 Gbps a 100 m)
```

---

## Referencias

- IEEE 802.3 (Ethernet Physical Layer)
- IEEE 802.11 (WLAN)
- IEEE 802.1Q (VLAN)
- TIA/EIA 568B (Cable Standards)
