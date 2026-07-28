# Nota Avanzada - Clase 3: Enrutamiento Inter-VLAN

## Introducción
El enrutamiento Inter-VLAN es el proceso de reenviar tráfico de red de una VLAN a otra mediante un dispositivo de Capa 3 (router o switch multicapa). Las VLANs son dominios de broadcast lógicamente separados, por lo que el tráfico no puede pasar de una a otra sin un enrutador.

## 1. Métodos de Enrutamiento Inter-VLAN

### 1.1 Enrutamiento Tradicional (Legacy)
Históricamente, se usaba una interfaz física distinta en el router para cada VLAN. 
- **Desventaja:** Escala muy mal. Si tienes 10 VLANs, necesitas 10 puertos en el router y 10 en el switch.

### 1.2 Router-on-a-Stick
Utiliza un solo puerto físico en el router conectado a un puerto troncal en el switch.
- El puerto del router se divide en múltiples **subinterfaces** lógicas (por ejemplo, `FastEthernet 0/0.10`).
- Cada subinterfaz se asigna a una VLAN usando encapsulación IEEE 802.1Q.
- Actúa como puerta de enlace predeterminada para esa VLAN.

### 1.3 Switch Multicapa (SVI)
Los switches de Capa 3 pueden realizar enrutamiento interno a velocidad de hardware.
- Utilizan **Interfaces Virtuales de Switch (SVI)**.
- Se crea una interfaz lógica por VLAN (`interface vlan 10`).
- Método más eficiente y escalable en redes empresariales.

## 2. Configuración: Router-on-a-Stick

### En el Switch:
Configurar el puerto que conecta al router como troncal.
```text
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
```

### En el Router:
Crear las subinterfaces y asignarles encapsulación y dirección IP.
```text
Router(config)# interface gigabitEthernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
```

## 3. Configuración: Switch Multicapa con SVI

Para habilitar el enrutamiento y crear las SVI:
```text
Switch(config)# ip routing  <- Comando crítico para habilitar capa 3
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown
```
También es posible configurar un puerto físico del switch para que actúe como un puerto de router usando el comando `no switchport`.

## 4. Resolución de Problemas (Troubleshooting)
1. **Encapsulación incorrecta:** La VLAN en la subinterfaz (`encapsulation dot1Q X`) debe coincidir con la VLAN del switch.
2. **Puertos de acceso vs troncales:** Asegurar que el puerto del switch hacia el router sea trunk.
3. **Gateway predeterminado:** Las PCs deben apuntar a la IP de la subinterfaz o SVI correspondiente a su VLAN.
