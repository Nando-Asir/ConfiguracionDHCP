## [Configuración Previa en VirtualBox](README.md)

1.  Servidor ISC DHCP (MV 1) y Failover (MV 2):
    -   Adaptador 1: **Adaptador Puente** (para la red 192.168.1.0/24)
    -   Adaptador 2: **Red Interna**, Nombre: **dhcp** (para la red
        192.168.2.0/24)

2.  Servidor Relay (MV 3):
    -   Adaptador 1: **Red Interna**, Nombre: **dhcp** (para la red
        192.168.2.0/24)
    -   Adaptador 2: **Red Interna**, Nombre: **relay** (para la red
        192.168.10.0/24)

3.  Cliente (MV 4):
    -   Adaptador 1: **Red Interna**, Nombre: **relay** (para la red
        192.168.10.0/24)
