## Configuración Servidor ISC DHCP Y FAILOVER (MV 1 y MV 2)

---

### Instalación (MV 1 y MV 2)

Instala el servidor ISC DHCP en ambas máquinas:
- **sudo apt update**
- **sudo apt install isc-dhcp-server -y**

---

### Habilitar la Interfaz (MV 1 y MV 2)

Indica al servicio la interfaz que debe escuchar (la de la red interna **dhcp**). Usa el nombre de la interfaz configurada para **192.168.2.x**.

Comando en ambas MVs:
-   **nano /etc/default/isc-dhcp-server**

Busca la línea de **INTERFACESv4** y edítala (reemplazando **enp0s8**):
-   INTERFACESv4= **enp0s8**

---

### Configuración del Failover y Subredes (MV 1 - Servidor Principal)

Edita el archivo de configuración principal.
Comando en MV 1:
-   **nano /etc/dhcp/dhcpd.conf**

Contenido para **/etc/dhcp/dhcpd.conf** (MV 1 - Servidor Principal):

**Descomenta esta línea para que el servidor sea la autoridad en su
red**
- **authoritative;**

**Opciones globales**
- **option domain-name \"red.local\";**
- **option domain-name-servers 8.8.8.8, 8.8.4.4;**
- **default-lease-time 600;**
- **max-lease-time 7200;**

**Declaración de la relación de Failover**
- **failover peer \"dhcp-failover-group\" {**
- ** primary;**
- ** address 192.168.2.1; → IP propia (MV 1)**
- ** port 647;****
- ** peer address 192.168.2.2; → IP del compañero (MV 2)**
- ** peer port 647;**
- ** max-response-delay 60;**
- ** max-unacked-updates 10;**
- ** mclt 1800;**
- ** split 128;**
- **}**

**Subred para la Red Interna \'dhcp\' (192.168.2.0/24)**

**Esta subred solo la usamos para la comunicación interna entre
servidores DHCP.**

-   ****subnet 192.168.2.0 netmask 255.255.255.0 {****
-   ****}****

**Subred para el Cliente (a través del Relay) (192.168.10.0/24)**

**El Servidor DHCP (MV 1) sirve IPs en esta subred.**

**La IP del router/gateway debe ser la IP de la interfaz del Servidor
Relay (MV 3)**

-   ****subnet 192.168.10.0 netmask 255.255.255.0 {****
-   **** pool {****
-   **** failover peer \"dhcp-failover-group\";****
-   **** range 192.168.10.100 192.168.10.200;****
-   **** option routers 192.168.10.1;**** → IP del Servidor Relay (MV
    3)**
-   **** }****
-   ****}****

D. Configuración del Failover (MV 2 - Servidor Secundario)

Comando en MV 2:

-   ****sudo nano /etc/dhcp/dhcpd.conf****

**Contenido para ******/etc/dhcp/dhcpd.conf****** (MV 2 - Servidor
Secundario):**

**Descomenta esta línea para que el servidor sea la autoridad en su
red**

-   ****authoritative;****

**

**

**

**Opciones globales**

-   ****option domain-name \"red.local\";****
-   ****option domain-name-servers 8.8.8.8, 8.8.4.4;****
-   ****default-lease-time 600;****
-   ****max-lease-time 7200;****

**Declaración de la relación de Failover (secundaria)**

-   ****failover peer \"dhcp-failover-group\" {****
-   **** secondary;****
-   **** address 192.168.2.2; → IP propia (MV 2)****
-   **** port 647;****
-   **** peer address 192.168.2.1; → IP del compañero (MV 1)****
-   **** peer port 647;****
-   **** max-response-delay 60;****
-   **** max-unacked-updates 10;****
-   ****}****

**Subred para la Red Interna \'dhcp\' (192.168.2.0/24)**

-   ****subnet 192.168.2.0 netmask 255.255.255.0 {****
-   **}**

**Subred para el Cliente (a través del Relay) (192.168.10.0/24)**

-   ****subnet 192.168.10.0 netmask 255.255.255.0 {****
-   **** pool {****
-   **** failover peer \"dhcp-failover-group\";****
-   **** range 192.168.10.100 192.168.10.200;****
-   **** option routers 192.168.10.1;**** → IP del Servidor Relay (MV
    3)**
-   **** }****
-   ****}****

E. Iniciar y Verificar el Servicio DHCP (MV 1 y MV 2)

Inicia el servicio en **ambos** servidores:

-   ****sudo systemctl start isc-dhcp-server****
-   ****sudo systemctl enable isc-dhcp-server****
-   ****sudo systemctl status isc-dhcp-server****

Hacer un **journal -u isc-dhcp-server.service** para comprobar que todo
funcional correctamente.

Añadir → **ip route add 192.168.10.0/24 via 192.168.2.10** → Permitimos
que de la IP al cliente.
