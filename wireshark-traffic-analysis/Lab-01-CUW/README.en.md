# 🔍 Network Traffic Analysis with Wireshark: Angler Exploit Kit

[🇪🇸 Español](README.md) · 🇬🇧 **English** (you are here)

Network traffic analysis lab using **Wireshark** on a packet capture (`.pcap`) of an **Angler Exploit Kit** infection. Academic project from Universidad Simón Bolívar, Cúcuta campus.

> 📄 The full report (PDF) is written in Spanish.

## 📌 Context

| | |
|---|---|
| **Course** | Professional Elective II |
| **Instructor** | Jover Alonso Cabrales Pineda |
| **Author** | Sneider G. Jurgensen B. |
| **Date** | February 27, 2026 |
| **Activity** | Lab 8 – Wireshark |

## 🎯 Objectives

- Get familiar with Wireshark as a network traffic analysis tool.
- Understand why correct configuration of data transmission protocols matters.
- Analyze and understand network traffic from a pcap file.

## 🧪 Scenario

A Windows host visits a website that starts a chain of events leading to the Angler Exploit Kit (EK), compromising `www.opushangszer.hu`.

## 🛠️ Tools

- **Wireshark** (stable release from the official website).
- `Traffic-analysis.pcap` capture (503 packets), provided by the instructor through the course platform.

> ⚠️ **The `.pcap` is not included in this repository** because it contains real malicious traffic. Ask your instructor or use an equivalent capture from a public traffic-analysis exercise.

## 📊 Findings

| Question | Answer |
|---|---|
| Date and time | January 5, 2015, 16:24:40 UTC |
| Windows host | IP `192.168.204.137` · MAC `00:0c:29:9d:b8:6d` |
| Compromised website | `www.opushangszer.hu` (`94.199.178.119`) |
| Exploit kit | `akronkappas.com` (`69.65.9.55`) |
| Browser | Internet Explorer 8 on Windows 7 |

## 🔎 How to reproduce

| Goal | Wireshark filter / action |
|---|---|
| Web traffic | `http` |
| Requests only | `http.request` |
| Compromised website | `http.host contains "opushangszer.hu"` |
| Exploit kit | `http.host == "akronkappas.com"` |
| UTC time | Frame 4 → *Frame* → *UTC Arrival Time* |
| Host IP and MAC | Frame 4 → *Ethernet II* / *IPv4* → *Source* (or *Destination* in frame 17) |
| Browser | Frame 25 → *HTTP* → *User-Agent* |

## ⏱️ Attack chain

![Attack chain](img/06-diagrama-cadena-ataque.png)

1. The victim arrives from a Google search result (`google.si`) at the compromised site `www.opushangszer.hu` (frame 4).
2. It downloads a Flash file (`.swf`) from `imprintchurch.org` (frame 25).
3. It sends a POST to the exploit kit page on `akronkappas.com` (frame 33).
4. It then makes several requests to `static.domainvertythephones.com` (`167.160.46.121`).

The whole chain takes about 36 seconds. The contents of the responses after the kit contact were not inspected, so no claim is made about what was downloaded.

## 🖼️ Evidence

| | |
|---|---|
| **1. Date and time** | ![Date and time](img/01-fecha-hora.png) |
| **2. Host IP and MAC** | ![IP and MAC](img/02-ip-mac-host.png) |
| **3. Compromised website** | ![Compromised website](img/03-sitio-comprometido.png) |
| **4. Exploit kit** | ![Exploit kit](img/04-kit-exploits.png) |
| **5. Browser** | ![Browser](img/05-navegador.png) |

## 📄 Full report

The report, with the detailed procedure per question, the indicators of compromise table and the conclusions, is at:
**[`docs/Informe_Analisis_Trafico_Angler_EK.pdf`](docs/Informe_Analisis_Trafico_Angler_EK.pdf)** *(Spanish)*

## 📁 Repository structure

```
.
├── README.md
├── README.en.md
├── .gitignore
├── docs/
│   └── Informe_Analisis_Trafico_Angler_EK.pdf
└── img/
    ├── 01-fecha-hora.png
    ├── 02-ip-mac-host.png
    ├── 03-sitio-comprometido.png
    ├── 04-kit-exploits.png
    ├── 05-navegador.png
    └── 06-diagrama-cadena-ataque.png
```

## ⚖️ Disclaimer

For educational purposes only. The indicators of compromise belong to a 2015 campaign and are included as academic reference only.
