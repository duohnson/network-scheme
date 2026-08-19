# Nota Avanzada de Clase 2-3: Conjunto de Protocolos y Modelo OSI

## Conjunto de Protocolos de Red (Suite de Protocolos)

En muchos casos, los protocolos deben poder trabajar con otros protocolos para que su experiencia en línea le proporcione todo lo que necesita para las comunicaciones de red. Los conjuntos de protocolos están diseñados para funcionar entre sí sin problemas y se denominan **suite de protocolos**.

### Capas y Protocolos

*   **CAPA DE APLICACIÓN**:
    Asume responsabilidades de interfaz de servicios de red, representación y formato de datos, y control del diálogo.
    *   *Protocolos*: DNS, DHCP, SLAAC, SMTP, POP3, IMAP, FTP, SFTP, TFTP, HTTP, HTTPS, REST.
*   **CAPA DE TRANSPORTE**:
    Asume la responsabilidad de entregar de forma correcta la información.
    *   *Protocolos*: TCP, UDP.
*   **CAPA DE INTERNET**:
    Permite el redireccionamiento y que los dispositivos se identifiquen.
    *   *Protocolos*: IPv4, IPv6, NAT, ICMPv4, ICMPv6, OSPF, EIGRP, BGP.
*   **CAPA DE ACCESO DE RED**:
    Permite la resolución de direcciones y el encuentro a nivel de hardware.
    *   *Protocolos*: ARP, ETHERNET, WLAN.

---

## Modelo OSI (Open System Interconnection) y PDU (Protocol Data Unit)

El modelo OSI es un marco conceptual que describe las funciones de un sistema de red.

| Capa del Modelo OSI | Nombre PDU | Funciones / Detalles Adicionales |
| :--- | :--- | :--- |
| **Aplicación** | Datos | DNS, HTTP, DHCP, IMAP, POP, SMTP |
| **Presentación** | Datos | Formato y cifrado de datos |
| **Sesión** | Datos | Control del diálogo |
| **Transporte** | Segmento | **Puerto** + Datos |
| **Red** | Paquete | **IP** + Segmento |
| **Enlace de Datos** | Trama | **MAC** + Paquete |
| **Física** | Bits | 10101010101010101 |

### ¿Para qué sirve el puerto?
El puerto identifica el tipo de dato, para que cuando ese segmento llegue al firewall, sepa a dónde enviarlo. Es un identificador numérico para identificar el dato del segmento.

### Conceptos Clave de Comunicación

*   **Encapsular**: Agregar encabezados y empaquetar la trama.
*   **Desencapsular**: Quitar encabezados.
*   **Segmentar**: Dividir datos.
*   **Multiplexación**: Intercalar datos.
*   **Convergencia**: Transmitir voz, video y todos los datos en el mismo medio.

### Tipos de Fibra Óptica

*   **Monomodo**: Usa Láser - Alcanza mayor distancia.
*   **Multimodo**: Usa LED - Permite mayor velocidad (en cortas distancias).

---

## MAC (Media Access Control)

La subcapa MAC se encarga de:

1.  **Encapsular la Trama**:
    *   *Estructura de la trama*: `[Inicio | Dir MAC | Tipo/Control de Flujo | PAQUETE | FCS | Final]`
    *   **FCS (Frame Check Sequence / Secuencia de Verificación de Trama)**: Utiliza **CRC** (Comprobación de Redundancia Cíclica) para la detección de errores.
2.  **Controlar el Acceso al Medio**:
    *   *Controlado*: Por turnos, evita colisiones.
    *   *Contienda*: Escucha el medio para detectar colisiones.
        *   **CSMA/CD** (802.3): Ethernet (Detección de colisiones).
        *   **CSMA/CA** (802.11): WLAN (Prevención de colisiones).

### Modos de Comunicación

*   **Full Duplex**: Capacidad de recibir y enviar datos a la vez.
*   **Half Duplex**: Solo envía o solo recibe datos a la vez.

### Encapsulación de Datos

*   Implica el armado de la trama antes de la transmisión y el desarmado de la trama en el momento en que se la recibe.
*   La capa MAC agrega un encabezado y un tráiler a la PDU de la capa de red.
*   Proporciona tres funciones principales:
    1.  **Delimitación de tramas**: Identifica un grupo de bits que componen una trama; proporciona sincronización entre los nodos emisor y receptor.
    2.  **Direccionamiento**: Cada encabezado Ethernet contiene la dirección física (dirección MAC) que permite entregar la trama a un nodo de destino.
    3.  **Detección de errores**: Comprobación de redundancia cíclica (CRC) del contenido de la trama.
