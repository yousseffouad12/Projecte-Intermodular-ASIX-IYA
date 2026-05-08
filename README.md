# 🖥️ IYA PROJECT – ASIX
### Projecte Intermodular Final · INS Sa Palomera · Curs 2025–2026

> Infraestructura robusta, escalable i segura dissenyada i implementada per estudiants de 2n curs d'ASIX.

---

## 👥 Equip

| Membre | Rol | Portfoli |
|--------|-----|---------|
| **Izan Ruiz Pérez** | Sistemes & Virtualització | [izaanruiz.github.io/Portfoli](https://izaanruiz.github.io/Portfoli/) |
| **Youssef Fouad Mabrouki** | Seguretat & Monitorització | [yousseffouad12.github.io/youssef_cv](https://yousseffouad12.github.io/youssef_cv/) |
| **Adrià Rodríguez Estrella** | Contenidors & Automatització | [adriiiii.github.io/cv-estrella](https://adriiiii.github.io/cv-estrella/) |

**Tutors:** Francesc Barragan · José Moreno · Joan Pou · Isaac Pulí · Josep Catà

---

## 📋 Descripció del projecte

L'**IYA_PROJECT** neix amb l'objectiu de dissenyar i implementar una infraestructura de sistemes i xarxes **robusta, escalable i segura**. El projecte demostra la integració de tecnologies d'orquestració de contenidors, sistemes de detecció d'intrusos (IDS/IPS) i monitorització avançada, tot gestionat amb metodologia Agile.

### Àrees d'actuació

- 🖥️ **Virtualització** – Entorn Proxmox VE amb emmagatzematge TrueNAS (ZFS/RAID 1)
- 🐳 **Orquestració** – Clústers Docker Swarm i Kubernetes (Minikube)
- 🛡️ **Ciberseguretat** – IDS/IPS amb Suricata integrat a pfSense
- 📊 **Monitorització** – Supervisió centralitzada amb Zabbix 7.0 via SNMP
- ⚙️ **Automatització** – Desplegament d'infraestructura amb Ansible

---

## 🚀 Estat del projecte

> 🟡 **En desenvolupament** · Data d'entrega: 08 de Maig del 2026

### Sprints (Metodologia Agile)

| Sprint | Objectiu | Estat |
|--------|----------|-------|
| **Sprint 1** – Preparació | Base documental i repositoris | ✅ Completat |
| **Sprint 2** – Core | Hipervisor, clúster i automatització | ✅ Completat |
| **Sprint 3** – Serveis | IDS/IPS i monitorització | ✅ Completat |
| **Sprint 4** – Tancament | Memòria tècnica i validació | 🔄 En procés |

---

## 🔧 Mòduls tècnics

### 1. Automatització amb Ansible
- Arquitectura **Agentless** (SSH + claus públiques)
- Playbooks YAML: creació d'usuaris, hardening UFW, desplegament de Docker i Nginx
- Validació d'**idempotència** (`changed=0` en segona execució)
- Ports oberts: `22/tcp`, `80/tcp`, `443/tcp`

### 2. Entorn virtual Proxmox
- **Hipervisor:** Proxmox VE (basat en Debian, llicència GPL)
- **Emmagatzematge:** TrueNAS SCALE amb pool en Mirror RAID 1 (2× 50 GiB)
- **Xarxa segmentada:** Host-Only (gestió), Xarxa interna (NFS), NAT (internet)
- Compartició de recursos via **NFS** (IP interna `10.0.0.2`)

### 3. IDS/IPS amb Suricata
- Motor **multithreading** amb Inspecció Profunda de Paquets (DPI)
- Mode **IPS Inline** via driver Netmap
- Regles actives: `ET Open (Emerging Threats)` – Scan, Web Server, Malware
- Alertes en temps real via **bot de Telegram** (format EVE JSON)
- Complement: **pfBlockerNG** per filtratge DNS/dominis de phishing

**Proves de penetració superades:**
- ✅ Nmap Null/Xmas Scan → bloquejat
- ✅ SQL Injection (sqlmap) → tallat en mil·lisegons
- ✅ Notificació Telegram en < 1 segon

### 4. Docker Swarm i Kubernetes
- Aplicació d'**e-commerce** en PHP + MySQL (`SHOPmicro PRO`)
- Seguretat: **BCRYPT** per contrasenyes, **HTTPS** amb Nginx com a Proxy Invers
- Docker Swarm: 1 Manager + 2 Workers, fitxer `docker-stack.yml`
- Kubernetes: Minikube amb PVC, readiness/liveness probes, NodePort
- Auditoria d'imatges amb **Trivy**

### 5. Monitorització Zabbix
- Servidor **Zabbix 7.0** sobre Debian 12 + MariaDB
- Recollida via **SNMP v2c** des de pfSense (sense agents externs)
- Triggers: alerta si CPU > 85% durant 5 minuts sostiguts
- Integració: **Gmail SMTP** → **Jira Service Management** (tiquets automàtics)
- Detecció de caiguda de serveis en **< 60 segons**

---

## 🛠️ Eines i tecnologies

| Categoria | Eines |
|-----------|-------|
| Gestió de projecte | Jira (Agile), GitHub, Google Drive |
| Virtualització | Proxmox VE, TrueNAS SCALE, VirtualBox |
| Contenidors | Docker, Docker Swarm, Kubernetes (Minikube) |
| Seguretat | pfSense, Suricata, pfBlockerNG, Wazuh |
| Automatització | Ansible, YAML Playbooks |
| Monitorització | Zabbix 7.0, SNMP, MariaDB |
| Pentesting | Kali Linux, Nmap, sqlmap, Trivy |
| Notificacions | Telegram Bot API, Gmail SMTP |

---

## 🔗 Enllaços del projecte

- 📁 [GitHub](#)
- 📋 [Jira](#)
- 📂 [Drive](#)
- 🐳 [Projecte Docker](#)
- ⚙️ [Automatització de la configuració](#)
- 🛡️ [Implementació del sistema de detecció](#)
- 📊 [Monitorització de xarxa](#)
- 🖥️ [Creació d'entorn virtual](#)

---

## 📚 Informació acadèmica

| Camp | Detall |
|------|--------|
| Cicle | Grau Superior – ASIX |
| Centre | Institut Sa Palomera |
| Població | Blanes, Girona |
| Curs | 2025–2026 |

---

*IYA Project · INS Sa Palomera · ASIX 2025–2026*
