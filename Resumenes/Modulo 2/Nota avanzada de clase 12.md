# Nota Avanzada - Clase 12: Rutas Estáticas (IPv4 e IPv6)

## Introducción
Las rutas estáticas son rutas configuradas manualmente por el administrador de red. Aunque no son escalables para redes grandes, son esenciales en escenarios específicos como rutas por defecto hacia un ISP, redes stub, y rutas de respaldo. Esta nota profundiza en los diferentes tipos de rutas estáticas, su sintaxis para IPv4 e IPv6, y la técnica de sumarización de rutas.

---

## 1. Tipos de Rutas Estáticas

### 1.1 Ruta Estática Estándar
Se utiliza para alcanzar una **red específica**. Se puede configurar de tres formas según el tipo de enlace:

**Sintaxis IPv4:**
```text
Router(config)# ip route [RED] [MÁSCARA] [INT_SALIDA | SIGUIENTE_SALTO] [AD]
```

**Sintaxis IPv6:**
```text
Router(config)# ipv6 route [RED/PREFIJO] [INT_SALIDA | SIGUIENTE_SALTO] [AD]
```

| Tipo | IPv4 Ejemplo | IPv6 Ejemplo | Uso |
|------|-------------|-------------|-----|
| **Punto a Punto** | `ip route 192.168.0.0 255.255.255.0 Ser0/0/0` | `ipv6 route 2001:DB8:ACAD:1::/64 Ser0/0/0` | Enlaces seriales directos |
| **Siguiente Salto (Multipunto)** | `ip route 192.168.0.0 255.255.255.0 10.0.0.2` | `ipv6 route 2001:DB8:ACAD:1::/64 FE80::2` | Redes Ethernet o multiaccess |
| **Completamente Especificada** | `ip route 192.168.0.0 255.255.255.0 Ser0/0/0 10.0.0.2` | `ipv6 route 2001:DB8:ACAD:1::/64 Ser0/0/0 FE80::2` | Máxima precisión (interfaz + siguiente salto) |

> **Nota sobre IPv6:** Cuando se usa una dirección **link-local** (FE80::) como siguiente salto, es **obligatorio** especificar también la interfaz de salida, ya que las direcciones link-local no son únicas globalmente.

---

### 1.2 Ruta Estática Predeterminada (Default Route)
Coincide con **todos los paquetes** que no tienen una ruta más específica en la tabla. También conocida como:
- Gateway de Último Recurso
- Quad Zero (IPv4)
- Ruta de Descarte

```text
! IPv4
Router(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/0

! IPv6
Router(config)# ipv6 route ::/0 Serial0/0/0
```

> **Caso de uso típico:** Un router de borde que envía todo el tráfico desconocido hacia el ISP.

---

### 1.3 Ruta Estática Flotante (Floating Static Route)
Es una ruta estática de **respaldo**. Se le asigna una **Distancia Administrativa (AD) mayor** a la de la ruta principal para que solo se active cuando la ruta principal falla.

```text
! Ruta principal (AD por defecto = 1)
Router(config)# ip route 192.168.0.0 255.255.255.0 Serial0/0/0

! Ruta flotante de respaldo (AD = 10)
Router(config)# ip route 192.168.0.0 255.255.255.0 Serial0/0/1 10
```

> En condiciones normales, solo la primera ruta aparece en la tabla. Si `Serial0/0/0` cae, la ruta flotante con AD=10 se instala automáticamente.

---

### 1.4 Ruta Estática hacia Host
Apunta a un **único dispositivo** usando una máscara `/32` (IPv4) o `/128` (IPv6).

```text
! IPv4 - Ruta hacia un servidor específico
Router(config)# ip route 192.168.0.10 255.255.255.255 Serial0/0/0

! IPv6 - Ruta hacia un host específico
Router(config)# ipv6 route 2001:DB8:ACAD:1::10/128 Serial0/0/0
```

> **Caso de uso:** Redirigir el tráfico hacia un servidor crítico por una ruta específica diferente a la ruta normal de la red.

---

## 2. Sumarización de Rutas (Route Summarization)

La sumarización permite condensar múltiples rutas en una sola **superred**, reduciendo el tamaño de la tabla de enrutamiento.

### 2.1 Proceso de Sumarización IPv4

**Paso 1:** Escribir las redes en binario.
**Paso 2:** Identificar los bits comunes de izquierda a derecha.
**Paso 3:** La nueva máscara comienza donde terminan los bits comunes.

#### Ejemplo: Sumarizar redes
```text
192.168.1.0/24  → 11000000.10101000.00000001.00000000
192.168.2.0/24  → 11000000.10101000.00000010.00000000
192.168.3.0/24  → 11000000.10101000.00000011.00000000
                                           ↑
                         Bits comunes hasta aquí = /22

Superred resultante: 192.168.0.0/22
Máscara:             255.255.252.0
Wildcard:            0.0.3.255
Broadcast:           192.168.3.255
```

**En lugar de tres rutas estáticas:**
```text
ip route 192.168.1.0 255.255.255.0 Ser0/0/0
ip route 192.168.2.0 255.255.255.0 Ser0/0/0
ip route 192.168.3.0 255.255.255.0 Ser0/0/0
```

**Se usa una sola ruta sumarizada:**
```text
ip route 192.168.0.0 255.255.252.0 Ser0/0/0
```

#### Ejemplo: Sumarizar subredes
```text
192.168.0.0/27   → 11000000.10101000.00000000.00000000
192.168.0.32/27  → 11000000.10101000.00000000.00100000
192.168.0.64/27  → 11000000.10101000.00000000.01000000
                                                ↑
                          Bits comunes hasta aquí = /25

Subred sumarizada: 192.168.0.0/25
Máscara:           255.255.255.128
Wildcard:          0.0.0.127
Broadcast:         192.168.0.127
```

---

### 2.2 Proceso de Sumarización IPv6

El proceso es idéntico, pero trabajando con hexadecimal y nibbles (grupos de 4 bits).

#### Ejemplo 1: Sumarizar /64
```text
2001:DB8:ACAD:1::/64  → :0001:  → 0000 0000 0000 0001
2001:DB8:ACAD:2::/64
2001:DB8:ACAD:3::/64
2001:DB8:ACAD:4::/64
2001:DB8:ACAD:5::/64
2001:DB8:ACAD:6::/64
2001:DB8:ACAD:7::/64  → :0007:  → 0000 0000 0000 0111
                                              ↑
                         Bits comunes hasta aquí = /61

Superred: 2001:DB8:ACAD::/61
```

#### Ejemplo 2: Sumarizar /48
```text
2001:DB8:CAFE::/48
2001:DB8:CAFF::/48
2001:DB8:CFFE::/48
2001:DB8:CACA::/48  → :CAAA:  → 1100 1010 1010 1010
2001:DB8:CAEE::/48
2001:DB8:CAAA::/48  → :CFFE:  → 1100 1111 1111 1110
                                  ↑
                 Bits comunes: 1100 1000 0000 0000 = /37

Superred: 2001:DB8:C800::/37
```

---

## 3. Tabla Resumen de Tipos de Rutas Estáticas

| Tipo de Ruta | Propósito | IPv4 | IPv6 |
|-------------|-----------|------|------|
| **Estándar** | Alcanzar una red específica | `ip route RED MASK SALIDA` | `ipv6 route RED/PREFIX SALIDA` |
| **Predeterminada** | Ruta para todo lo desconocido | `ip route 0.0.0.0 0.0.0.0 SALIDA` | `ipv6 route ::/0 SALIDA` |
| **Flotante** | Respaldo con AD mayor | `ip route RED MASK SALIDA AD` | `ipv6 route RED/PREFIX SALIDA AD` |
| **Hacia Host** | Un único dispositivo | `ip route HOST 255.255.255.255 SALIDA` | `ipv6 route HOST/128 SALIDA` |
| **Sumarizada** | Condensar múltiples redes | Calcular superred y aplicar | Calcular prefijo común |

---

## 4. Verificación de Rutas Estáticas

```text
Router# show ip route               ! Muestra toda la tabla de enrutamiento IPv4
Router# show ip route static        ! Filtra solo las rutas estáticas
Router# show ipv6 route             ! Muestra la tabla de enrutamiento IPv6
Router# show ipv6 route static      ! Filtra solo las rutas estáticas IPv6
Router# show running-config | include ip route   ! Muestra las rutas en la configuración
```

---

## Resumen
- Las rutas estáticas se configuran manualmente y son ideales para redes pequeñas o escenarios específicos.
- Existen cuatro tipos principales: estándar, predeterminada, flotante y hacia host.
- La **sumarización** reduce la tabla de enrutamiento agrupando múltiples redes en una superred.
- En IPv6, cuando se usa una dirección link-local como siguiente salto, se debe especificar obligatoriamente la interfaz de salida.
- La ruta flotante es una estrategia de redundancia económica que usa la Distancia Administrativa para priorizar rutas.
