# Comandos básicos tshark en servidores linux.

## Comando para listar interfaces de red disponibles:

```bash
    tshark -D
```

## Comando para capturar tráfico en una interfaz específica:

```bash
    tshark -i ens3
```

## Comando para guardar la captura en un archivo:

```bash
    tshark -i ens3 -w <nombre_del_archivo>.pcap
```
## Comando para mostrar solo los paquetes HTTP:

```bash
    tshark -i ens3 -Y "http"
```

## Comando para mostrar solo los paquetes DNS:

```bash
    tshark -i ens3 -Y "dns"
```

## Comando para mostrar solo los paquetes TCP:

```bash
    tshark -i ens3 -Y "tcp"
```

## Comando para mostrar solo los paquetes UDP:

```bash
    tshark -i ens3 -Y "udp"
```

## Comando para mostrar solo los paquetes ICMP:

```bash
    tshark -i ens3 -Y "icmp"
```

## Comando para mostrar solo los paquetes de una dirección IP específica:

```bash
    tshark -i ens3 -Y "ip.addr == <dirección_ip>"
```

## Comando para mostrar solo los paquetes de un puerto específico:

```bash
    tshark -i ens3 -Y "tcp.port == <número_de_puerto>"
```

## Comando para mostrar solo los paquetes de un protocolo específico:

```bash
    tshark -i ens3 -Y "<protocolo>"
```

## Comando para mostrar solo los paquetes de una conversación específica:

```bash
    tshark -i ens3 -q -z conv,ip
```

## Comando para deteccion de escaneos (mucho SYN)

```bash
    tshark -i ens3 -Y "tcp.flags.syn == 1 and tcp.flags.ack == 0"
```

## Comando para deteccion de escaneos (mucho ACK)

```bash
    tshark -i ens3 -Y "tcp.flags.ack == 1 and tcp.flags.syn == 0"
```

