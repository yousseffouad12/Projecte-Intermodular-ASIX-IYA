<!-- ╔══════════════════════════════════════════════════════════════╗ -->
<!-- ║           ZABBIX MONITORING · INS Sa Palomera              ║ -->
<!-- ║     Izan Ruiz · Youssef Fouad · Adrià Rodríguez            ║ -->
<!-- ╚══════════════════════════════════════════════════════════════╝ -->

<div align="center">

```
███████╗ █████╗ ██████╗ ██████╗ ██╗██╗  ██╗    ███╗   ███╗ ██████╗ ███╗   ██╗██╗████████╗ ██████╗ ██████╗ 
╚══███╔╝██╔══██╗██╔══██╗██╔══██╗██║╚██╗██╔╝    ████╗ ████║██╔═══██╗████╗  ██║██║╚══██╔══╝██╔═══██╗██╔══██╗
  ███╔╝ ███████║██████╔╝██████╔╝██║ ╚███╔╝     ██╔████╔██║██║   ██║██╔██╗ ██║██║   ██║   ██║   ██║██████╔╝
 ███╔╝  ██╔══██║██╔══██╗██╔══██╗██║ ██╔██╗     ██║╚██╔╝██║██║   ██║██║╚██╗██║██║   ██║   ██║   ██║██╔══██╗
███████╗██║  ██║██████╔╝██████╔╝██║██╔╝ ██╗    ██║ ╚═╝ ██║╚██████╔╝██║ ╚████║██║   ██║   ╚██████╔╝██║  ██║
╚══════╝╚═╝  ╚═╝╚═════╝ ╚═════╝ ╚═╝╚═╝  ╚═╝    ╚═╝     ╚═╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝   ╚═╝    ╚═════╝ ╚═╝  ╚═╝
```

### `Debian 12` · `Zabbix 7.0` · `pfSense 2.7.2` · `Suricata 7.0.8` · `MariaDB` · `Apache`

![Status](https://img.shields.io/badge/estat-actiu-brightgreen?style=flat-square)
![Zabbix](https://img.shields.io/badge/Zabbix-7.0-red?style=flat-square&logo=zabbix)
![pfSense](https://img.shields.io/badge/pfSense-2.7.2-darkblue?style=flat-square)
![Suricata](https://img.shields.io/badge/Suricata-7.0.8-orange?style=flat-square)
![License](https://img.shields.io/badge/llicència-MIT-blue?style=flat-square)
![INS](https://img.shields.io/badge/INS_Sa_Palomera-ASIX_2-purple?style=flat-square)

**Projecte Final de Curs 2025–2026 · Mòdul M0379 – Miniprojecte**

*Izan Ruiz · Youssef Fouad · Adrià Rodríguez*

</div>

---

## Arquitectura del Sistema

![funcionament](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/cd77904086b3839669c2b5533d789fc6dab57e61/ZABBIX/IMG/infrastructure.svg)

---

## Índex

- [Introducció](#introducció)
- [Stack Tecnològic](#stack-tecnològic)
- [Requisits del Sistema](#requisits-del-sistema)
- [Instal·lació i Configuració](#installació-i-configuració)
- [Monitorització de pfSense i Suricata](#monitorització-de-pfsense-i-suricata)
- [Polítiques i Triggers d'Alerta](#polítiques-i-triggers-dalerta)
- [Integració Gmail + Jira Service Management](#integració-gmail--jira-service-management)
- [Scripts Personalitzats](#scripts-personalitzats)
- [Dashboard](#dashboard)
- [Informes Periòdics](#informes-periòdics)
- [Proves i Validació](#proves-i-validació)
- [Bibliografia](#bibliografia)
- [Autors](#autors)

---

## Introducció

La **monitorització de xarxa** és una part fonamental de l'administració de sistemes moderns. En un entorn professional, no podem dependre de les queixes dels usuaris per detectar que un servei ha fallat — necessitem **visibilitat en temps real i alertes proactives**.

Aquest projecte implementa un sistema complet de monitoratge sobre una infraestructura de laboratori que simula un entorn empresarial real, amb:

- **pfSense** com a firewall/router central amb segmentació WAN/LAN
- **Suricata** com a motor IDS/IPS d'inspecció profunda de paquets (DPI)
- **Zabbix 7.0** com a plataforma integral de monitoratge, alertes i reporting

L'objectiu és la **proactivitat**: recollir mètriques de servidors, dispositius de xarxa i serveis per identificar fallades o colls d'ampolla *abans* que afectin la producció.

---

## Stack Tecnològic

| Component | Versió | Funció |
|-----------|--------|--------|
| **Zabbix Server** | 7.0 LTS | Motor de monitoratge, alertes i dashboard |
| **pfSense** | 2.7.2 | Firewall / Router (WAN · LAN) |
| **Suricata** | 7.0.8 | IDS/IPS · Inspecció de paquets (DPI) |
| **Debian** | 12 "Bookworm" | SO del servidor Zabbix |
| **MariaDB** | 10.11 | Base de dades de Zabbix |
| **Apache** | 2.4 | Frontend web de Zabbix |
| **PHP** | 8.2 | Backend del frontend web |
| **SNMP** | v2c | Protocol de recol·lecció sense agents |
| **Gmail SMTP** | SSL/TLS | Canal de notificació per correu |
| **Jira Service Mgmt** | Cloud | Gestió automàtica d'incidències |

---

## Requisits del Sistema

### Hardware (Màquina Virtual Zabbix)

```
CPU:              4 nuclis virtuals
RAM:              6 GB
Emmagatzematge:   60 GB SSD
SO:               Debian 12 (Bookworm)
Xarxa:            IP estàtica (requisit crític per SNMP i agents)
```

### Hardware (pfSense)

```
Interfícies:  em0 → WAN  |  em1 → LAN (192.168.10.1)
SNMP:         Port UDP 161 · Community: public
SSH:          Port 22 · Autenticació per clau pública RSA
```

---

## Instal·lació i Configuració

### 1. Preparació del Sistema Operatiu

```bash
# Verificació IP estàtica
ip a show enp0s3

# Actualització completa del sistema
sudo apt update && sudo apt upgrade -y
```

### 2. Instal·lació del Stack LAMP

```bash
sudo apt install apache2 mariadb-server mariadb-client \
  php php-gd php-bcmath php-xml php-mbstring \
  php-ldap php-mysql libapache2-mod-php -y
```

### 3. Seguretat de MariaDB

```bash
sudo mysql_secure_installation
# → Canviar contrasenya root
# → Eliminar usuaris anònims
# → Eliminar base de dades de test
```

### 4. Optimització de PHP (`/etc/php/8.2/apache2/php.ini`)

```ini
max_execution_time   = 300
memory_limit         = 128M
post_max_size        = 16M
upload_max_filesize  = 2M
date.timezone        = Europe/Madrid
```

### 5. Creació de la Base de Dades

```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER zabbix@localhost IDENTIFIED BY 'P@ssw0rd';
GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost;
SET GLOBAL log_bin_trust_function_creators = 1;
```

### 6. Instal·lació de Zabbix 7.0

```bash
# Repositori oficial
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_7.0-1+debian12_all.deb
sudo dpkg -i zabbix-release_7.0-1+debian12_all.deb
sudo apt update

# Paquets principals
sudo apt install zabbix-server-mysql zabbix-frontend-php \
  zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y

# Importació de l'esquema inicial
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | \
  mysql --default-character-set=utf8mb4 -u zabbix -p zabbix
```

### 7. Configuració i Activació

```bash
# /etc/zabbix/zabbix_server.conf
DBPassword=P@ssw0rd

# Activació de serveis
sudo systemctl enable --now zabbix-server zabbix-agent apache2
```

Accés al frontend web: `http://<IP_SERVIDOR>/zabbix`

---

## Monitorització de pfSense i Suricata

### Configuració SNMP al pfSense

Al panell d'administració web de pfSense (`Services → SNMP`):

```
SNMP Daemon:     Activat
Polling Port:    161 (UDP)
Community:       public
Mòduls actius:   MibII · Netgraph · PF · Host Resources · UCD · Regex
```

### Alta del dispositiu a Zabbix

```
Ruta:         Data collection → Hosts → Create host
Host name:    pfSense
Host groups:  Templates/Network devices
Plantilla:    PFSense by SNMP   ← conté regles LLD per WAN/LAN
Interfície:   SNMP · 192.168.10.1 · Port 161
Macro:        {$SNMP_COMMUNITY} = public
```

### Paràmetres Monitoritzats

#### Hardware (pfSense + Zabbix Server)

| Paràmetre | Descripció | Llindar crític |
|-----------|------------|----------------|
| **CPU Load** | % ús de tots els nuclis | > 85% durant 5 min |
| **Memory Usage** | RAM consumida | < 10% lliure |
| **Disk Space** | Ocupació de `/` i `/var/log` | > 90% usat |

#### Xarxa (Connectivitat i Amplada de Banda)

| Paràmetre | Interfície | Descripció |
|-----------|------------|------------|
| **Bits rebuts/enviats** | em0 (WAN) · em1 (LAN) | Trànsit en temps real |
| **DHCP server status** | LAN | Servei actiu = 2, Caigut = 0 |
| **DNS server status** | LAN | Servei actiu = 2, Caigut = 0 |
| **ICMP Ping** | pfSense | Disponibilitat del node |

---

## Polítiques i Triggers d'Alerta

### Taula de Llindars

| Paràmetre | Condició | Gravetat | Acció Recomanada |
|-----------|----------|----------|-----------------|
| **CPU Load** | `avg > 85%` durant 5 min | **ALT** | Revisar processos Suricata / atac DoS |
| **RAM Usage** | `< 10%` memòria lliure | **MITJANA** | Optimitzar MariaDB |
| **Disk Space** | `> 90%` espai usat | **ALT** | Netejar `/var/log` |
| **ICMP Ping** | `= 0` (no respon) | **DESASTRE** | Node apagat o tall físic |
| **DNS down** | `= 0` | **AVÍS** | Reiniciar servei DNS |
| **DHCP down** | `= 0` | **AVÍS** | Reiniciar servei DHCP |

### Exemples d'Expressions de Trigger

```
# CPU saturada (sense falsos positius)
avg(/PFSENSE_HARDWARE/system.cpu.load[percpu,avg1],5m) > 5

# RAM crítica
last(/pfsense/vm.memory.size[available]) < 10%

# Node caigut
last(/PFSENSE_HARDWARE/agent.ping) = 0

# Disc ple
last(/PFSENSE_HARDWARE/vfs.fs.dependent.size[/,pused]) > 90
```

---

## Integració Gmail + Jira Service Management

### Flux Complet d'Incident

![flux](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/876647a56846213d40f3a1450f7b9522f2fcaa94/ZABBIX/IMG/alert-pipeline.svg)

### Format de les Alertes Personalitzades

**Problema** — Assumpte:
```
[URGENT-IYA] {EVENT.NAME} en {HOST.NAME}
```

**Resolució** — Assumpte:
```
[SOLUCIONAT-IYA] {EVENT.DURATION}: {EVENT.NAME} en {HOST.NAME}
```

**Cos del missatge (exemple real):**
```
ALERTA CRÍTICA - SISTEMA IYA
==========================================
Problem started at 15:47:45 on 2026.05.02

• Problem name: PFSense: DNS server is not running
• Host: PFSENSE_XARXA
• Severity: Average
• Operational data: Current state: not running (0)
• Original problem ID: 1053
```

---

## Scripts Personalitzats

### Monitor de Suricata via SSH

Script Bash allotjat a `/usr/lib/zabbix/externalscripts/check_suricata.sh`:

```bash
#!/bin/bash
IP_PFSENSE=$1
# Connexió SSH sense interacció (clau pública RSA)
STATUS=$(ssh -o StrictHostKeyChecking=no admin@$IP_PFSENSE \
  "pgrep suricata | wc -l" | xargs)

if   [ "$STATUS" -ge 2 ]; then
  echo "OK: LAN i WAN protegides ($STATUS)"
elif [ "$STATUS" -eq 1 ]; then
  echo "WARNING: Només una interfície protegida"
else
  echo "CRITICAL: Suricata ATURAT"
fi
```

### Configuració a Zabbix

```
Nom item:     Estat Suricata LAN/WAN
Tipus:        Comprovació externa
Clau:         check_suricata.sh[{HOST.IP}]
Tipus info:   Caràcter (retorna text)
Interval:     1 minut
```

### Autenticació SSH sense Contrasenya

```bash
# Generar clau RSA per l'usuari zabbix
sudo -u zabbix ssh-keygen -t rsa
# (sense passphrase — necessari per automatització)

# Copiar clau pública al pfSense
# System → User Manager → admin → Authorized SSH Keys
sudo cat /var/lib/zabbix/.ssh/id_rsa.pub

# Validació
sudo -u zabbix ssh admin@192.168.10.1 "echo 'tot ok'"
# → tot ok
```

---

## Dashboard

El dashboard **ADMIN_ZABBIX** funciona com a centre de control operatiu amb lectura jeràrquica de la infraestructura.

![DASHBOARD](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/d4d25ed99592e92387f8c7cfcfa3d01c2d96432a/ZABBIX/IMG/Screenshot_2.png)

### Ginys implementats

| Giny | Tipus | Funció |
|------|-------|--------|
| **Suricata LAN/WAN** | Valor element | Estat IDS/IPS en temps real |
| **Equips** | Navegador d'equips | Salut global dels nodes |
| **Problemes** | Llista problemes | Alertes actives amb gravetat |
| **pfSense UP/DOWN** | Valor + color | Disponibilitat del firewall |
| **DHCP / DNS** | Valor + color | Serveis crítics (verd=OK, vermell=KO) |
| **Problemes per gravetat** | Resum | Visió ràpida per prioritat |
| **LAN / WAN** | Gràfic clàssic | Trànsit de xarxa en temps real |
| **Correu** | Log accions | Auditoria de notificacions enviades |

---

## Informes Periòdics

Configuració de l'informe programat `INFORME_ZABBIX_IYA`:

```
Nom:         INFORME ZABBIX_IYA
Tauler font: ADMIN_ZABBIX
Període:     Setmana anterior
Cicle:       Setmanalment — Dilluns a les 09:00h
Format:      PDF (amb totes les gràfiques del Dashboard)
Assumpte:    Zabbix: Informe de l'estat de la xarxa (Dilluns)
Destinatari: Admin (Zabbix Administrator) → Gmail + Jira
```

L'administrador rep cada dilluns a les 9h un PDF complet amb l'estat de disponibilitat dels serveis (SLA), les gràfiques de trànsit LAN/WAN de la setmana, el resum de les incidències detectades i resoltes, i l'estat del servei Suricata IDS/IPS.

---

## Proves i Validació

### 13.1. Aturada del servei DNS (DNS Resolver)

Per validar el funcionament complet del sistema, s'ha simulat una fallada real del servei DNS al pfSense. L'objectiu és comprovar que Zabbix detecta el canvi d'estat de xarxa sense necessitat de perdre la connectivitat ICMP completa del dispositiu.

S'ha accedit a la interfície web de gestió del firewall (`Status → Services`) i s'ha aturat manualment el servei **DNS Resolver**.

A la captura següent es pot observar la llista de serveis del pfSense amb el **DNS Resolver** en estat aturat (indicador vermell), mentre la resta de serveis com DHCP i Suricata continuen operatius (indicador verd).

![DNS aturat al pfSense](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/295f881ecc136c682176038365e1b1ea4b08adb6/ZABBIX/IMG/image.png)

---

### 13.2. Detecció per part de Zabbix i activació d'alertes

En menys d'un minut des de l'aturada, el Zabbix ha detectat que el port del servei DNS no responia. El dashboard ha canviat l'estat de l'equip `PFSENSE_XARXA` a **Problem**, mostrant el missatge `PFSense: DNS server is not running` i marcant el giny DNS en vermell.

Simultàniament, el trigger ha activat l'acció configurada i ha iniciat el flux de notificació.

A la captura del dashboard es pot veure el giny **DNS** en color vermell amb l'estat `not running (0)`, el comptador de problemes actius incrementat a 1, i la llista de problemes amb la incidència activa de gravetat **Average** i la marca de temps d'inici.

![Dashboard Zabbix amb alerta DNS](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/b84cce5c6f294751bb6de9f5d929e6887ca8c360/ZABBIX/IMG/image.png)

---

### 13.3. Notificació via Gmail

El sistema de notificacions ha connectat amb els servidors SMTP de Google i ha enviat el correu d'alerta a l'administrador amb l'assumpte personalitzat `[URGENT-IYA]`. Al registre d'accions del Zabbix, l'enviament apareix com a **Enviat**.

La captura mostra el correu rebut a la bústia de l'administrador amb l'assumpte `[URGENT-IYA] PFSense: DNS server is not running en PFSENSE_XARXA`, el cos de missatge amb tots els camps de la incidència (nom, host, severitat, dades operacionals i ID del problema), i la marca horària d'enviament.

![Correu d'alerta rebut a Gmail](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/c713aee0db6387ef62a32b911ee814f878b8fc86/ZABBIX/IMG/image1.png)

---

### 13.4. Creació automàtica del tiquet a Jira Service Management

El Jira ha detectat el nou correu a la bústia d'entrada i ha creat automàticament la sol·licitud sense cap intervenció manual. El tiquet inclou tota la informació proporcionada per Zabbix: nom de l'host, severitat, hora d'inici i ID del problema original.

La captura mostra el tiquet creat a Jira Service Management amb el títol `[URGENT-IYA] PFSense: DNS server is not running en PFSENSE_XARXA`, l'estat inicial **Obert**, la prioritat **Average** heretada de Zabbix, i el cos del tiquet amb tots els detalls de la incidència importats automàticament des del correu.

![Tiquet creat automàticament a Jira](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/8600f2b0f850be68aa5fcb627968d0ae558a6472/ZABBIX/IMG/image2.png)

El tiquet queda assignat i segueix el flux de treball estàndard: **Obert → En curs → Resolt → Tancat**. Totes les accions realitzades (canvis d'estat, comentaris tècnics, evidències) queden registrades dins del tiquet per a futures auditories.

---

### 13.5. Resolució de la incidència i confirmació

Un cop s'ha tornat a activar el servei DNS al pfSense, Zabbix ha detectat la recuperació i ha enviat el correu de resolució amb el prefix `[SOLUCIONAT-IYA]`, incloent la durada total de la incidència. El tiquet a Jira s'ha actualitzat automàticament a l'estat **Resolved**.

La captura del dashboard mostra tots els ginys en verd, el comptador de problemes actius a 0, i el giny **DNS** recuperat a l'estat `running (2)`, confirmant que el sistema ha tornat a la normalitat.

![Dashboard Zabbix sense alertes actives](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/ca28e92580433381bc2234cc26fa1eabaa1a6532/ZABBIX/IMG/image.png)

La captura del correu de resolució mostra l'assumpte `[SOLUCIONAT-IYA] 3m 42s: PFSense: DNS server is not running en PFSENSE_XARXA` amb la durada total de la incidència i la marca horària de recuperació.

![Correu de resolució [SOLUCIONAT-IYA]](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/2c87364642ccbcde132c650a8d28c081941ccf5e/ZABBIX/IMG/image.png)

---

### 13.6. Conclusió i validació del flux end-to-end

```
1. Aturada manual del DNS Resolver al pfSense
         |
         v
2. Zabbix detecta la fallada en menys d'1 minut
   → Dashboard: estat "Problem" en vermell
   → Trigger actiu: "PFSense: DNS server is not running"
         |
         v
3. Acció de notificació activada
   → Gmail: alerta [URGENT-IYA] enviada
   → Jira: tiquet creat automàticament (ZABBIX-N)
         |
         v
4. Servei DNS restaurat al pfSense
         |
         v
5. Zabbix detecta la recuperació
   → Gmail: [SOLUCIONAT-IYA] amb durada de la incidència
   → Tiquet Jira: estat actualitzat a "Resolved"
```

Aquest comportament automatitzat demostra que la plataforma Zabbix compleix correctament amb la seva funció principal: centralitzar la detecció d'incidents, eliminar la dependència de l'avís manual per part dels usuaris i garantir una resposta proactiva, immediata i totalment traçable gràcies a la integració amb el sistema de tiquets.

**Resultat: 100% del flux validat correctament.**

## Autors

<div align="center">

| | Autor |
|---|---|
| `ASIX2` | **Izan Ruiz** |
| `ASIX2` | **Youssef Fouad** |
| `ASIX2` | **Adrià Rodríguez** |

**INS Sa Palomera · ASIX 2 · Curs 2025–2026**

Mòdul: `M0379 - Miniprojecte`

</div>

---

<div align="center">

`ACTIVE` · `MONITORING` · `ALERTING` · `REPORTING`

*Zabbix 7.0 · pfSense 2.7.2 · Suricata 7.0.8 · ET Open Rules*

</div>
