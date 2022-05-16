# How to OpenVas (GVM Greenbone Vulnerability Management)

---

## Instal·lació de GVM amb OpenVas per a Debian 11


---

### 1) Creació de l'usuari GVM

Hem de crear l'usuari GVM per tal de que podem gestionar tota aquesta suit.

```
$useradd -r -d /opt/gvm -c "GVM User" -s /bin/bash gvm
$mkdir /opt/gvm && chown gvm: /opt/gvm

```

