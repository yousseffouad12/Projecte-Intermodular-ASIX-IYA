# 🛡️ Implementació d'IDS/IPS amb Suricata sobre pfSense

Aquest repositori conté la documentació, configuració i resultats del projecte final d'implementació d'un **Sistema de Detecció i Prevenció d'Intrusions (IDS/IPS)** basat en **Suricata** actuant sobre un tallafocs **pfSense**.

El projecte demostra l'enduriment d'una xarxa corporativa, la resposta davant d'atacs reals simulats (Pentesting), la configuració de logs avançats (EVE JSON), el filtratge DNS i l'automatització d'alertes mitjançant l'API de Telegram.

---

## 📑 Índex

1. [Arquitectura del Projecte](#-arquitectura-del-projecte)
2. [Tecnologies Utilitzades](#-tecnologies-utilitzades)
3. [Característiques Principals](#-característiques-principals)
4. [Auditoria i Pentesting (Casos d'Ús)](#-auditoria-i-pentesting-casos-dús)
5. [Automatització i Sistemes Complementaris](#-automatització-i-sistemes-complementaris)
6. [Optimització de Rendiment](#-optimització-de-rendiment)
7. [Autors](#-autors)

---

## 🏗️ Arquitectura del Projecte

L'entorn s'ha desplegat mitjançant màquines virtuals (VirtualBox) simulant una infraestructura de xarxa perimetral real:

*   **Tallafocs/Router:** pfSense (Virtualitzat amb 4 Cores, 8GB RAM).
*   **Gestió d'Emmagatzematge:** Configuració de doble disc dur físic separat (un per a l'OS, un dedicat exclusivament a la partició `/var/log` per evitar colls d'ampolla amb els logs de l'IDS).
*   **Interfícies de Xarxa:**
    *   `WAN` (Adaptador Pont): Exposada a Internet i atacs externs.
    *   `LAN` (Xarxa Interna): Xarxa privada protegida (192.168.10.x).

> ![Configuració de les interfícies]([INSERIR CAPTURA: Assignació d'IPs a la terminal inicial de pfSense - Pàgina 12])

---

## 🛠️ Tecnologies Utilitzades

*   **Core:** pfSense, Suricata (Motor IDS/IPS multfil).
*   **Alerting:** PHP, API de Telegram, Cron.
*   **Filtratge DNS:** pfBlockerNG (DNSBL).
*   **Auditoria Offensive (Kali Linux):** Nmap, Nikto, Hydra, DIRB, hping3, Sqlmap, WAFW00F.

---

## 🚀 Característiques Principals

### 1. Mode IPS i Ajustaments de Maquinari
S'ha deshabilitat l'acceleració per maquinari de la targeta de xarxa (`Disable hardware checksum offload`) forçant el trànsit a passar per la CPU. Això permet a Suricata realitzar Inspecció Profunda de Paquet (DPI) i actuar en mode actiu (Legacy Mode), tallant les connexions i inserint els atacants a la taula de bloqueig (`pf table`) en temps real.

### 2. Polítiques de Seguretat i Signatures
El motor utilitza les llistes intel·ligents **Emerging Threats (ET) Open**, combinades amb un conjunt de **regles personalitzades** creades específicament per detectar trànsit anòmal intern, mineria de criptomonedes, protocols insegurs (Telnet) i atacs ICMP de gran tamany.

### 3. Generació de Logs Estructurats (EVE JSON)
Es prescindeix del clàssic format de text pla (`fast.log`) per fer un salt qualitatiu a **EVE JSON**. Això permet estructurar els esdeveniments en format clau-valor, extraient informació detallada fins a la Capa 7 (Aplicació), preparant el terreny per a una integració directa amb solucions SIEM (ex. Elastic o Zabbix).

> ![Mostra EVE JSON]([INSERIR CAPTURA: Codi de la terminal llegint el format EVE JSON amb la comanda tail - Pàgina 20])

---

## ⚔️ Auditoria i Pentesting (Casos d'Ús)

S'ha realitzat una bateria de proves ofensives des d'un equip **Kali Linux** tant en la zona LAN (intent de moviment lateral) com a la WAN (atac perimetral) per validar la resposta de Suricata.

| Vector d'Atac | Eina Utilitzada | Resposta del Sistema (IPS) |
| :--- | :--- | :--- |
| **Reconeixement / Escaneig** | `Nmap` (NULL Scan) | Bloqueig per la signatura *ET SCAN Nmap User-Agent* |
| **Vulnerabilitats Web** | `Nikto` / `DIRB` | Detecció d'anomalies HTTP (HTTP Host header ambiguous) |
| **Denegació de Servei (DoS)** | `hping3` (SYN Flood) | Drop automàtic per *SURICATA STREAM excessive retransmissions* |
| **Força Bruta (SSH)** | `Hydra` | Intercepció en segons per *ET SCAN Potential SSH Scan* i límit de *threshold* |
| **Injecció SQL (SQLi)** | `sqlmap` (Capa 7) | Bloqueig de payload gràcies al DPI (*ET WEB_SERVER SQL Injection*) |
| **Fingerprinting** | `wafw00f` | Identificació del servidor rere un WAF actiu |

> ![Atac i Bloqueig]([INSERIR CAPTURA: Terminal del Kali Linux sent tallat per l'atac Sqlmap mostrant l'avís vermell CRITICAL WAF/IPS - Pàgina 35])

---

## 🤖 Automatització i Sistemes Complementaris

### Notificacions Crítiques via Telegram
Un sistema d'IDS no és eficaç sense una alerta primerenca. S'ha desenvolupat un script en **PHP** (allotjat a `/root/suricata_telegram.php`) que llegeix l'última línia dels logs, controla la duplicitat de les alertes i executa una petició `cURL` xifrada cap a l'**API d'un bot de Telegram**. Aquest procés està automatitzat a pfSense mitjançant un servei **Cron** que s'executa cada minut.

> ![Alerta mòbil]([INSERIR CAPTURA: Mòbil o aplicació de Telegram rebent els avisos [wDrop] SURICATA ALERT - Pàgina 41])

### Filtratge de Navegació i DNS Sinkholing (pfBlockerNG)
A banda del IDS, s'ha afegit una capa de protecció addicional mitjançant el paquet **pfBlockerNG**. Actuant com a DNS Sinkhole, redirigeix les peticions de dominis llistats a bases de dades de *malware* o llistes personalitzades a una Virtual IP de bloqueig, protegint els clients de la xarxa local abans que la comunicació surti a Internet.

> ![Bloqueig web]([INSERIR CAPTURA: Pàgina de bloqueig al navegador "Warning: Potential Security Risk Ahead" - Pàgina 46])

---

## ⚙️ Optimització de Rendiment

En entorns de producció, l'anàlisi profunda de paquets pot saturar la memòria RAM. Per evitar-ho s'han aplicat les següents mesures d'optimització (Tuning):
*   **Ajust del motor de detecció:** Configuració en mode `AutoFP` per a un millor aprofitament dels fils del processador virtual.
*   **Limitació de Flow Memory:** Limitació estricta de la memòria dedicada a la desfragmentació de paquets i seguiment de connexions.
*   **Selecció intel·ligent de regles:** Activació exclusiva dels conjunts de regles aplicables a la xarxa (ex. descartant regles industrials DNP3 si no s'apliquen), descarregant dràsticament l'ús de la CPU.

> ![Configuració Tuning]([INSERIR CAPTURA: Taula de regles amb algunes caselles marcades i d'altres desmarcades - Pàgina 48])

---

## 👥 Autors i Context Acadèmic

Aquest projecte correspon al Mòdul **M0379 - Miniprojectes** del Grau Superior d'Administració de Sistemes Informàtics en Xarxa (ASIX 2).

*   **Autors:** Izan Ruiz, Youssef Fouad, Adrià Rodríguez.
*   **Institut:** INS Sa Palomera.
*   **Curs Acadèmic:** 2023 - 2024.

*Aquesta documentació té finalitats purament acadèmiques i educatives en l'àmbit de la ciberseguretat.*
