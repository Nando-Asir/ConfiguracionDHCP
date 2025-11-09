## [CONFIGURACIÓN DHCP RELAY (MV 3)](README.md)

---

### Instalación

Instala el paquete **isc-dhcp-relay**:
```bash
apt update
apt install isc-dhcp-relay -y
```
---

### Configuración del Relay

Edita el archivo de configuración del relay para indicarle a qué servidores DHCP debe enviar las peticiones y por qué interfaz debe escuchar las peticiones de los clientes.\
\
Comando en MV 3:
```bash
nano /etc/default/isc-dhcp-relay
```

**Contenido para ******/etc/default/isc-dhcp-relay****** (MV 3 - Servidor Relay):**\
\
**Dirección IP del servidor **o servidores** DHCP**\
**Apunta a ambos servidores DHCP (MV 1 y MV 2) en la red 192.168.2.0/24**
- **SERVERS=\"192.168.2.1 192.168.2.2\"**

**Interfaces por donde el relay escucha a los clientes**\
**Debe ser la interfaz de la red interna \'relay\' (192.168.10.0/24)**
- **INTERFACES=\"enp0s3 enp0s8\"**

---

### Habilitar el Reenvío (Forwarding)

El Servidor Relay (MV 3) necesita actuar como un router básico para reenviar los paquetes entre la red **relay** (192.168.10.0/24) y la red **dhcp** (192.168.2.0/24).\
\
Comando en MV 3:
```bash
nano /etc/sysctl.conf**
```

Descomenta o añade la siguiente línea:
- **net.ipv4.ip_forward=1**

Aplica el cambio sin reiniciar:
```bash
sysctl -p
```
---

### Iniciar y Verificar el Servicio Relay (MV 3)

Inicia y verifica el servicio:
- **systemctl start isc-dhcp-relay**
- **systemctl enable isc-dhcp-relay**
- **systemctl status isc-dhcp-relay**
