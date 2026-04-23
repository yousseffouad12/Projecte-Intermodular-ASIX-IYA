<!-- ============================================================ -->
<!--   IDS/IPS SURICATA — README VISUAL                          -->
<!-- ============================================================ -->

<div align="center">

```
██╗██████╗ ███████╗    ██╗██████╗ ███████╗
██║██╔══██╗██╔════╝    ██║██╔══██╗██╔════╝
██║██║  ██║███████╗    ██║██████╔╝███████╗
██║██║  ██║╚════██║    ██║██╔═══╝ ╚════██║
██║██████╔╝███████║    ██║██║     ███████║
╚═╝╚═════╝ ╚══════╝    ╚═╝╚═╝     ╚══════╝
         SURICATA  ·  pfSense  ·  Kali Linux
```

</div>

---

<!-- HERO SVG 1 — Perimeter Architecture -->
<div align="center">

```svg
<!-- Network Perimeter Diagram — paste this in a renderer that supports inline SVG -->
```

</div>

<!-- HERO SVG: NETWORK PERIMETER -->
<p align="center">
<svg width="780" height="160" viewBox="0 0 780 160" xmlns="http://www.w3.org/2000/svg" style="background:#0a0e14;border:1px solid #1a2a1a;border-radius:8px;font-family:monospace">
  <!-- Grid lines subtle -->
  <defs>
    <pattern id="grid" width="20" height="20" patternUnits="userSpaceOnUse">
      <path d="M 20 0 L 0 0 0 20" fill="none" stroke="rgba(0,255,136,0.04)" stroke-width="0.5"/>
    </pattern>
    <marker id="arr-red" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto">
      <path d="M2 2L8 5L2 8" fill="none" stroke="#ff4444" stroke-width="1.5"/>
    </marker>
    <marker id="arr-green" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto">
      <path d="M2 2L8 5L2 8" fill="none" stroke="#00ff88" stroke-width="1.5"/>
    </marker>
    <marker id="arr-blue" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto">
      <path d="M2 2L8 5L2 8" fill="none" stroke="#4488ff" stroke-width="1.5"/>
    </marker>
  </defs>
  <rect width="780" height="160" fill="url(#grid)"/>

  <!-- INTERNET / ATTACKER zone -->
  <rect x="20" y="30" width="110" height="100" rx="6" fill="rgba(255,68,68,0.06)" stroke="rgba(255,68,68,0.4)" stroke-width="1" stroke-dasharray="4 3"/>
  <text x="75" y="50" text-anchor="middle" font-size="9" fill="rgba(255,68,68,0.6)" letter-spacing="2">INTERNET</text>
  <!-- Kali skull icon as text -->
  <text x="75" y="95" text-anchor="middle" font-size="28" fill="rgba(255,68,68,0.3)">☠</text>
  <text x="75" y="122" text-anchor="middle" font-size="8" fill="rgba(255,68,68,0.5)">Kali Linux</text>
  <text x="75" y="132" text-anchor="middle" font-size="7" fill="rgba(255,68,68,0.35)">10.93.255.105</text>

  <!-- Attack arrow — dashed red -->
  <line x1="132" y1="80" x2="190" y2="80" stroke="rgba(255,68,68,0.7)" stroke-width="1.5" stroke-dasharray="5 3" marker-end="url(#arr-red)"/>
  <text x="161" y="72" text-anchor="middle" font-size="7" fill="rgba(255,68,68,0.6)">ATTACK</text>

  <!-- WAN Interface box -->
  <rect x="193" y="55" width="80" height="50" rx="4" fill="rgba(255,68,68,0.08)" stroke="rgba(255,68,68,0.5)" stroke-width="1"/>
  <text x="233" y="74" text-anchor="middle" font-size="8" fill="rgba(255,68,68,0.8)" font-weight="bold">WAN</text>
  <text x="233" y="86" text-anchor="middle" font-size="7" fill="rgba(255,68,68,0.5)">em0</text>
  <text x="233" y="97" text-anchor="middle" font-size="6.5" fill="rgba(255,68,68,0.4)">10.93.255.38</text>

  <!-- pfSense+Suricata — central shield -->
  <rect x="295" y="20" width="190" height="120" rx="8" fill="rgba(68,136,255,0.08)" stroke="rgba(68,136,255,0.6)" stroke-width="1.5"/>
  <!-- Shield icon -->
  <path d="M390 35 L405 42 L405 62 Q405 75 390 82 Q375 75 375 62 L375 42 Z" fill="rgba(68,136,255,0.2)" stroke="rgba(68,136,255,0.7)" stroke-width="1"/>
  <text x="390" y="62" text-anchor="middle" font-size="9" fill="rgba(68,136,255,0.9)">✓</text>
  <text x="390" y="100" text-anchor="middle" font-size="10" fill="rgba(68,136,255,0.9)" font-weight="bold">pfSense</text>
  <text x="390" y="113" text-anchor="middle" font-size="8" fill="rgba(68,136,255,0.6)">+ Suricata 7.0.8</text>
  <text x="390" y="126" text-anchor="middle" font-size="7" fill="rgba(68,136,255,0.4)">LEGACY MODE IPS</text>

  <!-- Blocked label over WAN arrow -->
  <rect x="183" y="38" width="46" height="14" rx="3" fill="rgba(255,68,68,0.15)" stroke="rgba(255,68,68,0.4)" stroke-width="0.8"/>
  <text x="206" y="48" text-anchor="middle" font-size="7" fill="#ff4444">✕ BLOCKED</text>

  <!-- Internal arrow (allowed) -->
  <line x1="273" y1="80" x2="293" y2="80" stroke="rgba(68,136,255,0.5)" stroke-width="1" stroke-dasharray="3 2"/>

  <!-- LAN arrow out -->
  <line x1="487" y1="80" x2="507" y2="80" stroke="rgba(0,255,136,0.7)" stroke-width="1.5" marker-end="url(#arr-green)"/>
  <text x="497" y="72" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.6)">CLEAN</text>

  <!-- LAN Interface box -->
  <rect x="509" y="55" width="80" height="50" rx="4" fill="rgba(0,255,136,0.06)" stroke="rgba(0,255,136,0.4)" stroke-width="1"/>
  <text x="549" y="74" text-anchor="middle" font-size="8" fill="rgba(0,255,136,0.8)" font-weight="bold">LAN</text>
  <text x="549" y="86" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.5)">em1</text>
  <text x="549" y="97" text-anchor="middle" font-size="6.5" fill="rgba(0,255,136,0.4)">192.168.10.1</text>

  <!-- LAN arrow to clients -->
  <line x1="591" y1="80" x2="611" y2="80" stroke="rgba(0,255,136,0.5)" stroke-width="1.5" marker-end="url(#arr-green)"/>

  <!-- PROTECTED ZONE -->
  <rect x="613" y="30" width="145" height="100" rx="6" fill="rgba(0,255,136,0.04)" stroke="rgba(0,255,136,0.3)" stroke-width="1" stroke-dasharray="4 3"/>
  <text x="685" y="50" text-anchor="middle" font-size="9" fill="rgba(0,255,136,0.5)" letter-spacing="2">PROTECTED</text>
  <!-- Client devices -->
  <rect x="628" y="60" width="35" height="22" rx="2" fill="rgba(0,255,136,0.08)" stroke="rgba(0,255,136,0.3)" stroke-width="0.8"/>
  <text x="645" y="74" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.6)">CLIENT</text>
  <rect x="672" y="60" width="35" height="22" rx="2" fill="rgba(0,255,136,0.08)" stroke="rgba(0,255,136,0.3)" stroke-width="0.8"/>
  <text x="689" y="74" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.6)">CLIENT</text>
  <rect x="716" y="60" width="35" height="22" rx="2" fill="rgba(0,255,136,0.08)" stroke="rgba(0,255,136,0.3)" stroke-width="0.8"/>
  <text x="733" y="74" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.6)">CLIENT</text>
  <text x="685" y="120" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.4)">192.168.10.x/24</text>
</svg>
</p>

---

<!-- HERO SVG 2 — Attack Matrix -->
<p align="center">
<svg width="780" height="200" viewBox="0 0 780 200" xmlns="http://www.w3.org/2000/svg" style="background:#0a0e14;border:1px solid #1a2a1a;border-radius:8px;font-family:monospace">
  <defs>
    <pattern id="grid2" width="20" height="20" patternUnits="userSpaceOnUse">
      <path d="M 20 0 L 0 0 0 20" fill="none" stroke="rgba(0,255,136,0.03)" stroke-width="0.5"/>
    </pattern>
  </defs>
  <rect width="780" height="200" fill="url(#grid2)"/>
  <text x="390" y="24" text-anchor="middle" font-size="10" fill="rgba(0,255,136,0.4)" letter-spacing="4">ATTACK VECTOR MATRIX</text>

  <!-- Attack cards — 6 columns -->
  <!-- 1. Nmap -->
  <rect x="20" y="38" width="112" height="148" rx="5" fill="rgba(255,68,68,0.05)" stroke="rgba(255,68,68,0.35)" stroke-width="1"/>
  <rect x="20" y="38" width="112" height="20" rx="5" fill="rgba(255,68,68,0.15)"/>
  <text x="76" y="52" text-anchor="middle" font-size="8" fill="rgba(255,68,68,0.9)" font-weight="bold">NMAP SCAN</text>
  <text x="76" y="74" text-anchor="middle" font-size="22" fill="rgba(255,68,68,0.2)">⌖</text>
  <text x="76" y="108" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">NULL / SYN / FIN</text>
  <text x="76" y="120" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">Port recon</text>
  <rect x="30" y="140" width="92" height="16" rx="3" fill="rgba(255,68,68,0.15)" stroke="rgba(255,68,68,0.4)" stroke-width="0.8"/>
  <text x="76" y="151" text-anchor="middle" font-size="7.5" fill="#ff4444">✕ BLOCKED</text>
  <text x="76" y="175" text-anchor="middle" font-size="6" fill="rgba(255,68,68,0.4)">ET SCAN User-Agent</text>

  <!-- 2. Hydra -->
  <rect x="148" y="38" width="112" height="148" rx="5" fill="rgba(255,136,0,0.05)" stroke="rgba(255,136,0,0.35)" stroke-width="1"/>
  <rect x="148" y="38" width="112" height="20" rx="5" fill="rgba(255,136,0,0.15)"/>
  <text x="204" y="52" text-anchor="middle" font-size="8" fill="rgba(255,136,0,0.9)" font-weight="bold">HYDRA SSH</text>
  <text x="204" y="74" text-anchor="middle" font-size="22" fill="rgba(255,136,0,0.2)">⚷</text>
  <text x="204" y="108" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">Brute Force</text>
  <text x="204" y="120" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">Port 22 / SSH</text>
  <rect x="158" y="140" width="92" height="16" rx="3" fill="rgba(255,136,0,0.15)" stroke="rgba(255,136,0,0.4)" stroke-width="0.8"/>
  <text x="204" y="151" text-anchor="middle" font-size="7.5" fill="#ff8800">✕ BLOCKED</text>
  <text x="204" y="175" text-anchor="middle" font-size="6" fill="rgba(255,136,0,0.4)">SID:1000011 threshold</text>

  <!-- 3. hping3 -->
  <rect x="276" y="38" width="112" height="148" rx="5" fill="rgba(255,204,0,0.05)" stroke="rgba(255,204,0,0.35)" stroke-width="1"/>
  <rect x="276" y="38" width="112" height="20" rx="5" fill="rgba(255,204,0,0.15)"/>
  <text x="332" y="52" text-anchor="middle" font-size="8" fill="rgba(255,204,0,0.9)" font-weight="bold">SYN FLOOD</text>
  <text x="332" y="74" text-anchor="middle" font-size="22" fill="rgba(255,204,0,0.2)">≋</text>
  <text x="332" y="108" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">hping3 --flood</text>
  <text x="332" y="120" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">119k pkts/s</text>
  <rect x="286" y="140" width="92" height="16" rx="3" fill="rgba(255,204,0,0.15)" stroke="rgba(255,204,0,0.4)" stroke-width="0.8"/>
  <text x="332" y="151" text-anchor="middle" font-size="7.5" fill="#ffcc00">✕ BLOCKED</text>
  <text x="332" y="175" text-anchor="middle" font-size="6" fill="rgba(255,204,0,0.4)">ET DOS SYN Flood</text>

  <!-- 4. sqlmap -->
  <rect x="404" y="38" width="112" height="148" rx="5" fill="rgba(136,68,255,0.05)" stroke="rgba(136,68,255,0.35)" stroke-width="1"/>
  <rect x="404" y="38" width="112" height="20" rx="5" fill="rgba(136,68,255,0.15)"/>
  <text x="460" y="52" text-anchor="middle" font-size="8" fill="rgba(136,68,255,0.9)" font-weight="bold">SQL INJECT</text>
  <text x="460" y="74" text-anchor="middle" font-size="22" fill="rgba(136,68,255,0.2)">⌗</text>
  <text x="460" y="108" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">sqlmap Layer 7</text>
  <text x="460" y="120" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">OR '1'='1</text>
  <rect x="414" y="140" width="92" height="16" rx="3" fill="rgba(136,68,255,0.15)" stroke="rgba(136,68,255,0.4)" stroke-width="0.8"/>
  <text x="460" y="151" text-anchor="middle" font-size="7.5" fill="#8844ff">✕ BLOCKED</text>
  <text x="460" y="175" text-anchor="middle" font-size="6" fill="rgba(136,68,255,0.4)">DPI + SID:1000010</text>

  <!-- 5. Nikto/DIRB -->
  <rect x="532" y="38" width="112" height="148" rx="5" fill="rgba(0,204,255,0.05)" stroke="rgba(0,204,255,0.35)" stroke-width="1"/>
  <rect x="532" y="38" width="112" height="20" rx="5" fill="rgba(0,204,255,0.15)"/>
  <text x="588" y="52" text-anchor="middle" font-size="8" fill="rgba(0,204,255,0.9)" font-weight="bold">WEB SCAN</text>
  <text x="588" y="74" text-anchor="middle" font-size="22" fill="rgba(0,204,255,0.2)">⌕</text>
  <text x="588" y="108" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">Nikto / DIRB</text>
  <text x="588" y="120" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">HTTP anomalies</text>
  <rect x="542" y="140" width="92" height="16" rx="3" fill="rgba(0,204,255,0.15)" stroke="rgba(0,204,255,0.4)" stroke-width="0.8"/>
  <text x="588" y="151" text-anchor="middle" font-size="7.5" fill="#00ccff">✕ DETECTED</text>
  <text x="588" y="175" text-anchor="middle" font-size="6" fill="rgba(0,204,255,0.4)">HTTP Host invalid</text>

  <!-- 6. wafw00f -->
  <rect x="660" y="38" width="106" height="148" rx="5" fill="rgba(0,255,136,0.05)" stroke="rgba(0,255,136,0.35)" stroke-width="1"/>
  <rect x="660" y="38" width="106" height="20" rx="5" fill="rgba(0,255,136,0.15)"/>
  <text x="713" y="52" text-anchor="middle" font-size="8" fill="rgba(0,255,136,0.9)" font-weight="bold">WAFW00F</text>
  <text x="713" y="74" text-anchor="middle" font-size="22" fill="rgba(0,255,136,0.2)">⊙</text>
  <text x="713" y="108" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">Fingerprinting</text>
  <text x="713" y="120" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">WAF detection</text>
  <rect x="670" y="140" width="86" height="16" rx="3" fill="rgba(0,255,136,0.15)" stroke="rgba(0,255,136,0.4)" stroke-width="0.8"/>
  <text x="713" y="151" text-anchor="middle" font-size="7.5" fill="#00ff88">IPS CONFIRMED</text>
  <text x="713" y="175" text-anchor="middle" font-size="6" fill="rgba(0,255,136,0.4)">pkt/connection level</text>
</svg>
</p>

---

<!-- HERO SVG 3 — EVE JSON + Telegram pipeline -->
<p align="center">
<svg width="780" height="120" viewBox="0 0 780 120" xmlns="http://www.w3.org/2000/svg" style="background:#0a0e14;border:1px solid #1a2a1a;border-radius:8px;font-family:monospace">
  <defs>
    <marker id="a" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto">
      <path d="M2 2L8 5L2 8" fill="none" stroke="#00ff88" stroke-width="1.5"/>
    </marker>
  </defs>
  <text x="390" y="18" text-anchor="middle" font-size="9" fill="rgba(0,255,136,0.4)" letter-spacing="4">DETECTION → LOG → ALERT PIPELINE</text>

  <!-- Step 1: Traffic -->
  <rect x="20" y="30" width="100" height="70" rx="5" fill="rgba(255,68,68,0.05)" stroke="rgba(255,68,68,0.3)" stroke-width="1"/>
  <text x="70" y="55" text-anchor="middle" font-size="20" fill="rgba(255,68,68,0.3)">⚡</text>
  <text x="70" y="76" text-anchor="middle" font-size="8" fill="rgba(255,68,68,0.7)" font-weight="bold">Network</text>
  <text x="70" y="88" text-anchor="middle" font-size="7" fill="rgba(255,68,68,0.4)">Traffic</text>

  <line x1="122" y1="65" x2="152" y2="65" stroke="rgba(0,255,136,0.5)" stroke-width="1.5" marker-end="url(#a)"/>

  <!-- Step 2: Suricata DPI -->
  <rect x="154" y="30" width="120" height="70" rx="5" fill="rgba(68,136,255,0.05)" stroke="rgba(68,136,255,0.3)" stroke-width="1"/>
  <text x="214" y="55" text-anchor="middle" font-size="20" fill="rgba(68,136,255,0.3)">◈</text>
  <text x="214" y="76" text-anchor="middle" font-size="8" fill="rgba(68,136,255,0.7)" font-weight="bold">Suricata DPI</text>
  <text x="214" y="88" text-anchor="middle" font-size="7" fill="rgba(68,136,255,0.4)">Layer 7 analysis</text>

  <line x1="276" y1="65" x2="306" y2="65" stroke="rgba(0,255,136,0.5)" stroke-width="1.5" marker-end="url(#a)"/>

  <!-- Step 3: EVE JSON -->
  <rect x="308" y="30" width="120" height="70" rx="5" fill="rgba(255,204,0,0.05)" stroke="rgba(255,204,0,0.3)" stroke-width="1"/>
  <text x="368" y="55" text-anchor="middle" font-size="20" fill="rgba(255,204,0,0.3)">{ }</text>
  <text x="368" y="76" text-anchor="middle" font-size="8" fill="rgba(255,204,0,0.7)" font-weight="bold">EVE JSON Log</text>
  <text x="368" y="88" text-anchor="middle" font-size="7" fill="rgba(255,204,0,0.4)">/var/log/suricata</text>

  <line x1="430" y1="65" x2="460" y2="65" stroke="rgba(0,255,136,0.5)" stroke-width="1.5" marker-end="url(#a)"/>

  <!-- Step 4: PHP script -->
  <rect x="462" y="30" width="120" height="70" rx="5" fill="rgba(136,68,255,0.05)" stroke="rgba(136,68,255,0.3)" stroke-width="1"/>
  <text x="522" y="55" text-anchor="middle" font-size="20" fill="rgba(136,68,255,0.3)">⌥</text>
  <text x="522" y="76" text-anchor="middle" font-size="8" fill="rgba(136,68,255,0.7)" font-weight="bold">PHP + Cron</text>
  <text x="522" y="88" text-anchor="middle" font-size="7" fill="rgba(136,68,255,0.4)">Every 1 minute</text>

  <line x1="584" y1="65" x2="614" y2="65" stroke="rgba(0,255,136,0.5)" stroke-width="1.5" marker-end="url(#a)"/>

  <!-- Step 5: Telegram -->
  <rect x="616" y="30" width="144" height="70" rx="5" fill="rgba(0,255,136,0.05)" stroke="rgba(0,255,136,0.4)" stroke-width="1"/>
  <text x="688" y="55" text-anchor="middle" font-size="20" fill="rgba(0,255,136,0.3)">✉</text>
  <text x="688" y="76" text-anchor="middle" font-size="8" fill="rgba(0,255,136,0.8)" font-weight="bold">Telegram Alert</text>
  <text x="688" y="88" text-anchor="middle" font-size="7" fill="rgba(0,255,136,0.4)">Real-time mobile</text>
</svg>
</p>

---

<!-- Badges -->
<div align="center">

[![Version](https://img.shields.io/badge/version-1.0.0-00ff88?style=flat-square&labelColor=0a0e14)](https://semver.org/)
[![License](https://img.shields.io/badge/license-MIT-4488ff?style=flat-square&labelColor=0a0e14)](LICENSE)
[![pfSense](https://img.shields.io/badge/pfSense-2.7.2-ff4444?style=flat-square&labelColor=0a0e14)](https://pfsense.org)
[![Suricata](https://img.shields.io/badge/Suricata-7.0.8-ffcc00?style=flat-square&labelColor=0a0e14)](https://suricata.io)
[![ET Open](https://img.shields.io/badge/rules-ET_Open-8844ff?style=flat-square&labelColor=0a0e14)](https://rules.emergingthreats.net)
[![Maintained](https://img.shields.io/badge/Maintained-yes-00ff88?style=flat-square&labelColor=0a0e14)]()

</div>

---

## 🛡️ Implementació d'IDS/IPS amb Suricata sobre pfSense

> Projecte acadèmic de ciberseguretat que demostra la implementació d'un sistema de **Detecció i Prevenció d'Intrusions** sobre un tallafocs pfSense, amb proves de pentesting reals des de Kali Linux.

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

- **Tallafocs/Router:** pfSense (Virtualitzat amb 4 Cores, 8GB RAM).
- **Emmagatzematge:** Doble disc dur físic separat — un per al SO, un dedicat a `/var/log`.
- **Interfícies de Xarxa:**
  - `WAN` (Adaptador Pont): Exposada a Internet i atacs externs.
  - `LAN` (Xarxa Interna): Xarxa privada protegida (192.168.10.x).

---

## 🛠️ Tecnologies Utilitzades

| Categoria | Eines |
|:---|:---|
| **Core** | pfSense 2.7.2, Suricata 7.0.8 (IDS/IPS multifil) |
| **Signatures** | Emerging Threats (ET) Open, Regles personalitzades |
| **Alerting** | PHP, API de Telegram, Cron |
| **Logs** | EVE JSON (format estructurat Capa 7) |
| **DNS Filtering** | pfBlockerNG (DNSBL + IP Lists) |
| **Pentesting (Kali)** | Nmap, Nikto, Hydra, DIRB, hping3, sqlmap, WAFW00F |

---

## 🚀 Característiques Principals

### 1. Mode IPS Actiu (Legacy Mode)
S'ha deshabilitat l'acceleració per maquinari (`Disable hardware checksum offload`) forçant tot el trànsit a passar per la CPU. Suricata realitza Deep Packet Inspection (DPI) i insereix automàticament les IPs atacants a la taula de bloqueig de pfSense (`pf table`) en temps real.

### 2. Polítiques de Seguretat i Signatures
Motor **Emerging Threats (ET) Open** combinat amb regles personalitzades per detectar:
- Trànsit anòmal intern i moviment lateral
- Mineria de criptomonedes (protocol Stratum)
- Protocols insegurs (Telnet port 23)
- Atacs ICMP de gran tamany (tunneling)

### 3. Logs Estructurats EVE JSON
Format estructurat clau-valor que extreu informació fins a la **Capa 7 (Aplicació)**, preparat per integrar-se amb SIEM (Elastic, Zabbix).

### 4. Pass List (Anti-lockout)
Llista blanca `Admin_Pass` que protegeix les IPs de gestió contra autobloquejos accidentals del motor IPS.

---

## ⚔️ Auditoria i Pentesting (Casos d'Ús)

### LAN Pentesting

| Prova | Eina | Comanda | Resultat IPS |
|:---|:---|:---|:---|
| Escaneig de ports | `Nmap` | `nmap -sN` (NULL scan) | ✕ `ET SCAN Nmap User-Agent` |
| Vulnerabilitats web | `Nikto` | `nikto -h 192.168.10.1` | ✕ HTTP Host header anomaly |
| Força bruta SSH | `Hydra` | `hydra -l admin ... ssh` | ✕ `SID:1000011` threshold |
| Enumeració dirs | `DIRB` | `dirb http://...` | ✕ `ET SCAN Possible Nmap` |
| DoS SYN Flood | `hping3` | `hping3 -S --flood` | ✕ `SURICATA STREAM` excessive |
| Protocol insegur | `Telnet` | `telnet 192.168.10.1` | ✕ `SID:1000012` |

### WAN Pentesting

| Prova | Eina | Resultat IPS |
|:---|:---|:---|
| Reconeixement extern | `Nmap -sS -sV` | ✕ IP blocked, scan aturada |
| SYN Flood WAN | `hping3 -S --flood -p 443` | ✕ `ET DOS SYN Flood` |
| Força bruta SSH | `Hydra` | ✕ Dual alert: ET + Custom |
| Injecció SQL | `sqlmap --batch` | ✕ `CRITICAL: WAF/IPS detected` |
| Fingerprinting | `wafw00f` | ✓ IPS confirmed (pkt level) |

---

## 🤖 Automatització i Sistemes Complementaris

### Notificacions Crítiques via Telegram

Script PHP (`/root/suricata_telegram.php`) que:
1. Llegeix l'última línia de `alerts.log`
2. Controla duplicitats amb fitxer temporal de control
3. Executa `cURL` xifrat cap a l'API del bot de Telegram
4. S'executa automàticament cada minut via **Cron**

```
[SURICATA ALERT] 04/20-19:42:37 [wDrop]
[1:1000013:5] ATENCIO - ICMP TETECTAT
{ICMP} 10.93.255.82:8 → 10.93.255.38:0
```

### DNS Sinkholing amb pfBlockerNG

Redirigeix dominis de malware i llistes personalitzades a una Virtual IP de bloqueig (`10.10.10.1`), protegint els clients de la LAN **abans** que la comunicació surti a Internet.

---

## ⚙️ Optimització de Rendiment

- **Run Mode:** `AutoFP` — distribució de flux entre fils del processador virtual (Hash scheduler)
- **Flow Memory Cap:** 128 MB màxim per al seguiment de connexions
- **IP Defrag Memory Cap:** 32 MB per a desfragmentació
- **Selecció intel·ligent de regles:** Només categories aplicables a la xarxa activa (excloses: DNP3, Modbus, MQTT, Kerberos, SMB…)

---

## 👥 Autors i Context Acadèmic

| Camp | Detall |
|:---|:---|
| **Mòdul** | M0379 - Miniprojectes |
| **Grau** | ASIX 2 — Administració de Sistemes Informàtics en Xarxa |
| **Autors** | Izan Ruiz · Youssef Fouad · Adrià Rodríguez |
| **Institut** | INS Sa Palomera |
| **Curs** | 2025 – 2026 |

> *Documentació amb finalitats purament acadèmiques i educatives en l'àmbit de la ciberseguretat.*

---

## 📚 Webgrafia

- [Netgate Docs — pfSense](https://docs.netgate.com)
- [OISF Suricata User Guide — EVE JSON](https://suricata.readthedocs.io/en/latest/output/eve/eve-json-format.html)
- [Emerging Threats Open Rules](https://rules.emergingthreats.net/open/)
- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [THC-Hydra GitHub](https://github.com/vanhauser-thc/thc-hydra)
- [sqlmap Project](https://sqlmap.org/)
- [WAFW00F](https://github.com/EnableSecurity/wafw00f)
- [Telegram Bot API](https://core.telegram.org)
- [pfBlockerNG Forum](https://forum.netgate.com/category/62/pfblockerng)
