# Smart Factory: Real-Time AI Quality Control

The Smart Factory is a cyber-physical edge computing system designed for automated,
real-time defect detection and quality control on industrial manufacturing lines.
The system pairs embedded YOLOv8 computer vision inference with an MQTT messaging backbone
and a live Flask supervisory dashboard.

---

## System Architecture

```text
+-----------------------+
|  Manufacturing Line   |  Camera Feed (Picamera2 / Web Camera)
+-----------------------+
            |
            v
+-----------------------+
|   YOLOv8 Edge Node    |  - High-speed object detection & classification (src/detect.py)
|   (Inference Engine)  |  - Defect labeling, bounding boxes & confidence scoring
+-----------------------+
            |
            +---------------------------------------+
            |                                       |
            v                                       v
+-----------------------+               +-----------------------+
|  Persistent Logging   |               |   MQTT Client Node    |  (src/mqtt_client.py)
|  - CSV Audit Records  |               |  - JSON Telemetry     |
|  - Anomaly Tracking   |               |  - QoS 1 Delivery     |
+-----------------------+               +-----------------------+
                                                    |
                                                    v
                                        +-----------------------+
                                        |  MQTT Broker Engine   |  (Mosquitto, Port 1883)
                                        +-----------------------+
                                                    |
                                                    v
                                        +-----------------------+
                                        | Real-Time Dashboard   |  (src/dashboard.py)
                                        | - Flask Web Server    |
                                        | - HTTP localhost:5000 |
                                        +-----------------------+
```

---

## Key Features

- **Real-Time Edge Detection:** Optimized YOLOv8 neural network delivering low-latency object classification and defect identification on edge hardware.
- **Hardware Integration:** Native interfaces for Raspberry Pi 4/5, Picamera2, and generic USB video capture devices.
- **Industrial Telemetry:** Asynchronous publish/subscribe message delivery over MQTT to industrial brokers.
- **Supervisory Dashboard:** Responsive Flask web interface rendering real-time defect counts, throughput rates, and classification confidence scores.
- **Compliance & Auditing:** Automated persistent logging of detection events and timestamps in structured CSV formats.

---

## Project Structure

```text
smart-factory/
├── README.md                          # Technical specification and system architecture
├── requirements.txt                   # Python runtime dependencies
├── models/                            # Trained YOLO model weights (.pt files)
├── logs/                              # Production defect and detection CSV logs
└── src/                               # Application source code
    ├── detect.py                      # Video capture loop and YOLOv8 inference engine
    ├── dashboard.py                   # Flask server rendering live production statistics
    └── mqtt_client.py                 # Telemetry publisher and broker connection handler
```

---

## Execution Instructions

### 1. Prerequisites and Setup
- Python 3.10 or higher
- Running MQTT Broker (e.g., Eclipse Mosquitto)

```bash
git clone git@github.com:ind4x/smart-factory.git
cd smart-factory
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 2. Launching the System
1. **Start the MQTT Broker:**
   ```bash
   mosquitto -v
   ```
2. **Start the Supervisory Web Dashboard:**
   ```bash
   python3 src/dashboard.py
   ```
   Open `http://localhost:5000` in your web browser.
3. **Start the Edge Detection Engine:**
   ```bash
   python3 src/detect.py
   ```

---

## Technology Stack

- **Computer Vision & Ingestion:** OpenCV, YOLOv8 (Ultralytics), Picamera2
- **Industrial Messaging:** MQTT (Paho-MQTT client, QoS 1)
- **Web Supervision:** Flask, HTML5, CSS3, JavaScript
- **Programming Language:** Python 3

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Author

- **Author:** Youssef Fellah ([ind4x_](https://github.com/ind4x))
