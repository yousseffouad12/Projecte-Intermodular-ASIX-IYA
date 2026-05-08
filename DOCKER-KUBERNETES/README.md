<!-- ╔══════════════════════════════════════════════════════════════╗ -->
<!-- ║       DOCKER SWARM & KUBERNETES · INS Sa Palomera          ║ -->
<!-- ║     Izan Ruiz · Youssef Fouad · Adrià Rodríguez            ║ -->
<!-- ╚══════════════════════════════════════════════════════════════╝ -->

<div align="center">

```
███████╗██╗  ██╗ ██████╗ ██████╗ ███╗   ███╗██╗ ██████╗██████╗  ██████╗
██╔════╝██║  ██║██╔═══██╗██╔══██╗████╗ ████║██║██╔════╝██╔══██╗██╔═══██╗
███████╗███████║██║   ██║██████╔╝██╔████╔██║██║██║     ██████╔╝██║   ██║
╚════██║██╔══██║██║   ██║██╔═══╝ ██║╚██╔╝██║██║██║     ██╔══██╗██║   ██║
███████║██║  ██║╚██████╔╝██║     ██║ ╚═╝ ██║██║╚██████╗██║  ██║╚██████╔╝
╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚═╝     ╚═╝     ╚═╝╚═╝ ╚═════╝╚═╝  ╚═╝ ╚═════╝
```

### `Docker Engine v24` · `Docker Swarm` · `Kubernetes (Minikube)` · `Nginx` · `MySQL 8.0` · `PHP` · `Trivy`

![Status](https://img.shields.io/badge/estat-actiu-brightgreen?style=flat-square)
![Docker](https://img.shields.io/badge/Docker-Swarm-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Minikube-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-Proxy-009639?style=flat-square&logo=nginx&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/llicència-MIT-blue?style=flat-square)
![INS](https://img.shields.io/badge/INS_Sa_Palomera-ASIX_2-purple?style=flat-square)

**Projecte Final de Curs 2025–2026 · Mòdul M0487 – Orquestradors**

*Izan Ruiz · Youssef Fouad · Adrià Rodríguez*

</div>

---

## 📋 Índex

- [Sobre el Projecte](#-sobre-el-projecte)
- [Arquitectura General](#-arquitectura-general)
- [Fase 1 — Infraestructura Base i Microserveis](#-fase-1--infraestructura-base-i-microserveis)
- [Fase 2 — Alta Disponibilitat amb Docker Swarm](#-fase-2--alta-disponibilitat-amb-docker-swarm)
- [Fase 3 — Seguretat al Clúster Swarm](#-fase-3--seguretat-al-clúster-swarm)
- [Fase 4 — Migració a Kubernetes](#-fase-4--migració-a-kubernetes)
- [Comparativa Swarm vs Kubernetes](#-comparativa-swarm-vs-kubernetes)
- [Estructura de Fitxers](#-estructura-de-fitxers)
- [Equip](#-equip)

---

## 🚀 Sobre el Projecte

**ShopMicro** és una plataforma d'e-commerce especialitzada en maquinari de xarxa professional, dissenyada i desplegada des de zero amb una arquitectura de **microserveis**. El projecte ha evolucionat en quatre fases, des d'un desplegament bàsic amb Docker Compose fins a una infraestructura orquestrada amb Kubernetes, passant per Docker Swarm i una auditoria de seguretat en profunditat.

> 💡 L'objectiu no és només que la botiga funcioni — és que sigui **resilient, segura i escalable** en un entorn de producció real.

<!--
  📸 FOTO RECOMANADA #1
  Tipus: Diagrama o captura de la pàgina principal de ShopMicro al navegador
  (mostra la botiga amb les cards de productes, el dark mode i el SSL actiu a la barra del navegador)
  Pàgina de referència per buscar-la: Secció 4.2 del document / captura pròpia del projecte
-->

---

## 🏗️ Arquitectura General

L'aplicació separa les responsabilitats en tres capes independents que es comuniquen entre elles a través de xarxes Docker aïllades:

```
  [ Client / Navegador ]
          │  HTTPS (443)
          ▼
  ┌───────────────────┐
  │   Nginx Gateway   │  ◄─── Proxy Invers + Terminació SSL
  │  (frontend-net)   │
  └────────┬──────────┘
           │  HTTP intern
           ▼
  ┌───────────────────┐
  │   PHP App Server  │  ◄─── Lògica de negoci, sessions, carret
  │  (backend-net)    │
  └────────┬──────────┘
           │  TCP 3306
           ▼
  ┌───────────────────┐
  │    MySQL 8.0      │  ◄─── Persistència de dades (users + products)
  │  (backend-net)    │
  └───────────────────┘
```

- **`frontend-net`** (Bridge): aïlla el trànsit entre el proxy i l'exterior.
- **`backend-net`** (Bridge): xarxa privada exclusiva per a comunicació interna.

---

## 📦 Fase 1 — Infraestructura Base i Microserveis

### Entorn i Tecnologies

| Component | Tecnologia |
|---|---|
| Sistema Host | Ubuntu Desktop 22.04 LTS |
| Motor de Contenidors | Docker Engine v24.0.x |
| Orquestració inicial | Docker Compose v2.x |
| Proxy Invers | Nginx (amb SSL/TLS) |
| Backend | PHP |
| Base de Dades | MySQL 8.0 |

### 🔐 Seguretat SSL i DNS Local

S'ha configurat **terminació SSL a Nginx**: les peticions arriben xifrades pel port `443`, i qualsevol accés per HTTP (`80`) es redirigeix automàticament amb un `301`. L'aplicació respon al domini `www.shopmicro.local`, validat amb un certificat SSL propi i configurat al fitxer `/etc/hosts` del client.

### 👤 Gestió d'Usuaris

El sistema d'autenticació garanteix que les dades dels usuaris estiguin protegides en tot moment:

- **Registre**: les contrasenyes es processen amb `password_hash()` de PHP usant l'algorisme **BCRYPT**. Mai es guarda la contrasenya en text pla a MySQL.
- **Login**: la verificació es fa amb `password_verify()`, que compara el hash emmagatzemat amb el que introdueix l'usuari.
- **Sessions**: PHP manté la sessió activa mentre l'usuari navega. El fitxer `logout.php` destrueix la sessió completament amb `session_destroy()` i neteja les cookies del navegador.

### 🛒 Cicle de Compra i Stock

La gestió d'inventari és totalment automàtica i transaccional:

1. **Validació** prèvia de disponibilitat a la base de dades.
2. **`UPDATE` atòmic** a MySQL per restar les unitats venudes del camp `stock`.
3. **Feedback visual** a l'usuari amb confirmació de compra.
4. **Bloqueig automàtic**: quan el `stock` arriba a `0`, el botó es desactiva i mostra *"Sense Stock"*.

<!--
  📸 FOTO RECOMANADA #2
  Tipus: Captura del diagrama de xarxes Docker (frontend-net / backend-net) o del docker-compose.yml
  Pàgina de referència: Secció 2.1 i 6.1 del document / eina: `docker network ls` o VSCode
-->

---

## ⚙️ Fase 2 — Alta Disponibilitat amb Docker Swarm

### Inicialització del Clúster

El clúster s'ha muntat amb **3 màquines virtuals**: un node `Manager` i dos nodes `Worker`.

```bash
# Al node Manager
sudo docker swarm init

# Als nodes Worker (amb el token generat)
docker swarm join --token <TOKEN> <IP_MANAGER>:2377

# Verificació
sudo docker node ls
```

### Adaptació del `docker-stack.yml`

El `docker-compose.yml` de la Fase 1 s'ha transformat en un `docker-stack.yml` amb les següents millores:

| Directiva | Descripció |
|---|---|
| `replicas: 2+` | Mínim 2 rèpliques per als microserveis d'API |
| `constraints: manager` | MySQL i el Gateway s'executen sempre al Manager |
| `restart_policy` | Reinici automàtic si un contenidor cau |
| `update_config` | Rolling update amb `parallelism: 1` per no tallar el servei |

### 🌐 Xarxes Overlay

A diferència de les xarxes `bridge`, les xarxes **overlay** permeten comunicació **xifrada entre contenidors de màquines físiques diferents**. Essencial per a un clúster multi-node.

### 📌 Sticky Sessions amb ip_hash

El carret de la compra es buidava al fer balanceig de càrrega. Solució: directiva `ip_hash` a `nginx.conf`, que fa que cada client sempre arribi a la **mateixa rèplica**, mantenint la sessió activa.

### 🚀 Desplegament de l'Stack

```bash
sudo docker stack deploy -c docker-stack.yml shopmicro
sudo docker stack ps shopmicro
```

### 🔥 Prova de Tolerància a Fallades

S'ha simulat la caiguda d'un node en producció:

```bash
# Simulem la caiguda del Worker-2
sudo systemctl stop docker   # (al worker-2)

# Al Manager, veiem el node com a "Down"
docker node ls

# Swarm redistribueix automàticament les rèpliques
docker stack ps shopmicro

# Recuperació
sudo systemctl start docker  # (al worker-2)
```

> ✅ **Resultat**: les rèpliques del Worker-2 es van redistribuir automàticament al Manager i al Worker-1 en qüestió de segons, sense cap interrupció del servei.

### ↕️ Escalat en Calent

```bash
docker service scale shopmicro_product-service=4
```

<!--
  📸 FOTO RECOMANADA #3
  Tipus: Captura de terminal mostrant `docker node ls` amb els 3 nodes (Manager + 2 Workers) en estat "Ready"
  Pàgina de referència: Secció 7 i 10 del document / captura pròpia de la terminal
-->

---

## 🔒 Fase 3 — Seguretat al Clúster Swarm

### Vulnerabilitats Identificades

Abans d'aplicar millores, s'han detectat dos riscos principals al `docker-stack.yml`:

| Risc | Problema |
|---|---|
| ⚠️ Contrasenyes en text pla | `MYSQL_ROOT_PASSWORD` visible al fitxer YAML |
| ⚠️ Xarxa massa oberta | Tots els serveis compartien visibilitat; risc de moviment lateral |

### 🔑 Docker Secrets

Les contrasenyes s'han migrat a **Docker Secrets**: s'emmagatzemen **xifrades al node Manager** i només s'entreguen en memòria als contenidors que les necessiten.

```bash
# Creació del secret
echo "la_meva_contrasenya_segura" | docker secret create db_password -
```

Al YAML, el servei accedeix al secret via la variable `_FILE`:
```yaml
environment:
  MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_password
secrets:
  - db_password
```

### 🧱 Segmentació de Xarxes (internal: true)

La xarxa `backend-net` s'ha configurat amb `internal: true`, que actua com un **tallafoc intern**: cap contenidor d'aquesta xarxa pot accedir a Internet, ni des de fora es pot accedir-hi directament.

**Prova del "Ping"**: des de dins del contenidor de MySQL s'intenta fer `ping 8.8.8.8` → ❌ **falla**. Demostrat: un atacant que comprometés la BD no podria exfiltrar dades.

### 🛡️ mTLS entre Nodes

Docker Swarm implementa **TLS mutu (mTLS)** per defecte entre nodes. Cada node té el seu certificat digital, gestionat per la CA interna de Swarm amb **rotació automàtica cada 90 dies**.

### 🔍 Escaneig d'Imatges amb Trivy

S'ha realitzat una auditoria de seguretat filtrant per vulnerabilitats `CRITICAL`:

```bash
trivy image --severity CRITICAL mysql:8.0
```

| Camp | Detall |
|---|---|
| **CVE** | CVE-2025-68121 |
| **Biblioteca** | `stdlib` (Go) — `crypto/tls` |
| **Versió afectada** | v1.24.6 |
| **Impacte** | Validació incorrecta de certificats en represa de sessió TLS → possible suplantació de connexions xifrades |

**Mesures aplicades:**
- `docker pull mysql:8.0` per obtenir la darrera revisió amb pedaços de seguretat de Go.
- Migrar a una imatge `mysql:8.0-slim` per eliminar eines auxiliars vulnerables innecessàries.

<!--
  📸 FOTO RECOMANADA #4
  Tipus: Captura de la sortida de Trivy mostrant la vulnerabilitat CVE-2025-68121 en vermell
  Pàgina de referència: Secció 16 del document / captura pròpia de la terminal
-->

---

## ☸️ Fase 4 — Migració a Kubernetes

### Per Què Kubernetes?

Docker Swarm va ser un gran punt de partida, però **Kubernetes aporta un nivell superior de control i automatització**:

- **Self-healing real**: Kubernetes no només reinicia contenidors — comprova la salut real de l'aplicació via HTTP/Port amb `livenessProbe` i `readinessProbe`.
- **Rolling Updates gestionats**: actualitzacions seqüencials garantides sense temps d'inactivitat.
- **Secrets més segurs**: integrats en el cicle de vida del Pod.
- **Escalat intel·ligent**: automàtic i basat en mètriques reals.

### Entorn: Minikube

```bash
minikube start
kubectl get nodes
```

### 📄 Fitxers YAML de la Infraestructura

#### `k8s-pvc.yaml` — Persistència de Dades

Un `PersistentVolumeClaim` d'1Gi garanteix que les dades de MySQL **sobreviuen al reinici dels Pods**. Muntat a `/var/lib/mysql`.

#### `k8s-db.yaml` — Base de Dades

- Imatge: `MySQL 8.0`
- La contrasenya s'injecta des d'un **Secret de Kubernetes** (`db-pass`), mai en text pla.
- Servei intern `db` per a la descoberta de servei interna.

#### `k8s-product.yaml` — Microservei de Productes

El component central de la botiga, configurat per ser totalment autònom:

```yaml
# Auto-configuració dinàmica amb sed
command: ["/bin/sh"]
args:
  - -c
  - |
    sed -i 's/old_host/db/g' /var/www/html/db.php
    sed -i 's/old_pass/$(DB_PASS)/g' /var/www/html/db.php
    apache2-foreground
```

| Paràmetre | Valor |
|---|---|
| Rèpliques | 3 |
| `livenessProbe` | HTTP GET al port 80 — reinicia el Pod si no respon |
| `readinessProbe` | HTTP GET al port 80 — no envia trànsit fins que l'app estigui llesta |
| Exposició | NodePort `30081` |

#### `k8s-gateway.yaml` — Gateway d'Accés

Nginx com a punt d'entrada únic al clúster:

- **NodePort `30080`**: tota la trànsit extern passa per aquí.
- **Proxy Invers**: el client mai connecta directament al microservei.
- **Escalable**: nous microserveis es poden afegir sense obrir nous ports exteriors.

### 🗂️ Namespace Dedicat

```bash
kubectl create namespace shopmicro
```

Tots els recursos estan aïllats dins del namespace `shopmicro`, separats dels serveis per defecte de Kubernetes.

### 🚀 Desplegament Complet

```bash
kubectl apply -f k8s-pvc.yaml -n shopmicro
kubectl apply -f k8s-db.yaml -n shopmicro
kubectl apply -f k8s-product.yaml -n shopmicro
kubectl apply -f k8s-gateway.yaml -n shopmicro

# Verificació
kubectl get pods -n shopmicro
kubectl get services -n shopmicro
```

### 🔄 Rolling Update Automàtic

En modificar la imatge del `product-service`, Kubernetes gestiona la transició de forma transparent:

```
Pod v1 actiu   →   Pod v2 s'inicia   →   Pod v1 es tanca   →   Pod v2 actiu
Pod v1 actiu   →   Pod v2 s'inicia   →   Pod v1 es tanca   →   Pod v2 actiu
Pod v1 actiu   →   Pod v2 s'inicia   →   Pod v1 es tanca   →   Pod v2 actiu
```
> ✅ La botiga **no ha deixat de funcionar en cap moment** durant l'actualització.

### ✅ Verificació Final

```bash
# Accés via port-forward
kubectl port-forward service/api-gateway 8080:80 -n shopmicro
```

Navegant a `http://localhost:8080` s'ha verificat que el catàleg es carrega correctament des de la base de dades persistent.

<!--
  📸 FOTO RECOMANADA #5
  Tipus: Captura de `kubectl get pods -n shopmicro` mostrant tots els pods en estat "Running"
  Pàgina de referència: Secció 21 i 23 del document / captura pròpia de la terminal
-->

---

## ⚖️ Comparativa Swarm vs Kubernetes

| Característica | Docker Swarm | Kubernetes |
|---|:---:|:---:|
| **Corba d'aprenentatge** | 🟢 Baixa | 🔴 Alta |
| **Escalat** | Manual (`docker service scale`) | Automàtic i basat en mètriques |
| **Self-healing** | Reinicia si el procés mor | Comprova la salut real (HTTP/Port) |
| **Rolling Updates** | Configurable al YAML | Natiu i gestionat |
| **Secrets** | Xifrats al Manager | Integrats en el cicle del Pod |
| **Xarxes** | Overlay bàsic | CNI avançat, polítiques de xarxa |
| **Ideal per a** | Projectes petits/mitjans | Producció a gran escala |

---

## 📁 Estructura de Fitxers

```
shopmicro/
│
├── 📄 docker-compose.yml        # Desplegament local (Fase 1)
├── 📄 docker-stack.yml          # Desplegament Swarm (Fase 2 i 3)
│
├── nginx/
│   └── 📄 nginx.conf            # Proxy invers, SSL, ip_hash
│
├── app/
│   ├── 📄 db.php                # Connexió + autoinstal·lació de l'esquema
│   ├── 📄 index.php             # Controlador principal / catàleg
│   ├── 📄 register.php          # Alta d'usuaris (BCRYPT)
│   ├── 📄 login.php             # Autenticació (password_verify)
│   ├── 📄 cart.php              # Transaccions de compra (UPDATE stock)
│   ├── 📄 logout.php            # Destrucció de sessió
│   ├── 📁 images/               # Fotografies dels productes
│   └── 📁 css/
│       └── 📄 style.css         # Dark Mode + Responsive Cards
│
└── kubernetes/
    ├── 📄 k8s-pvc.yaml          # PersistentVolumeClaim (1Gi)
    ├── 📄 k8s-db.yaml           # MySQL 8.0 + Secret + Service
    ├── 📄 k8s-product.yaml      # Microservei productes (3 rèpliques + probes)
    └── 📄 k8s-gateway.yaml      # Nginx Gateway (NodePort 30080)
```

---

## 👥 Equip

<div align="center">

| | Nom | Rol |
|:---:|:---:|:---:|
| 🧑‍💻 | **Izan Ruiz** | Infraestructura i Orquestració |
| 🧑‍💻 | **Youssef Fouad** | Backend, Seguretat i Kubernetes |
| 🧑‍💻 | **Adrià Rodríguez** | Frontend, Networking i Documentació |

**INS Sa Palomera · ASIX – Projecte Final de Curs 2025–2026**

</div>

---

<div align="center">

*Fet amb 🐳 Docker, ☸️ Kubernetes i molt de café.*

</div>
