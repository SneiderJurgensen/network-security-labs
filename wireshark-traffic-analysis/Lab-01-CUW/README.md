# 🔍 Análisis de tráfico de red con Wireshark: Exploit Kit Angler

🇪🇸 **Español** (estás aquí) · [🇬🇧 English](README.en.md)

Taller de análisis de tráfico de red con **Wireshark** sobre una captura (`.pcap`) de una infección por el Exploit Kit **Angler**. Proyecto académico de la Universidad Simón Bolívar, sede Cúcuta.

## 📌 Contexto

| | |
|---|---|
| **Asignatura** | Electiva Profesional II |
| **Docente** | Jover Alonso Cabrales Pineda |
| **Autor** | Sneider G. Jurgensen B. |
| **Fecha** | 27 de febrero de 2026 |
| **Actividad** | Taller 8 – Wireshark |

## 🎯 Objetivos

- Familiarizarse con Wireshark como herramienta de análisis de tráfico de red.
- Comprender la importancia de la configuración adecuada de los protocolos de transmisión de datos.
- Analizar y comprender el tráfico de red a partir de un archivo pcap.

## 🧪 Escenario

Un host Windows visita un sitio web que inicia una cadena de eventos que conduce al Exploit Kit (EK) Angler, comprometiendo el sitio `www.opushangszer.hu`.

## 🛠️ Herramientas

- **Wireshark** (versión estable descargada del sitio oficial).
- Captura `Traffic-analysis.pcap` (503 paquetes), entregada por el docente en el aula extendida.

> ⚠️ **El `.pcap` no se incluye en este repositorio** porque contiene tráfico malicioso real. Solicítalo a tu docente o usa una captura equivalente de un ejercicio público de análisis de tráfico.

## 📊 Resultados

| Pregunta | Respuesta |
|---|---|
| Fecha y hora | 5 de enero de 2015, 16:24:40 UTC |
| Host Windows | IP `192.168.204.137` · MAC `00:0c:29:9d:b8:6d` |
| Sitio comprometido | `www.opushangszer.hu` (`94.199.178.119`) |
| Exploit Kit | `akronkappas.com` (`69.65.9.55`) |
| Navegador | Internet Explorer 8 sobre Windows 7 |

## 🔎 Cómo reproducir el análisis

| Objetivo | Filtro / acción en Wireshark |
|---|---|
| Ver tráfico web | `http` |
| Ver solo peticiones | `http.request` |
| Sitio comprometido | `http.host contains "opushangszer.hu"` |
| Exploit Kit | `http.host == "akronkappas.com"` |
| Hora UTC | Frame 4 → *Frame* → *UTC Arrival Time* |
| IP y MAC del host | Frame 4 → *Ethernet II* / *IPv4* → *Source* (o *Destination* en el frame 17) |
| Navegador | Frame 25 → *HTTP* → *User-Agent* |

## ⏱️ Cadena de ataque

![Cadena de ataque](img/06-diagrama-cadena-ataque.png)

1. La víctima llega desde un resultado de búsqueda de Google (`google.si`) al sitio comprometido `www.opushangszer.hu` (frame 4).
2. Descarga un archivo Flash (`.swf`) desde `imprintchurch.org` (frame 25).
3. Realiza un POST a la página del kit en `akronkappas.com` (frame 33).
4. Después, realiza varias peticiones a `static.domainvertythephones.com` (`167.160.46.121`).

Todo ocurre en unos 36 segundos. El contenido de las respuestas posteriores al kit no fue inspeccionado, por lo que no se afirma qué se descargó.

## 🖼️ Evidencias

| | |
|---|---|
| **1. Fecha y hora** | ![Fecha y hora](img/01-fecha-hora.png) |
| **2. IP y MAC del host** | ![IP y MAC](img/02-ip-mac-host.png) |
| **3. Sitio comprometido** | ![Sitio comprometido](img/03-sitio-comprometido.png) |
| **4. Exploit Kit** | ![Exploit Kit](img/04-kit-exploits.png) |
| **5. Navegador** | ![Navegador](img/05-navegador.png) |

## 📄 Informe completo

El informe con el procedimiento detallado por pregunta, la tabla de indicadores de compromiso y las conclusiones está en:
**[`docs/Informe_Analisis_Trafico_Angler_EK.pdf`](docs/Informe_Analisis_Trafico_Angler_EK.pdf)**

## 📁 Estructura del repositorio

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

## ⚖️ Aviso

Proyecto con fines educativos. Los indicadores de compromiso pertenecen a una campaña de 2015 y se incluyen solo como referencia académica.
