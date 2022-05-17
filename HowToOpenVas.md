# How to OpenVas (GVM Greenbone Vulnerability Management)

---

## Instal·lació de GVM amb OpenVas per a Debian 11


---

### 1) Creació de l'usuari GVM

Hem de crear l'usuari GVM per tal de que podem gestionar tota aquesta suit.

```
$sudo useradd -r -d /opt/gvm -c "GVM User" -s /bin/bash gvm
$sudo mkdir /opt/gvm && chown gvm: /opt/gvm
```
Seguidament, haurem d'instal·lar les dependències necesàries i diverses eines que ens ajudaran en el procés d'instal·lació.
```
$sudo apt install gcc g++ make bison flex libksba-dev \
curl redis libpcap-dev cmake git pkg-config libglib2.0-dev libgpgme-dev \
nmap libgnutls28-dev uuid-dev libssh-gcrypt-dev libldap2-dev gnutls-bin \
libmicrohttpd-dev libhiredis-dev zlib1g-dev libxml2-dev libnet-dev libradcli-dev \
clang-format libldap2-dev doxygen gcc-mingw-w64 xml-twig-tools libical-dev perl-base \
heimdal-dev libpopt-dev libunistring-dev graphviz libsnmp-dev python3-setuptools \
python3-paramiko python3-lxml python3-defusedxml python3-dev gettext python3-polib \
xmltoman python3-pip texlive-fonts-recommended \
texlive-latex-extra --no-install-recommends xsltproc sudo vim rsync -y
```

A continuació, instal·larem, Yarn, què és un gestor de paquets de Javasript.

```
$sudo curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
$sudo echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
$sudo apt update
$sudo apt install yarn -y
```

### 2) Instal·lació i configuració de PostgreSQL per el backend de GVM.

La creació de la Base de Dades i de l'usuari que la controlarà, és l'usuari postgres de PostgreSQL
```
$sudo apt install postgresql-13 postgresql-contrib-13 postgresql-server-dev-13 -y
$sudo -Hiu postgres
$createuser gvm
$createdb -O gvm gvmd
$psql gvmd
$create role dba with superuser noinherit;
$grant dba to gvm;
$\q
```

### 3) Creació i construcció del diversos mòduls de GVM.

Aquesta suit compta amb diversos mòduls que ens ajudaran en les diverses funcions de GVM.

```
* GVM Libraries
* OpenVAS Scanner
* OSPd
* OSPd-OpenVAS
* Greenbone Vulnerability Manager
* Greenbone Security Assistant
* Phyton-GVM
* GVM-Tools
* OpenVAS SMB
```
