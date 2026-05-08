<!-- ╔══════════════════════════════════════════════════════════════╗ -->
<!-- ║       ANSIBLE AUTOMATION · INS Sa Palomera                 ║ -->
<!-- ║     Izan Ruiz · Youssef Fouad · Adrià Rodríguez            ║ -->
<!-- ╚══════════════════════════════════════════════════════════════╝ -->

<div align="center">

```
 █████╗ ███╗   ██╗███████╗██╗██████╗ ██╗     ███████╗
██╔══██╗████╗  ██║██╔════╝██║██╔══██╗██║     ██╔════╝
███████║██╔██╗ ██║███████╗██║██████╔╝██║     █████╗  
██╔══██║██║╚██╗██║╚════██║██║██╔══██╗██║     ██╔══╝  
██║  ██║██║ ╚████║███████║██║██████╔╝███████╗███████╗
╚═╝  ╚═╝╚═╝  ╚═══╝╚══════╝╚═╝╚═════╝ ╚══════╝╚══════╝
```

### `Ubuntu Server 24.04` · `Ansible 9.2` · `Docker` · `Nginx` · `UFW` · `Fail2Ban`

![Status](https://img.shields.io/badge/estat-actiu-brightgreen?style=flat-square)
![Ansible](https://img.shields.io/badge/Ansible-9.2.0-red?style=flat-square&logo=ansible)
![Ubuntu](https://img.shields.io/badge/Ubuntu_Server-24.04-orange?style=flat-square&logo=ubuntu)
![Docker](https://img.shields.io/badge/Docker-actiu-blue?style=flat-square&logo=docker)
![Nginx](https://img.shields.io/badge/Nginx-actiu-009639?style=flat-square&logo=nginx)
![License](https://img.shields.io/badge/llicència-MIT-blue?style=flat-square)
![INS](https://img.shields.io/badge/INS_Sa_Palomera-ASIX_2-purple?style=flat-square)

**Projecte Final de Curs 2025–2026 · ASIX – Administració de Sistemes Informàtics en Xarxa**

*Izan Ruiz · Youssef Fouad · Adrià Rodríguez*

</div>

---

## 📋 Índex

- [📖 Introducció](#-introducció)
- [🏗️ Arquitectura de l'entorn](#️-arquitectura-de-lentorn)
- [⚙️ Instal·lació d'Ansible](#️-installació-dansible)
- [🔑 Connectivitat i seguretat SSH](#-connectivitat-i-seguretat-ssh)
- [📁 Configuració tècnica](#-configuració-tècnica)
  - [Inventari](#inventari-etcansiblehosts)
  - [Playbook](#playbook-setup_servidorsyml)
- [🚀 Execució i Validació](#-execució-i-validació)
- [👥 Autors](#-autors)

---

## 📖 Introducció

Aquest projecte desenvolupa un **entorn d'automatització centralitzat** utilitzant Ansible, amb l'objectiu de migrar d'una administració manual i fragmentada cap a un model basat en el concepte d'**Infrastructure as Code (IaC)**.

> 💡 **Per què Ansible?**
> La seva arquitectura **agentless** permet gestionar nodes remots únicament via SSH segur, sense necessitat d'instal·lar cap programari addicional als servidors clients. Tota la configuració s'expressa mitjançant **playbooks YAML**, que actuen com a manuals d'instruccions declaratius i idempotents.

---

## 🏗️ Arquitectura de l'entorn

<!-- 📸 FOTO RECOMANADA: Diagrama/captura de la configuració IP dels nodes (ip a) -->
<!-- Pàgina 4 del document – secció "3. Disseny de l'entorn" -->

```
┌─────────────────────────────────────────────────────────┐
│                    XARXA INTERNA (192.168.1.0/24)        │
│                                                         │
│   ┌──────────────────┐        ┌──────────────────────┐  │
│   │   NODE MESTRE    │  SSH   │   NODE 1 (node1)     │  │
│   │  192.168.1.1     │───────▶│   192.168.1.50       │  │
│   │  Ubuntu Server   │        └──────────────────────┘  │
│   │  Ansible 9.2     │        ┌──────────────────────┐  │
│   │                  │  SSH   │   NODE 2 (node2)     │  │
│   │                  │───────▶│   192.168.1.51       │  │
│   └──────────────────┘        └──────────────────────┘  │
│                                                         │
│   ══════════════════════════════════════════════════    │
│        Interfície NAT (descàrrega paquets/APT)          │
└─────────────────────────────────────────────────────────┘
```

| Rol | Hostname | IP | SO |
|---|---|---|---|
| 🎛️ Node de Control (Mestre) | `ubuntuserver24` | `192.168.1.1` | Ubuntu Server 24.04 |
| 🖥️ Node Gestionat 1 | `node1` | `192.168.1.50` | Ubuntu Desktop 22 |
| 🖥️ Node Gestionat 2 | `node2` | `192.168.1.51` | Ubuntu Desktop 22 |

S'ha configurat una **doble interfície de xarxa** per separar el trànsit:
- **Xarxa interna** → gestió Ansible entre nodes
- **Interfície NAT** → descàrrega d'actualitzacions i paquets des de repositoris oficials

---

## ⚙️ Instal·lació d'Ansible

El procediment s'ha realitzat sobre el **node mestre** (`192.168.1.1`):

```bash
# 1. Actualització del sistema
sudo apt update

# 2. Instal·lació d'Ansible
sudo apt install ansible -y

# 3. Verificació de la versió
ansible --version
```

<!-- 📸 FOTO RECOMANADA: Terminal amb la sortida d'ansible --version i ansible install -->
<!-- Pàgina 3 del document – secció "2. Preparació de l'entorn" -->

> ✅ Versió instal·lada: **Ansible 9.2.0** (core 2.16.3) amb Python 3.12.3

---

## 🔑 Connectivitat i seguretat SSH

Per establir la comunicació entre el mestre i els nodes gestionats, s'ha creat una **relació de confiança mitjançant claus SSH RSA de 4096 bits**.

### Pas 1 – Generació de claus

```bash
ssh-keygen -t rsa -b 4096
```

<!-- 📸 FOTO RECOMANADA: Terminal amb la generació de la clau RSA i el randomart -->
<!-- Pàgina 5 del document – secció "5. Fase de connectivitat i seguretat SSH" -->

### Pas 2 – Distribució de claus als nodes

```bash
ssh-copy-id alumne@192.168.1.50
ssh-copy-id alumne@192.168.1.51
```

### Pas 3 – Verificació de connectivitat

```bash
ansible all -m ping
```

<!-- 📸 FOTO RECOMANADA: Sortida del ping d'Ansible amb SUCCESS als dos nodes -->
<!-- Pàgina 4 del document – secció "4. Gestió de paquets i connectivitat inicial" -->

Resultat esperat:
```
node1 | SUCCESS => { "ping": "pong" }
node2 | SUCCESS => { "ping": "pong" }
```

---

## 📁 Configuració tècnica

### Inventari (`/etc/ansible/hosts`)

L'inventari és el fitxer central on es defineixen i organitzen els nodes gestionats. Permet agrupar servidors i assignar-los variables de connexió específiques per a una gestió totalment **desatesa**.

<!-- 📸 FOTO RECOMANADA: Contingut del fitxer /etc/ansible/hosts al nano -->
<!-- Pàgina 6 del document – secció "6.1 Inventari" -->

```ini
[servidors]
node1 ansible_host=192.168.1.50
node2 ansible_host=192.168.1.51

[servidors:vars]
ansible_user=alumne
ansible_become=yes
ansible_become_method=sudo
ansible_become_password=4Lumn3
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

| Variable | Descripció |
|---|---|
| `ansible_user` | Usuari SSH per connectar-se als nodes |
| `ansible_become` | Activa l'escalada de privilegis |
| `ansible_become_method` | Mètode d'escalada (`sudo`) |
| `ansible_become_password` | Contrasenya per al sudo automatitzat |
| `ansible_ssh_common_args` | Desactiva la verificació estricta de claus |

---

### Playbook (`setup_servidors.yml`)

El playbook és el component central d'Ansible. Defineix l'**estat desitjat** del sistema de forma **idempotent**: només aplica canvis si la configuració actual no coincideix amb la definida.

El nostre playbook s'estructura en **quatre blocs lògics**:

---

#### 🅐 Aprovisionament de l'entorn

Prepara la infraestructura base de tots els nodes:

<!-- 📸 FOTO RECOMANADA: Codi YAML del bloc A (Aprovisionament) al nano/editor -->
<!-- Pàgina 7 del document – captura del playbook secció A -->

```yaml
# Crear grup de seguretat
- name: "Aprov: Crear grup d'administradors"
  group:
    name: admins_projecte
    state: present

# Crear usuari de gestió
- name: "Aprov: Crear usuari de gestió gestor_asix"
  user:
    name: gestor_asix
    group: admins_projecte
    shell: /bin/bash
    create_home: yes

# Crear directori de treball
- name: "Aprov: Crear directori de treball /opt/projecte_asix"
  file:
    path: /opt/projecte_asix
    state: directory
    mode: '0755'
    owner: gestor_asix
    group: admins_projecte

# Instal·lar eines base
- name: "Aprov: Instal·lar paquets base i eines"
  apt:
    name: [nginx, git, curl, ufw, fail2ban]
    state: present
    update_cache: yes
```

| Recurs | Detalls |
|---|---|
| 👥 Grup | `admins_projecte` – millora traçabilitat i evita comptes genèrics |
| 👤 Usuari | `gestor_asix` – usuari de gestió amb home pròpia |
| 📂 Directori | `/opt/projecte_asix` – permisos `0755` |
| 🛠️ Eines base | `git`, `vim`, `curl` – set estàndard a tots els nodes |

---

#### 🅑 Seguretat i Hardening

Aplica polítiques restrictives per blindar els nodes davant accessos no autoritzats:

<!-- 📸 FOTO RECOMANADA: Codi YAML del bloc B (Seguretat/UFW/Hardening SSH) al nano -->
<!-- Pàgina 7-8 del document – captura del playbook secció B -->

```yaml
# Configurar UFW (ports SSH i HTTP)
- name: "Seguretat: Configurar UFW (SSH i HTTP)"
  ufw:
    rule: allow
    port: "{{ item }}"
    proto: tcp
  loop: ['22', '80']

# Activar Firewall
- name: "Seguretat: Activar Firewall"
  ufw:
    state: enabled

# Prohibir login root per SSH
- name: "Seguretat: Hardening SSH (Prohibir Root)"
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^PermitRootLogin'
    line: 'PermitRootLogin no'
  notify: Reiniciar SSH
```

| Mesura | Descripció |
|---|---|
| 🔥 UFW Firewall | Només permet ports `22` (SSH) i `80` (HTTP) |
| 🚫 Root Login | `PermitRootLogin no` al `sshd_config` |
| 🛡️ Fail2Ban | Bloqueja automàticament IPs amb intents fallits |

---

#### 🅒 Desplegament d'Aplicacions (Docker i Nginx)

Automatitza la posada en marxa dels serveis d'aplicació:

<!-- 📸 FOTO RECOMANADA: Codi YAML del bloc C (Docker + Nginx) al nano/editor -->
<!-- Pàgina 8-9 del document – captura del playbook secció C -->

```yaml
# Instal·lar Docker
- name: "Desplegament: Instal·lar el motor Docker"
  apt:
    name: docker.io
    state: present

# Assegurar serveis actius
- name: "Desplegament: Assegurar que Docker i Nginx estiguin actius"
  service:
    name: "{{ item }}"
    state: started
    enabled: yes
  loop: [nginx, docker]

# Desplegar Landing Page
- name: "Desplegament: Crear landing page HTML"
  copy:
    content: "<h1>Servidor ASIX gestionat per Ansible</h1>"
    dest: /var/www/html/index.html
    owner: www-data
    group: www-data
```

---

#### 🅓 Gestió de la Configuració i Manteniment

Garanteix l'estabilitat del sistema i automatitza les tasques repetitives:

<!-- 📸 FOTO RECOMANADA: Codi YAML del bloc D (Cron + Handlers) al nano/editor -->
<!-- Pàgina 9 del document – captura del playbook secció D -->

```yaml
# Tasca Cron de neteja diària
- name: "Manteniment: Tasca diària de neteja /tmp"
  cron:
    name: "Neteja temporals"
    minute: "0"
    hour: "2"
    job: "rm -rf /tmp/*"

# Handler per reiniciar SSH
handlers:
  - name: Reiniciar SSH
    service:
      name: ssh
      state: restarted
```

| Funcionalitat | Detalls |
|---|---|
| ♻️ Bucles (`loop`) | Gestió simultània de múltiples serveis |
| ⏰ Cron | Neteja automàtica de `/tmp/` cada dia a les `02:00h` |
| 🔔 Handlers | Reinici SSH **només** quan es modifica la configuració |

---

## 🚀 Execució i Validació

### Execució del Playbook

```bash
ansible-playbook /etc/ansible/setup_servidors.yml
```

![Captura](IMG/Screenshot_1.png)

<!-- Pàgina 10 del document – secció "7.1 Execució del Playbook" -->

El resum final (`PLAY RECAP`) confirma l'execució correcta:

```
node1   : ok=12  changed=2  unreachable=0  failed=0  skipped=0
node2   : ok=12  changed=2  unreachable=0  failed=0  skipped=0
```

---

### ✅ Verificació remota (node1 – 192.168.1.50)

Un cop executat el playbook, es connecta per SSH al node client per validar l'estat real del sistema:

**Usuaris i grups:**
```bash
id gestor_asix
# uid=1001(gestor_asix) gid=1001(admins_projecte) groups=1001(admins_projecte)

ls -ld /opt/projecte_asix
# drwxr-xr-x 2 gestor_asix admins_projecte 4096 ...
```

<!-- 📸 FOTO RECOMANADA: Sortida dels comandos id gestor_asix i ls -ld /opt/projecte_asix -->
<!-- Pàgina 11 del document – secció "7.2 Verificació remota" -->

**Estat del Firewall (UFW):**
```bash
sudo ufw status
# Status: active  →  22/tcp ALLOW, 80/tcp ALLOW
```

<!-- 📸 FOTO RECOMANADA: Sortida de sudo ufw status amb els ports autoritzats -->
<!-- Pàgina 11 del document – secció "7.2 Verificació remota" -->

**Serveis Docker i Nginx:**
```bash
sudo systemctl status docker nginx
# docker.service: active (running)
# nginx.service:  active (running)
```

<!-- 📸 FOTO RECOMANADA: Sortida de systemctl status docker nginx amb els dos serveis en verd -->
<!-- Pàgina 11 del document – secció "7.2 Verificació remota" -->

**Landing Page desplegada:**
```bash
curl localhost
# <h1>Servidor ASIX gestionat per Ansible</h1>
```

<!-- 📸 FOTO RECOMANADA: Navegador mostrant "Servidor ASIX gestionat per Ansible" a http://192.168.1.50 -->
<!-- Pàgina 12 del document – secció "7.2 Verificació remota" -->

**Hardening SSH:**
```bash
grep "PermitRootLogin" /etc/ssh/sshd_config
# PermitRootLogin no
```

**Cron de manteniment:**
```bash
sudo crontab -l
# 0 2 * * * rm -rf /tmp/*
```

<!-- 📸 FOTO RECOMANADA: Sortida de sudo crontab -l amb les tasques d'Ansible registrades -->
<!-- Pàgina 12 del document – secció "7.2 Verificació remota" -->

---

## 👥 Autors

<div align="center">

| | Nom |
|---|---|
| 👤 | **Izan Ruiz**
| 👤 | **Youssef Fouad**
| 👤 | **Adrià Rodríguez**

**INS Sa Palomera · ASIX · Projecte Final de Curs 2025–2026**

![INS](https://img.shields.io/badge/INS_Sa_Palomera-ASIX-purple?style=for-the-badge)
![Ansible](https://img.shields.io/badge/Powered_by-Ansible-EE0000?style=for-the-badge&logo=ansible)

</div>
