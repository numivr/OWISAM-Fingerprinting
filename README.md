# 📡 OWISAM-FP — Wi-Fi Fingerprinting Tool

> Herramienta de reconocimiento pasivo de redes Wi-Fi basada en la captura y análisis de tramas Beacon.  
> Desarrollada en Python con Scapy como parte de la metodología **OWISAM** (Open Wireless Security Assessment Methodology).

---

## 📋 Descripción

**OWISAM-FP** es un script de fingerprinting Wi-Fi que captura tramas Beacon en modo monitor para extraer metadatos técnicos de los puntos de acceso (AP) cercanos. A partir de esta información, genera un **fingerprint único por dispositivo** y exporta los resultados a archivos estructurados (JSON / CSV) para su posterior análisis.

Esta herramienta forma parte del bloque **FP (Fingerprinting)** de la metodología OWISAM, cuyo objetivo es identificar y caracterizar los dispositivos inalámbricos presentes en el entorno auditado.

---

## 🎯 Objetivo

Identificar y perfilar dispositivos Wi-Fi (puntos de acceso) detectados pasivamente, extrayendo la siguiente información:

| Campo            | Descripción                                      |
|------------------|--------------------------------------------------|
| **BSSID**        | Dirección MAC del punto de acceso                |
| **SSID**         | Nombre de la red Wi-Fi                           |
| **Fabricante**   | Identificado mediante lookup OUI (IEEE)          |
| **Canal**        | Canal de operación del AP                        |
| **Potencia**     | Nivel de señal RSSI (dBm)                        |
| **Cifrado**      | Tipo de seguridad (WPA2, WPA3, WEP, Open...)     |
| **Fingerprint**  | Hash único generado a partir de los metadatos    |

---

## 🛠️ Requisitos

### Sistema operativo
- Linux (recomendado: Kali Linux / Parrot OS)
- Adaptador Wi-Fi con soporte para **modo monitor**

### Software
- Python 3.8+
- Scapy
- (Opcional) `manuf` o base de datos OUI para resolución de fabricantes

### Instalación de dependencias

```bash
pip install scapy
pip install manuf        # Para resolución de fabricante por OUI
```

> ⚠️ Se requieren permisos de superusuario (`sudo`) para capturar tráfico en modo monitor.

---

## ⚙️ Instalación

```bash
# Clonar el repositorio
git clone https://github.com/usuario/owisam-fp.git
cd owisam-fp

# Instalar dependencias
pip install -r requirements.txt
```

---

## 🚀 Uso

### 1. Activar modo monitor en la interfaz Wi-Fi

```bash
sudo airmon-ng start wlan0
# La interfaz quedará disponible como wlan0mon
```

### 2. Ejecutar el script

```bash
sudo python3 owisam_fp.py -i wlan0mon
```

### Opciones disponibles

```
uso: owisam_fp.py [-h] -i INTERFAZ [-t TIEMPO] [-o SALIDA] [-f FORMATO]

opciones:
  -h, --help            Muestra este mensaje de ayuda
  -i INTERFAZ           Interfaz Wi-Fi en modo monitor (ej: wlan0mon)
  -t TIEMPO             Tiempo de captura en segundos (default: 60)
  -o SALIDA             Nombre del archivo de salida (default: resultado)
  -f FORMATO            Formato de exportación: json | csv (default: json)
```

### Ejemplo de uso

```bash
# Capturar durante 120 segundos y exportar a CSV
sudo python3 owisam_fp.py -i wlan0mon -t 120 -o escaneo_red -f csv
```

---

## 📊 Salida esperada

### Tabla en consola

```
+-------------------+------------------+----------------------------+-------+--------+--------+----------------------------------+
| BSSID             | SSID             | Fabricante                 | Canal | RSSI   | Cifrado| Fingerprint                      |
+-------------------+------------------+----------------------------+-------+--------+--------+----------------------------------+
| AA:BB:CC:11:22:33 | RedEmpresa_5G    | Cisco Systems              | 36    | -55dBm | WPA2   | 3f4a1b2c...                      |
| DD:EE:FF:44:55:66 | MiCasa           | TP-Link Technologies       | 6     | -72dBm | WPA3   | 9c1d8e7f...                      |
| 00:11:22:33:44:55 | HotspotPublico   | Unknown                    | 11    | -81dBm | Open   | 1a2b3c4d...                      |
+-------------------+------------------+----------------------------+-------+--------+--------+----------------------------------+
```

### Archivo JSON generado (`resultado.json`)

```json
[
  {
    "bssid": "AA:BB:CC:11:22:33",
    "ssid": "RedEmpresa_5G",
    "fabricante": "Cisco Systems",
    "canal": 36,
    "rssi": -55,
    "cifrado": "WPA2",
    "fingerprint": "3f4a1b2c9e..."
  }
]
```

---

## 📁 Estructura del proyecto

```
owisam-fp/
├── owisam_fp.py          # Script principal
├── requirements.txt      # Dependencias del proyecto
├── utils/
│   ├── oui_lookup.py     # Resolución de fabricante por OUI
│   ├── fingerprint.py    # Generación del hash fingerprint
│   └── exporter.py       # Exportación a JSON / CSV
├── output/               # Carpeta de resultados generados
└── README.md             # Este archivo
```

## 📚 Referencias

- [OWISAM — Open Wireless Security Assessment Methodology](https://www.owisam.org)
- [Scapy Documentation](https://scapy.readthedocs.io)
- [IEEE OUI Registry](https://regauth.standards.ieee.org/standards-ra-web/pub/view.html#registries)
- [Aircrack-ng Suite](https://www.aircrack-ng.org)

---

## 🧑‍💻 Autor

Desarrollado como parte del proyecto de fin de máster (proyecto de auditoría Wi-Fi basado en la metodología OWISAM) 
realizado por Marco Jiménez y Numi Valenzuela.  

