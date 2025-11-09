## Cliente DHCP (MV 4)

### Obtener IP por DHCP

En el Cliente (MV 4) → Pedir IP al servidor
```bash
dhclient -r → Soltamos la que tenemos, si es que tenemos
dhclient -v → Solicitamos una nueva IP
```

---

### Verificar la IP

Verifica que el cliente haya recibido una IP del rango **192.168.10.100 - 192.168.10.200** y que el router sea **192.168.10.1**:
```bash
ip a
```

---

### Verificar Logs

En el **Servidor ISC DHCP (MV 1)**, revisa el log para ver que la petición llegó a través del relay:
```bash
journalctl -u isc-dhcp-server
```
