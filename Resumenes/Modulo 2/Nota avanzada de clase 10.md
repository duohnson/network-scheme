# Nota Avanzada - Clase 10: WLAN (Wireless LAN)

## Introducción
Las redes inalámbricas (WLAN) permiten conectividad sin cables usando ondas de radio. El estándar IEEE 802.11 define las especificaciones de WiFi. Esta nota cubre los conceptos, estándares, seguridad y configuración básica de WLANs.

---

## 1. Componentes de una WLAN

| Componente | Descripción |
|-----------|-------------|
| **AP (Access Point)** | Dispositivo que conecta clientes inalámbricos a la red cableada |
| **WLC (Wireless LAN Controller)** | Controlador centralizado que gestiona múltiples APs |
| **STA (Station)** | Cliente inalámbrico (laptop, teléfono, tablet) |
| **SSID** | Nombre de la red inalámbrica visible para los clientes |

---

## 2. Estándares IEEE 802.11

| Estándar | Nombre WiFi | Frecuencia | Velocidad Máx | Año |
|----------|-------------|-----------|---------------|-----|
| 802.11a | — | 5 GHz | 54 Mbps | 1999 |
| 802.11b | — | 2.4 GHz | 11 Mbps | 1999 |
| 802.11g | — | 2.4 GHz | 54 Mbps | 2003 |
| 802.11n | **WiFi 4** | 2.4/5 GHz | 600 Mbps | 2009 |
| 802.11ac | **WiFi 5** | 5 GHz | 6.93 Gbps | 2013 |
| 802.11ax | **WiFi 6/6E** | 2.4/5/6 GHz | 9.6 Gbps | 2019 |

---

## 3. Bandas de Frecuencia

### 2.4 GHz
- **Alcance:** Mayor (mejor penetración de paredes)
- **Velocidad:** Menor
- **Interferencia:** Mayor (microondas, Bluetooth, otros WiFi)
- **Canales sin solapamiento:** 1, 6 y 11

### 5 GHz
- **Alcance:** Menor
- **Velocidad:** Mayor
- **Interferencia:** Menor
- **Canales:** Muchos más disponibles, menos congestionada

---

## 4. Topologías WLAN

| Topología | Descripción | Uso |
|-----------|-------------|-----|
| **Ad Hoc (IBSS)** | Conexión directa entre dispositivos sin AP | Transferencia temporal de archivos |
| **Infraestructura (BSS)** | Clientes conectados a través de un AP | Red doméstica/oficina |
| **ESS (Extended Service Set)** | Múltiples APs con mismo SSID, roaming | Empresas, campus |

---

## 5. Modos de AP

### Autónomo
- Cada AP se configura **independientemente**.
- Ideal para redes pequeñas (1-3 APs).
- Difícil de gestionar a escala.

### Lightweight (Ligero)
- Los APs son gestionados **centralmente** por un WLC.
- Protocolo: **CAPWAP** (Control and Provisioning of Wireless Access Points).
- Usa dos túneles:
  - **Control:** Gestión y configuración (cifrado).
  - **Data:** Tráfico de datos de clientes.
- Ideal para redes medianas y grandes.

---

## 6. Seguridad WLAN

### Evolución de la Seguridad

| Protocolo | Cifrado | Estado | Nivel |
|-----------|---------|--------|-------|
| **Abierta** | Ninguno | ❌ No seguro | — |
| **WEP** | RC4 | ❌ Obsoleto, fácilmente crackeable | — |
| **WPA** | TKIP | ⚠️ Obsoleto | — |
| **WPA2** | AES-CCMP | ✅ Estándar mínimo actual | Bueno |
| **WPA3** | SAE + AES-GCMP | ✅ Más seguro | Excelente |

### Modos de Autenticación

| Modo | Método | Uso |
|------|--------|-----|
| **Personal (PSK)** | Contraseña compartida (Pre-Shared Key) | Hogar, pequeñas empresas |
| **Enterprise** | 802.1X con servidor RADIUS | Empresas (cada usuario tiene credenciales únicas) |

### WPA3 Mejoras
- **SAE (Simultaneous Authentication of Equals):** Reemplaza el handshake de 4 vías, resistente a ataques de diccionario offline.
- **Forward Secrecy:** Aunque se capture tráfico y luego se descubra la contraseña, el tráfico antiguo no se puede descifrar.

---

## 7. Amenazas WLAN

| Amenaza | Descripción | Contramedida |
|---------|-------------|-------------|
| **Rogue AP** | AP no autorizado conectado a la red | Detección con WLC, WIPS |
| **Evil Twin** | AP falso con mismo SSID que el legítimo | 802.1X Enterprise, certificados |
| **Sniffing** | Interceptar tráfico inalámbrico | WPA2/WPA3 con cifrado fuerte |
| **DoS inalámbrico** | Interferir señal o desautenticar clientes | PMF (Protected Management Frames) |
| **Jamming** | Saturar la frecuencia con ruido | Detectar y localizar la fuente |

---

## 8. Configuración Básica

### En WLC (pasos generales):
1. Crear VLAN para la WLAN
2. Crear interfaz en WLC asociada a la VLAN
3. Crear WLAN (definir SSID)
4. Asignar seguridad (WPA2-PSK o Enterprise)
5. Asignar interfaz a la WLAN
6. Habilitar la WLAN

### En router con AP integrado:
```text
Router(config)# dot11 ssid MI_RED
Router(config-ssid)# authentication open
Router(config-ssid)# authentication key-management wpa version 2
Router(config-ssid)# wpa-psk ascii MiContraseña123
Router(config-ssid)# exit
```

---

## 9. Verificación
```text
Router# show wlan summary
Router# show ap summary
Router# show client summary
Router# show dot11 associations
```

---

## Resumen
- WiFi 6 (802.11ax) es el estándar actual más avanzado con soporte para 2.4, 5 y 6 GHz.
- Canales 1, 6 y 11 en 2.4 GHz no se solapan.
- **WPA2** es el mínimo aceptable; **WPA3** con SAE es lo recomendado.
- En empresas, usar modo **Enterprise con RADIUS** y APs **Lightweight con WLC**.
- Las amenazas principales son Rogue APs y Evil Twins.
