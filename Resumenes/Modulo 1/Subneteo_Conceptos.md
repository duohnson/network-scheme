# Conceptos Generales y Subneteo

## Protocolos y Comunicación

Un **protocolo** es un conjunto de reglas que rigen la comunicación entre dispositivos en una red.

*   **Segmentación**: Consiste en dividir los paquetes de datos en partes más pequeñas para facilitar su transmisión a través de la red.
*   **Control de flujo**: Regula la cantidad y la velocidad a la que se envían los mensajes para evitar congestionar el receptor.

### Maneras de entregar un mensaje:
*   **Unicast**: Transmisión de un paquete de un origen a un único destino (1 a 1).
*   **Multicast**: Transmisión de un paquete de un origen a múltiples destinos específicos (1 a varios).
*   **Broadcast**: Transmisión de un paquete de un origen a todos los dispositivos de la red (1 a todos).

---

## Suite de Protocolos y Modelos de Referencia

Una **suite de protocolos** es un conjunto de protocolos diseñados para trabajar juntos. Ejemplo: HTTP, TCP, IP, ETHERNET.

### Comparación entre Modelo OSI y TCP/IP

| Modelo OSI (Referencia) | Modelo TCP/IP (Protocolos) | Ejemplos de Protocolos y PDUs |
| :--- | :--- | :--- |
| 7. **Aplicación** | **Aplicación** | HTTP, DNS, FTP, POP, IMAP |
| 6. **Presentación** | **Aplicación** | (Datos) |
| 5. **Sesión** | **Aplicación** | (Datos) |
| 4. **Transporte** | **Transporte** | TCP, UDP (Segmento: # Puerto + Datos) |
| 3. **Red** | **Internet** | IP, ICMP, NAT (Paquete: IP + Segmento) |
| 2. **Enlace de Datos** | **Acceso a la Red** | Ethernet, MAC, ARP (Trama: MAC + Paquete) |
| 1. **Física** | **Acceso a la Red** | (Bits: 1010101010...) |

*   **Encapsulación**: Proceso de agregar encabezados a los datos a medida que descienden por las capas del modelo OSI.
*   **Desencapsulación**: Proceso de quitar los encabezados a medida que los datos ascienden por las capas.

### Protocolos Comunes por Capa

#### Capa de Aplicación:
*   **HTTP / HTTPS**: Hyper Text Transfer Protocol (Secure)
*   **DNS**: Domain Name Server
*   **FTP / TFTP**: (Trivial) File Transfer Protocol
*   **POP / IMAP / SMTP**: Protocolos de correo electrónico
*   **DHCP**: Dynamic Host Configuration Protocol

#### Capa de Transporte:
*   **TCP**: Transmission Control Protocol (Confiable, orientado a la conexión)
*   **UDP**: User Datagram Protocol (Rápido, sin conexión)

#### Capa de Red:
*   **IP**: Internet Protocol
*   **ICMP**: Internet Control Message Protocol (usado por Ping)
*   **NAT**: Network Address Translation
*   *Protocolos de Enrutamiento*: RIP, EIGRP, OSPF

#### Capa de Acceso a la Red:
*   **Ethernet**
*   **ARP**: Address Resolution Protocol
*   **PPP**: Point to Point Protocol
*   **HDLC**: High Level Data Link Control

---

## Direccionamiento y Dispositivos

### Tipos de Direccionamiento
*   **Lógico**: Dirección IP (ej. `192.168.0.10`). Funciona en la Capa 3 (Red) y se asocia a los **Paquetes**.
*   **Físico**: Dirección MAC (ej. `D4-3B-04-6A-B4-13`). Funciona en la Capa 2 (Enlace de Datos) y se asocia a las **Tramas**. MAC significa *Media Access Control*.

### Dispositivos Intermediarios
| Dispositivo | Acción Principal | Capa OSI | Tipo de Direccionamiento | Ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| **Router (Enrutador)** | Enruta tráfico | Capa 3 (Red) | Direccionamiento Lógico (IP) | `192.168.0.10` |
| **Switch (Conmutador)** | Conmuta tráfico | Capa 2 (Enlace) | Direccionamiento Físico (MAC) | `D4-3B-04-6A-B4-13` |

> [!NOTE]
> La **dirección de gateway** (puerta de enlace) es la dirección IP de la interfaz del router que está conectada a la misma red local que el host, permitiéndole comunicarse con redes externas.

---

## Subneteo (Subnetting)

El subneteo consiste en dividir una red grande en subredes más pequeñas para mejorar la eficiencia y seguridad.

### Reglas y Fórmulas
*   **Sacar Prefijo**: `32 - N = /??` (Donde "N" es la potencia base 2 necesaria para los hosts).
*   **Broadcast**: Dirección de Red + Máscara Wildcard.
*   **1° Utilizable**: Dirección de Red + 1.
*   **Última Utilizable**: Dirección de Broadcast - 1.
*   **Siguiente Red**: Dirección de Broadcast + 1.

### Tabla de Potencias Base 2
| N (Potencia) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Resultado** | 1 | 2 | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1024 |
| **Hosts Utiles (-2)**| - | 0 | 2 | 6 | 14 | 30 | 62 | 126 | 254 | 510 | 1022 |

### Ejemplo Práctico de Subneteo

**Red Inicial**: `10.0.0.0/16`
Se requieren subredes para 300, 100 y 32 hosts. Se deben acomodar de mayor a menor.

| Requisito | Hosts Requeridos (+2) | Fórmula Prefijo | Máscara / Wildcard | Dirección de Red | Rango Utilizable | Dirección de Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **300 Hosts** | 300 + 2 (512, N=9) | 32 - 9 = **/23** | 255.255.254.0 <br> *(W: 0.0.1.255)* | `10.0.0.0` | `10.0.0.1` - `10.0.1.254` | `10.0.1.255` |
| **100 Hosts** | 100 + 2 (128, N=7) | 32 - 7 = **/25** | 255.255.255.128 <br> *(W: 0.0.0.127)* | `10.0.2.0` | `10.0.2.1` - `10.0.2.126` | `10.0.2.127` |
| **32 Hosts** | 32 + 2 (64, N=6) | 32 - 6 = **/26** | 255.255.255.192 <br> *(W: 0.0.0.63)* | `10.0.2.128` | `10.0.2.129` - `10.0.2.190` | `10.0.2.191` |

*(Nota: Para calcular la siguiente red, se suma 1 a la dirección de broadcast de la red anterior).*
