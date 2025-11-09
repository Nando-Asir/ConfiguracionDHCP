## [Configuración de Red Estática](README.md)

En las 4 máquinas virtuales, configuramos las IPs estáticas editando el archivo de configuración de red.

Comando en todas las MVs:
- `nano /etc/network/interfaces`

---

### `Servidor ISC DHCP (MV 1)`

IPs a configurar:
- Adaptador Puente: **192.168.1.50**
- Red **dhcp**: **192.168.2.1**

Archivo → **/etc/network/interfaces**

```bash
auto enp0s3
iface enp0s3 inet static
 address 192.168.1.50
 netmask 255.255.255.0
 gateway 192.168.1.1

auto enp0s8
iface enp0s8 inet static
 address 192.168.2.1
 netmask 255.255.255.0
```

---

### `Servidor Failover (MV 2)`

IPs a configurar:
- Adaptador Puente: **192.168.1.51**
- Red **dhcp**: **192.168.2.2**

Archivo → **/etc/network/interfaces**

```bash
auto enp0s3
iface enp0s3 inet static
 address 192.168.1.51
 netmask 255.255.255.0
 gateway 192.168.1.1

auto enp0s8
iface enp0s8 inet static
 address 192.168.2.2
 netmask 255.255.255.0
```

---

### `Servidor Relay (MV 3)`

IPs a configurar:
- Red **dhcp**: **192.168.2.10**
- Red **relay**: **192.168.10.1**

Archivo → **/etc/network/interfaces**

```bash
auto enp0s3
iface enp0s3 inet static
 address 192.168.2.10
 netmask 255.255.255.0

auto enp0s8
iface enp0s8 inet static
 address 192.168.10.1
 netmask 255.255.255.0
```

---

### `Cliente (MV 4)`

El cliente tomará IP por DHCP.
Archivo → **/etc/network/interfaces**

```bash
auto enp0s3
iface enp0s3 inet dhcp
```

---

### Aplicar Configuración de Red (Todas las MVs)

Reinicia el servicio de red en las 4 MVs:
- `systemctl restart networking`

---

### [Pasos Previos <- Anterior](vbox.md) | [Siguiente -> Servidor & Failover](servidor.md)
