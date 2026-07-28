# Nota Avanzada - Clases 1 y 2: Introducción a VLANs y Enrutamiento Básico

## Introducción

Las Redes de Área Local Virtuales (VLANs) y el enrutamiento entre ellas (Inter-VLAN Routing) son conceptos fundamentales en el diseño de redes modernas con switches gestionables. Esta nota avanzada reconstruye los conceptos principales de las primeras clases del módulo, enfocándose en la segmentación de red y la comunicación capa 3.

## 1. Conceptos Básicos de Switching

Antes de entender las VLANs, es importante recordar cómo funciona un switch de capa 2.

### 1.1 Reenvío de Tramas
- Los switches toman decisiones basadas en direcciones MAC de destino.
- Mantienen una tabla de direcciones MAC (Tabla CAM).
- Si la MAC destino es desconocida, el switch inunda la trama por todos los puertos excepto por el que se recibió (Flooding).

### 1.2 Dominios de Colisión y Broadcast
- **Dominio de Colisión**: Cada puerto de un switch representa un dominio de colisión independiente.
- **Dominio de Broadcast**: Por defecto, todos los puertos de un switch pertenecen al mismo dominio de broadcast. Un mensaje de broadcast (MAC FF:FF:FF:FF:FF:FF) llegará a todos los dispositivos conectados.

---

## 2. Redes de Área Local Virtuales (VLANs)

### 2.1 ¿Qué es una VLAN?
Una VLAN es una conexión lógica que permite segmentar una red física en múltiples redes lógicas (y por ende, múltiples dominios de broadcast) dentro del mismo switch o a través de varios switches.

### 2.2 Beneficios de las VLANs
1. **Seguridad mejorada**: Los datos confidenciales se separan del resto de la red.
2. **Reducción de costos**: No se necesitan múltiples switches físicos para diferentes grupos.
3. **Mejor rendimiento**: Al dividir los dominios de broadcast, se reduce el tráfico innecesario en la red.
4. **Mitigación de tormentas de broadcast**: Contener un broadcast a una sola VLAN evita que sature toda la red.
5. **Mayor eficiencia del personal de TI**: Facilita la gestión; usuarios con requerimientos similares comparten la misma VLAN sin importar su ubicación física.

### 2.3 Tipos de VLANs
- **VLAN de Datos**: Usada para el tráfico generado por usuarios (correo, web, etc.). A veces llamada VLAN de usuario.
- **VLAN Predeterminada (Default)**: Todos los puertos del switch son miembros de la VLAN 1 al inicio.
- **VLAN Nativa**: Asignada a un puerto troncal (802.1Q). El tráfico sin etiquetar se coloca en la VLAN nativa.
- **VLAN de Administración**: Cualquier VLAN configurada para acceder a las capacidades de administración del switch (SSH/Telnet).
- **VLAN de Voz**: Reservada para tráfico de Voz sobre IP (VoIP), asegurando ancho de banda y prioridad.

---

## 3. Configuración de VLANs y Puertos

### 3.1 Puertos de Acceso
Un puerto de acceso pertenece a una única VLAN y generalmente se conecta a dispositivos finales (PC, impresora).

**Comandos básicos:**
```text
Switch(config)# vlan 10
Switch(config-vlan)# name Ventas
Switch(config-vlan)# exit
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

### 3.2 Puertos Troncales (Trunks)
Un puerto troncal es un enlace punto a punto entre switches (o entre un switch y un router) que transporta tráfico de múltiples VLANs. Utiliza el protocolo de encapsulamiento IEEE 802.1Q para "etiquetar" las tramas.

**Etiqueta 802.1Q:**
Inserta un campo de 4 bytes en la trama Ethernet original para identificar a qué VLAN pertenece el paquete.

**Comandos básicos:**
```text
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,30,99
```

---

## 4. Enrutamiento Inter-VLAN (Inter-VLAN Routing)

Las VLANs segmentan la red lógicamente. Los dispositivos en la VLAN 10 no pueden comunicarse directamente con los dispositivos en la VLAN 20 usando solo un switch de capa 2. Se necesita un dispositivo de capa 3 (Router o Switch Multicapa).

### 4.1 Enfoque Legacy (Tradicional)
- Requiere una interfaz física del router por cada VLAN.
- Muy ineficiente y no escalable.

### 4.2 Router-on-a-Stick
- Utiliza una sola interfaz física del router conectada a un puerto troncal del switch.
- La interfaz física se divide en múltiples "subinterfaces" lógicas.
- Cada subinterfaz se asigna a una VLAN específica y actúa como el "Default Gateway" para esa VLAN.

**Configuración de Router-on-a-Stick:**
```text
Router(config)# interface gigabitEthernet 0/0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface gigabitEthernet 0/0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

Router(config)# interface gigabitEthernet 0/0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
```

### 4.3 Switches Multicapa (Capa 3)
- Los switches modernos pueden realizar funciones de enrutamiento.
- Utilizan interfaces virtuales llamadas SVI (Switch Virtual Interface).
- Se crea una SVI por cada VLAN y se le asigna una dirección IP.
- Es el método más rápido y escalable en redes corporativas modernas.

**Configuración en Switch Multicapa:**
```text
Switch(config)# ip routing
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown
```

---

## 5. Resolución de Problemas (Troubleshooting) Básica

Al enfrentar problemas de conectividad con VLANs, verifique:
1. **Asignación de puertos**: ¿El PC está en el puerto correcto y asignado a la VLAN correcta? (`show vlan brief`)
2. **Estado del Troncal**: ¿El enlace entre switches está configurado como trunk? (`show interfaces trunk`)
3. **VLAN Nativa**: ¿Hay discrepancia de VLAN nativa entre los switches? Esto causará mensajes de error CDP.
4. **Direccionamiento IP**: ¿Los dispositivos tienen la IP y máscara de subred correctas para su VLAN? ¿Tienen configurado el Gateway predeterminado?
5. **Subinterfaces del Router**: ¿Las subinterfaces tienen la encapsulación 802.1Q correcta que coincide con el ID de la VLAN?

## 6. Conclusión
El uso de VLANs junto con el enrutamiento Inter-VLAN permite redes escalables, seguras y de alto rendimiento. Comprender cómo las tramas son etiquetadas y cómo los routers procesan este tráfico es esencial para cualquier administrador de red.
