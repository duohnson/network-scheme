# Nota Avanzada de Clase 10: Capa de Aplicación

La Capa de Aplicación es la capa superior del modelo OSI y TCP/IP, que interactúa directamente con las aplicaciones de software.

## Protocolos de la Capa de Aplicación

### HTTP (HyperText Transfer Protocol)

Utilizado para la transferencia de páginas web y recursos en la World Wide Web.

*   **URL (Localizador Uniforme de Recursos)**:
    Partes principales de una URL:
    *   `http`: El protocolo o esquema.
    *   `www.cisco.com`: El nombre del servidor (dominio).
    *   `index.html`: El nombre de archivo específico solicitado.
*   **Mensajes de HTTP**:
    *   `GET`: Solicitar/Obtener información.
    *   `POST`: Enviar/Subir información.
    *   `PUT`: Subir/Actualizar información.

### Correo Electrónico

El correo electrónico depende de múltiples protocolos para enviar y recibir mensajes.

| Protocolo | Descripción | Puerto Común |
| :--- | :--- | :--- |
| **SMTP** (Simple Mail Transport Protocol) | Envía el correo al servidor. | 25 |
| **POP** (Post Office Protocol) | Recibe correo y lo elimina del servidor. | 110 |
| **IMAP** (Internet Message Access Protocol) | Recibe correo y guarda una copia de respaldo en el servidor. | 143 |

*   **Terminología de Email**:
    *   **MUA (Mail User Agent)**: Agente de usuario (cliente de correo).
    *   **MTA (Mail Transport Agent)**: Agente de transporte (servidor que rutea).
    *   **MDA (Mail Delivery Agent)**: Agente de entrega (entrega al buzón).

### Nombres de Dominio y DNS

*   **DNS (Domain Name System)**: Traduce nombres de dominio legibles por humanos (ej. www.cisco.com) en direcciones IP. (Puerto **53**).
*   **nslookup**: Herramienta de línea de comandos utilizada para resolver nombres de dominio en direcciones IP.

### DHCP (Dynamic Host Configuration Protocol)

Asigna direcciones IP y otros parámetros de red dinámicamente a los hosts.

*   **Proceso de obtención de IP (DORA)**:
    1.  **DISCOVER**: El cliente busca servidores DHCP (Broadcast).
    2.  **OFFER**: El servidor ofrece una IP al cliente (Unicast).
    3.  **REQUEST**: El cliente solicita la IP ofrecida (Broadcast).
    4.  **ACKNOWLEDGE (ACK)**: El servidor confirma la asignación (Unicast).
*   **Proceso de renovación**:
    1.  **REQUEST (REN)**: El cliente solicita renovar su IP actual (Unicast).
    2.  **ACKNOWLEDGE (ACK)**: El servidor confirma la renovación (Unicast).

### Otros Protocolos y Puertos Clave

| Protocolo | Descripción | Puertos | Tipo | Detalles |
| :--- | :--- | :--- | :--- | :--- |
| **FTP** (File Transfer Protocol) | Transferencia confiable de archivos. | 20, 21 | TCP | Requiere usuario / contraseña. |
| **TFTP** (Trivial File Transfer Protocol)| Transferencia simple y rápida de archivos. | 69 | UDP | Sin autenticación. |
| **SMB** (Server Message Block) | Comparte archivos y recursos en redes Microsoft. | - | - | - |
