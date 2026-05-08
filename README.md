<div align="center">

# IYA PROJECT

### Administració de Sistemes Informàtics en Xarxa
**INS Sa Palomera · Blanes, Girona · Curs 2025–2026**

[![Estado](https://img.shields.io/badge/estat-en%20desenvolupament-yellow?style=flat-square)](.)
[![Cicle](https://img.shields.io/badge/cicle-ASIX%20Grau%20Superior-blue?style=flat-square)](.)
[![Llicència](https://img.shields.io/badge/llicència-acadèmica-lightgrey?style=flat-square)](.)

</div>

---

## Sobre el projecte

L'**IYA_PROJECT** és el projecte final integrador del Cicle Formatiu de Grau Superior d'ASIX. L'objectiu és dissenyar, implementar i documentar una infraestructura de sistemes i xarxes **robusta, escalable i segura**, integrant les principals tecnologies del sector: virtualització, orquestració de contenidors, ciberseguretat perimetral, automatització i monitorització avançada.

Tot el projecte s'ha gestionat amb **metodologia Agile (Scrum)**, fent servir Jira per al seguiment de tasques i GitHub com a eix central de codi i documentació.

---

## Equip

<table>
  <tr>
    <td align="center">
      <b>Izan Ruiz Pérez</b><br/>
      <a href="https://izaanruiz.github.io/Portfoli/">🌐 Portfoli</a>
    </td>
    <td align="center">
      <b>Youssef Fouad Mabrouki</b><br/>
      <a href="https://yousseffouad12.github.io/youssef_cv/">🌐 Portfoli</a>
    </td>
    <td align="center">
      <b>Adrià Rodríguez Estrella</b><br/>
      <a href="https://adriiiii.github.io/cv-estrella/">🌐 Portfoli</a>
    </td>
  </tr>
</table>

> **Tutors:** Francesc Barragan · José Moreno · Joan Pou · Isaac Pulí · Josep Catà

---

## Mòduls del projecte

### ⚙️ Automatització amb Ansible
Desplegament complet de la infraestructura mitjançant Playbooks YAML. S'han automatitzat la gestió d'usuaris, el hardening del sistema (UFW), la instal·lació de Docker i el desplegament d'Nginx. Validat el principi d'idempotència en entorn multi-node.

### 🖥️ Entorn virtual amb Proxmox
Implementació d'un hipervisor Proxmox VE connectat a un servidor d'emmagatzematge TrueNAS SCALE. El pool de dades utilitza ZFS en mode Mirror (RAID 1) per garantir la tolerància a fallades. La comunicació entre serveis es realitza via NFS sobre xarxa interna dedicada.

### 🛡️ Sistema IDS/IPS amb Suricata
Infraestructura de defensa profunda integrada a pfSense. Suricata opera en **Mode IPS Inline** amb Inspecció Profunda de Paquets (DPI) i regles ET Open (Emerging Threats). S'ha implementat un sistema d'alertes automàtiques via **bot de Telegram** (format EVE JSON). Complement de filtratge DNS amb pfBlockerNG.

**Proves de penetració superades:**
- Nmap Null/Xmas Scan → detectat i bloquejat
- SQL Injection via sqlmap → tallat en mil·lisegons
- Notificació Telegram en menys d'1 segon post-atac

### 🐳 Orquestració amb Docker Swarm i Kubernetes
Aplicació d'e-commerce (`SHOPmicro PRO`) en PHP + MySQL desplegada en microserveis. Evolució des de Docker Compose fins a un clúster Swarm (1 Manager + 2 Workers) i posterior migració a Kubernetes amb Minikube. S'han validat alta disponibilitat, escalat en calent i auditoria d'imatges amb **Trivy**.

### 📊 Monitorització amb Zabbix
Sistema de monitorització centralitzat amb Zabbix 7.0 sobre Debian 12 + MariaDB. Recollida de mètriques via **SNMP v2c** des de pfSense sense necessitat d'agents. Triggers configurats amb llindar sostigut (CPU > 85% durant 5 min). Integració amb **Gmail SMTP** i **Jira Service Management** per a la generació automàtica de tiquets d'incidència.

---

## Tecnologies

| Àrea | Stack |
|------|-------|
| Virtualització | Proxmox VE · TrueNAS SCALE · ZFS · VirtualBox |
| Contenidors | Docker · Docker Swarm · Kubernetes · Minikube |
| Seguretat | pfSense · Suricata · pfBlockerNG · Kali Linux |
| Automatització | Ansible · YAML · SSH |
| Monitorització | Zabbix 7.0 · SNMP · MariaDB · Apache |
| Gestió | Jira · GitHub · Google Drive |
| Notificacions | Telegram Bot API · Gmail SMTP |

---

## Planificació

| Sprint | Objectiu | Estat |
|--------|----------|-------|
| Sprint 1 – Preparació | Base documental, repositoris i plantilles | ✅ Completat |
| Sprint 2 – Core | Hipervisor, clúster de contenidors i automatització | ✅ Completat |
| Sprint 3 – Serveis | IDS/IPS i monitorització de xarxa | ✅ Completat |
| Sprint 4 – Tancament | Memòria tècnica i validació final | 🔄 En procés |

---

## Enllaços

| Recurs | Enllaç |
|--------|--------|
| Codi font | [GitHub](#) |
| Gestió de tasques | [Jira](#) |
| Documentació | [Google Drive](#) |
| Projecte Docker | [Veure](#) |
| Automatització | [Veure](#) |
| Sistema de detecció | [Veure](#) |
| Monitorització | [Veure](#) |
| Entorn virtual | [Veure](#) |

---

<div align="center">
  <sub>IYA Project · INS Sa Palomera · ASIX 2025–2026</sub>
</div>
