
<p align="center">
  <img src="logo.jpg" alt="Sabbathia Logo" width="300"/>
</p>

# Sabbathia

**Sabbathia** is a proprietary control backend developed by **Supernova 3D** for the **Pulse One** machine. It builds on the core concepts of MonkeyPrint but removes slicing and GUI components for a lightweight, headless controller capable of running on SBCs like the Raspberry Pi 5.

---

## Overview

Sabbathia (formerly Lite MonkeyPrint) is designed for machines using dual-bucket 3D DLP technology. It manages model projection, mechatronics communication, and real-time parameter updates through a robust HTTP-based API.

---

## Key Features

- Load `.vlm` files containing dual-bucket slice stacks (8K PNGs).
- Control Pulse One via HTTP using Duet/RepRap-compatible firmware.
- Cast slices to HDMI-connected LCD in real time.
- Modify exposure and peeling parameters mid-print.
- Monitor print progress and machine status via API or web UI.
- Auto-load machine profiles and run on boot.

---

## Technology Stack

| Feature              | Sabbathia                             |
|---------------------|----------------------------------------|
| Slicing             | ❌ (.vlm required)                     |
| GUI                 | ❌ Headless Web UI only                |
| Mechatronics Comm.  | ✅ HTTP (Duet/RepRapFirmware)         |
| LCD Projection      | ✅ HDMI (Pillow, OpenCV, NumPy)       |
| Platform            | ✅ SBC-friendly (e.g., Raspberry Pi)  |
| User Interaction    | ✅ Web UI (Flask, FastAPI backend)    |

---

## Installation

### On Raspberry Pi (recommended)

```bash
# Ensure Python 3.7 is available
sudo apt install python3.7
pip3.7 install virtualenv

# Create and activate virtual environment
python3.7 -m virtualenv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Launch Sabbathia
python sabbathia.py
```

Set Sabbathia to auto-launch using `systemd` for production.

---

## Usage

### Web Interface

- Upload `.vlm` files (containing pre-sliced layers, machine config, and settings).
- Select machine profile (proto) and material.
- Adjust exposure, peel speed, etc.
- Start/stop print jobs.
- View system status and logs.

### .vlm Structure

A `.vlm` archive contains:

- `left/######_left.png` — Left bucket slices
- `right/######_right.png` — Right bucket slices
- `settings/` — Configuration for print
- `Pickle.bin` — Model metadata
- `model.stl` — STL model reference

---

## REST API Endpoints

| Endpoint              | Method | Description                                |
|-----------------------|--------|--------------------------------------------|
| `/start-print`        | POST   | Start a print job                          |
| `/stop-print`         | POST   | Stop current job                           |
| `/upload-slice`       | POST   | Upload `.vlm` file                         |
| `/update-settings`    | POST   | Modify parameters live                     |
| `/profiles`           | GET    | Fetch machine/material profiles            |
| `/machine-info`       | GET    | Query connected printer specs              |
| `/status`             | GET    | Monitor job status                         |
| `/update-profile`     | POST   | Change material or machine presets         |
| `/api/health`         | GET    | API server and hardware health             |

> All endpoints require `Authorization: Bearer <api_key>` headers.

---

## System Architecture

Sabbathia runs a Flask/FastAPI backend that:
- Receives and parses `.vlm` files
- Sends slice images over HDMI
- Talks to the printer via HTTP
- Serves a browser-based control UI

Optional modules:
- Logging to `/home/admin/Printerlog/`
- WebSockets for real-time status
- Auto-load profile from local config

---

## Licensing and Ownership

This software and all associated intellectual property is © Supernova Additive S.L.U. Unauthorized distribution or reverse engineering is prohibited. Sabbathia is provided under a proprietary license.

Freelancers or third-party contributors must sign NDAs and acknowledge the work is for hire with no licensing rights retained.

---

## Contact

For support or licensing inquiries, contact:  
**Supernova Additive S.L.U.**  
Email: support@supernova3d.com  
Website: [https://www.supernova3d.com](https://www.supernova3d.com)

---

Happy Printing from the Sabbathia team.
