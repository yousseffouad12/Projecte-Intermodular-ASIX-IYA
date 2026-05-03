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

![Status](https://img.shields.io/badge/estat-✅_actiu-brightgreen?style=flat-square)
![Zabbix](https://img.shields.io/badge/Zabbix-7.0-red?style=flat-square&logo=zabbix)
![pfSense](https://img.shields.io/badge/pfSense-2.7.2-darkblue?style=flat-square)
![Suricata](https://img.shields.io/badge/Suricata-7.0.8-orange?style=flat-square)
![License](https://img.shields.io/badge/llicència-MIT-blue?style=flat-square)
![INS](https://img.shields.io/badge/INS_Sa_Palomera-ASIX_2-purple?style=flat-square)

**Projecte Final de Curs 2025–2026 · Mòdul M0379 – Miniprojecte**

*Izan Ruiz · Youssef Fouad · Adrià Rodríguez*

</div>

---

## 📡 Arquitectura del Sistema

```
                    ┌─────────────────────────────────────────────────────┐
                    │              INFRAESTRUCTURA DE LABORATORI           │
                    │                                                      │
  ┌──────────┐      │   ┌──────────────┐      ┌──────────────────────┐   │
  │ INTERNET │──────│──▶│   pfSense    │─────▶│   XARXA LAN          │   │
  │          │  WAN │   │   2.7.2      │ LAN  │   192.168.10.x/24    │   │
  └──────────┘      │   │ + Suricata   │      └──────────────────────┘   │
                    │   │   IDS/IPS    │               │                  │
                    │   └──────────────┘               │                  │
                    │          │ SNMP/SSH               ▼                  │
                    │          │               ┌──────────────────────┐   │
                    │          └──────────────▶│  ZABBIX SERVER       │   │
                    │                          │  Debian 12           │   │
                    │                          │  192.168.10.x        │   │
                    │                          │  + MariaDB + Apache  │   │
                    │                          └──────────────────────┘   │
                    │                                   │                  │
                    │                          ┌────────▼───────────┐     │
                    │                          │  NOTIFICACIONS     │     │
                    │                          │  Gmail · Jira      │     │
                    │                          └────────────────────┘     │
                    └─────────────────────────────────────────────────────┘
```

---

## 🗂️ Índex

- [Introducció](#-introducció)
- [Stack Tecnològic](#-stack-tecnològic)
- [Requisits del Sistema](#-requisits-del-sistema)
- [Instal·lació i Configuració](#-installació-i-configuració)
- [Monitorització de pfSense i Suricata](#-monitorització-de-pfsense-i-suricata)
- [Polítiques i Triggers d'Alerta](#-polítiques-i-triggers-dalerta)
- [Integració Gmail + Jira Service Management](#-integració-gmail--jira-service-management)
- [Scripts Personalitzats](#-scripts-personalitzats)
- [Dashboard](#-dashboard)
- [Informes Periòdics](#-informes-periòdics)
- [Autors](#-autors)

---

## 🧭 Introducció

La **monitorització de xarxa** és una part fonamental de l'administració de sistemes moderns. En un entorn professional, no podem dependre de les queixes dels usuaris per detectar que un servei ha fallat — necessitem **visibilitat en temps real i alertes proactives**.

Aquest projecte implementa un sistema complet de monitoratge sobre una infraestructura de laboratori que simula un entorn empresarial real, amb:

- **pfSense** com a firewall/router central amb segmentació WAN/LAN
- **Suricata** com a motor IDS/IPS d'inspecció profunda de paquets (DPI)
- **Zabbix 7.0** com a plataforma integral de monitoratge, alertes i reporting

L'objectiu és la **proactivitat**: recollir mètriques de servidors, dispositius de xarxa i serveis per identificar fallades o colls d'ampolla *abans* que afectin la producció.

---

## 🛠️ Stack Tecnològic

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

## 💻 Requisits del Sistema

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

## 🚀 Instal·lació i Configuració

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

## 🔥 Monitorització de pfSense i Suricata

### Configuració SNMP al pfSense

Al panell d'administració web de pfSense (`Services → SNMP`):

```
SNMP Daemon:     ✅ Activat
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

#### 🖥️ Hardware (pfSense + Zabbix Server)

| Paràmetre | Descripció | Llindar crític |
|-----------|------------|----------------|
| **CPU Load** | % ús de tots els nuclis | > 85% durant 5 min |
| **Memory Usage** | RAM consumida | < 10% lliure |
| **Disk Space** | Ocupació de `/` i `/var/log` | > 90% usat |

#### 🌐 Xarxa (Connectivitat i Amplada de Banda)

| Paràmetre | Interfície | Descripció |
|-----------|------------|------------|
| **Bits rebuts/enviats** | em0 (WAN) · em1 (LAN) | Trànsit en temps real |
| **DHCP server status** | LAN | Servei actiu = 2, Caigut = 0 |
| **DNS server status** | LAN | Servei actiu = 2, Caigut = 0 |
| **ICMP Ping** | pfSense | Disponibilitat del node |

---

## 🚨 Polítiques i Triggers d'Alerta

### Taula de Llindars

| Paràmetre | Condició | Gravetat | Acció Recomanada |
|-----------|----------|----------|-----------------|
| **CPU Load** | `avg > 85%` durant 5 min | 🔴 **ALT** | Revisar processos Suricata / atac DoS |
| **RAM Usage** | `< 10%` memòria lliure | 🟡 **MITJANA** | Optimitzar MariaDB |
| **Disk Space** | `> 90%` espai usat | 🔴 **ALT** | Netejar `/var/log` |
| **ICMP Ping** | `= 0` (no respon) | 💥 **DESASTRE** | Node apagat o tall físic |
| **DNS down** | `= 0` | 🟠 **AVÍS** | Reiniciar servei DNS |
| **DHCP down** | `= 0` | 🟠 **AVÍS** | Reiniciar servei DHCP |

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

## 📧 Integració Gmail + Jira Service Management

### Flux Complet d'Incident
```
![flux](https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/876647a56846213d40f3a1450f7b9522f2fcaa94/ZABBIX/IMG/alert-pipeline.svg)
```

### Format de les Alertes Personalitzades

**Problema** — Assumpte:
```
[URGENT-IYA] 🔴 {EVENT.NAME} en {HOST.NAME}
```

**Resolució** — Assumpte:
```
[SOLUCIONAT-IYA] ✅ {EVENT.DURATION}: {EVENT.NAME} en {HOST.NAME}
```

**Cos del missatge (exemple real):**
```
🔴 ALERTA CRÍTICA - SISTEMA IYA
==========================================
Problem started at 15:47:45 on 2026.05.02

• Problem name: PFSense: DNS server is not running
• Host: PFSENSE_XARXA
• Severity: Average
• Operational data: Current state: not running (0)
• Original problem ID: 1053
```

---

## 🤖 Scripts Personalitzats

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
# → tot ok  ✅
```

---

## 📊 Dashboard

El dashboard **ADMIN_ZABBIX** funciona com a **Centre de Control Operatiu** amb lectura jeràrquica:

```
![DASHBOARD]https://github.com/yousseffouad12/Projecte-Intermodular-ASIX-IYA/blob/d4d25ed99592e92387f8c7cfcfa3d01c2d96432a/ZABBIX/IMG/Screenshot_2.png)
```

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

## 📑 Informes Periòdics

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

L'administrador rep cada dilluns a les 9h un PDF complet amb:
- Estat de disponibilitat dels serveis (SLA)
- Gràfiques de trànsit LAN/WAN de la setmana
- Resum de les incidències detectades i resoltes
- Estat del servei Suricata IDS/IPS

---

## 🔬 Prova de Validació End-to-End

```
1. Aturada manual del servei DNS al pfSense
         │
         ▼
2. Zabbix detecta port DNS no respon (<1 min)
   → Dashboard: estat "Problem" en vermell
   → Missatge: "PFSense: DNS server is not running"
         │
         ▼
3. Trigger activa l'Acció configurada
   → Gmail envia alerta [URGENT-IYA] 🔴
   → Jira crea tiquet automàticament (ZABBIX-N)
         │
         ▼
4. Servei DNS restaurat al pfSense
         │
         ▼
5. Zabbix detecta recuperació
   → Gmail envia [SOLUCIONAT-IYA] ✅ (amb durada: 7m 0s)
   → Tiquet Jira actualitzat → "Resolved"
```

**Resultat: 100% del flux validat ✅**

---

## 👥 Autors

<div align="center">

| | Nom | Rol |
|-|-----|-----|
| 🧑‍💻 | **Izan Ruiz** | Infraestructura, pfSense, Scripts SSH |
| 🧑‍💻 | **Youssef Fouad** | Zabbix, Notificacions, Jira Integration |
| 🧑‍💻 | **Adrià Rodríguez** | Dashboard, Triggers, Documentació |

**INS Sa Palomera · ASIX 2 · Curs 2025–2026**

Mòdul: `M0379 - Miniprojecte`

</div>

---

<div align="center">

```
● ACTIVE    ● MONITORING    ● ALERTING    ● REPORTING
```

*Zabbix 7.0 · pfSense 2.7.2 · Suricata 7.0.8 · ET Open Rules · Maintained: YES*

</div>
