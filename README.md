Underground Mines Safety System — Sensor Bridge

The Sensor Bridge connects the Arduino UNO installed with the mining safety sensors to the FastAPI backend deployed on Render.

Architecture

Arduino UNO
    │
    │ USB Serial
    ▼
Laptop
    │
    │ Python Sensor Bridge
    ▼
Render FastAPI
    │
    │ WebSocket
    ▼
Web Dashboard

Responsibilities

The Sensor Bridge:

1. Reads sensor data from the Arduino Serial/COM port.
2. Parses the incoming JSON.
3. Sends the sensor data to the Render FastAPI WebSocket.
4. Keeps the Arduino-to-backend communication running continuously.

Expected Arduino Data

The Arduino should send one JSON object per line:

{
  "temp": 28.0,
  "gas": 125,
  "fire": 0,
  "sos": 0,
  "warning": 0,
  "danger": 0,
  "fan": 0
}

Installation

Create and activate a virtual environment:

python -m venv venv

Windows:

venv\Scripts\activate

Install dependencies:

pip install -r requirements.txt

Configuration

Create a ".env" file:

ARDUINO_PORT=COM3
BAUD_RATE=9600
BACKEND_WS_URL=wss://your-backend.onrender.com/ws
SERIAL_TIMEOUT=1

Change "COM3" to the COM port assigned to your Arduino UNO.

Run

Start the bridge:

python main.py

Expected output:

============================================================
 Underground Mines Safety System - Sensor Bridge
============================================================

[SERIAL] Connected to Arduino on COM3 @ 9600 baud
[WS] Connected to backend: wss://your-backend.onrender.com/ws
[SYSTEM] Sensor bridge started.
[SYSTEM] Waiting for Arduino data...

[ARDUINO] Received: {'temp': 28.0, 'gas': 125, 'fire': 0, ...}
[WS] Sent: {"temp": 28.0, "gas": 125, "fire": 0, ...}

Important

Do not run the Arduino Serial Monitor while the Sensor Bridge is running. Both applications cannot normally use the same COM port simultaneously.

Also, do not commit ".env" to GitHub because it contains local configuration.

Data Flow

Sensor
   ↓
Arduino UNO
   ↓
USB Serial
   ↓
serial_reader.py
   ↓
main.py
   ↓
websocket_client.py
   ↓
Render FastAPI WebSocket
