# Generalidades

## Dispositivos Finales e Intermedios

Entendemos que existen dispositivos finales (computadores, impresoras, smartphones, etc.) 
y dispositivos intermedios (routers, switches, etc.)

Los dispositivos finales son los que generan y consumen datos, mientras que los dispositivos intermedios son los que transportan los datos entre los dispositivos finales.

Estos dispositivos intermedios funcionan como la columna vertebral de la red.

Para que me comprendan, los dispositivos finales es donde finaliza la comunicación, es decir, donde se envía y se recibe la información.
Por otro lado, los dispositivos intermedios son los que se encargan de transportar la información entre los dispositivos finales.

Un paquete viaja por la red (dispositivos intermedios) para llegar a su destino (dispositivos finales) que es donde el paquete se queda, y de ahí vuelve 
por el mismo camino, pero a la inversa, hasta llegar al dispositivo final.

![Dispositivos Finales e Intermedios](/Imagenes/CCNA/finales_intermedios.png)

## ¿Que es una IP y que significa?. 

INTERNET PROTOCOL (Protocolo de Internet) 

En simples palabras, IP es una etiqueta que se le asigna a un dispositivo para poder identificarlo en una red.
(Como el número de identificación de una persona, solo que en este caso es de un dispositivo).

## QoS (Quality of Service)

La calidad de servicio es la capacidad de una red para proporcionar diferentes niveles de servicio a diferentes tipos de tráfico.

¿Para que sirve?

Sirve para priorizar el tráfico de red, es decir, para que el tráfico importante tenga prioridad sobre el tráfico menos importante.

Por ejemplo, si estamos en una videollamada y al mismo tiempo estamos descargando un archivo, el tráfico de la videollamada tiene prioridad sobre el tráfico de la descarga.

## Como viaja la informacion por la red. (Fisicamente)

Existen varios medios por los cuales viaja la información por la red:

- Cobre
- Fibra Optica
- Inalambrico
- Telefonico

## Como viaja la información por la red. (Logicamente)

Para que la información viaje por la red, debe ser dividida en paquetes.
Cada paquete tiene una dirección IP de origen y una dirección IP de destino.
Cada paquete tiene un número de secuencia para que pueda ser reensamblado en el destino.

Y sus tipos de red son:

- LAN (Local Area Network)
- WAN (Wide Area Network)
- MAN (Metropolitan Area Network)
- PAN (Personal Area Network)
- WLAN (Wireless Local Area Network)

![Tipos de red](/Imagenes/CCNA/tipos_de_red.png)

¿Porque no aparece en la imagen WLAN?. 

Porque el vivo ejemplo de una red WLAN es la que estamos utilizando en este momento, que es la red Wi-Fi de la casa. 
(Es decir, la red que se conecta por medio de un router inalámbrico y posee conexión por ethernet tambien).

¿Entonces en mi casa yo contraté internet, tengo un modem o un router?

Se le puede llamar ya que cumple las funciones de ambos, es decir, es un modem y un router a la vez.
Sin embargo su terminologia correcta es ISR (Integrated Service Router).

## Convergencia de Redes

Basicamente es cuando una red puede transportar diferentes tipos de tráfico, como voz, datos y video.
Las redes actuales por lo general son convergentes, sin embargo no siempre fue asi.
Al principio existian redes que solo transportaban datos, otras que solo transportaban voz y otras que solo transportaban video.
Por eso algunos datafonos no funcionaban si en la misma red habia una llamada en curso. (Por el tipo de tráfico que transportaba la red).
Eso ya no sucede gracias a la convergencia de redes.

## ¿Cuales son las caracteristicas de una red?. 

Las caracteristicas principales de una red son (principales me refiero a más comunes porque hay muchas más):

- Tolerancia a Fallos
- Escalabilidad
- Calidad de Servicio (QoS)
- Seguridad

### Tolerancia a Fallos

Es la capacidad de una red para seguir funcionando incluso si algunos de sus componentes fallan. 
Por lo general una red esta compuesta por varios dispositivos, por lo que si uno falla, los otros pueden seguir funcionando.

### Escalabilidad

Es la capacidad de una red para crecer y adaptarse a nuevas necesidades. 
Por ejemplo, si en una red hay 10 dispositivos y se le agregan 10 más, la red debe poder soportar el aumento de tráfico.

### Calidad de Servicio (QoS)

Es la capacidad de una red para proporcionar diferentes niveles de servicio a diferentes tipos de tráfico. 
Por ejemplo, si en una red hay una videollamada y una descarga de archivos, la videollamada tiene prioridad sobre la descarga.

### Seguridad

Seguridad fisica y logica.

- Seguridad fisica: Es la seguridad de los dispositivos fisicos, es decir, de los routers, switches, etc.
    * No permitir el acceso a personas no autorizadas a la sala de servidores. (O al rack donde se encuentran los dispositivos).
    * Proteccion contra cortocircuitos.
    * Proteccion contra sobrecargas.
    * Proteccion contra variaciones de voltaje.
- Seguridad logica: Es la seguridad de los datos, es decir, de los archivos, etc.
    * Proteccion contra virus.
    * Proteccion contra hackers.
    * Proteccion contra robo de datos.
    * Encriptacion de datos.
    * Autenticacion (Validar la identidad del usuario).
    * etc etc etc. 

Esto, en la primera sección del módulo 1.

========================================================================================================================================================

