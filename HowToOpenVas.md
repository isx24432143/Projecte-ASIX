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
Per tal de fer aquest procés, ens convertirem en el usuari GVM, creat previament, i crearem un directori, on estaran ubicats, tots els mòduls per fer la instal·lació. Per descarregar-nos aquest mòduls, farem us del seu Repositori Oficial de [GitHub](https://github.com/greenbone/)
```
$su - gvm
$mkdir gvm-source
$cd gvm-source
$git clone -b stable --single-branch https://github.com/greenbone/gvm-libs.git
$git clone -b main --single-branch https://github.com/greenbone/openvas-smb.git
$git clone -b stable --single-branch https://github.com/greenbone/openvas.git
$git clone -b stable --single-branch https://github.com/greenbone/ospd.git
$git clone -b stable --single-branch https://github.com/greenbone/ospd-openvas.git
$git clone -b stable --single-branch https://github.com/greenbone/gvmd.git
$git clone -b stable --single-branch https://github.com/greenbone/gsa.git
$git clone -b stable --single-branch https://github.com/greenbone/gsad.git
```

Abans de començar a instal·lar els diversos mòduls, ens hem d'assegurar-nos d'instal·lar les llibreres de GVM, per poder fer la resta de la instal·lació. Les comandes les executarem com a l'usuari gvm.

```
$cd gvm-libs
$mkdir build && cd build
$cmake ..
$make
$sudo make install
```
---

Seguidament, instal·larem i configurarem l'OpenVAS Scanner i l'OpenVAS SMB.

*Open Vulnerability Assessment Scanner (OpenVAS) is a full-featured scan engine that executes a continuously updated and extended feed of Network Vulnerability Tests (NVTs).

OpenVAS SMB provides modules for the OpenVAS Scanner to interface with Microsoft Windows Systems through the Windows Management Instrumentation API and a winexe binary to execute processes remotely on that system.*



 