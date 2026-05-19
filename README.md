# AI Driven Robotic Arm Optimization in Conveyor Belt

![Project Banner](banner.png)

## Overview

**AI Driven Robotic Arm Optimization in Conveyor Belt** is a full-stack automated sortation and quality control system. It integrates computer vision, a real-time decision engine, and a web-based control dashboard to identify, track, and sort items on a moving conveyor belt with a robotic arm.

This project uses advanced object detection (YOLO/Caffe models) to inspect products, read QR codes, and intelligently guide a serial-controlled robotic arm to optimize sorting processes.

![System Architecture](placeholder-image-architecture.png)

## Features

- 📸 **Computer Vision Pipeline**: Real-time object tracking, damage detection, and defect identification.
- 🦾 **Robotic Arm Control**: Serial communication to a robotic arm for automated sorting and picking (via `decision_engine` and `arm_service`).
- 🖥️ **Interactive Web Dashboard**: React-based frontend to monitor the camera feed, view logs, simulate the conveyor belt, and control system operations.
- ⚡ **Real-time WebSockets**: Low-latency communication between the AI backend and the frontend interface.
- 🔍 **QR Scanning**: Native integrations for tracking individual products throughout the cycle.

---

## 🧠 Detailed System Architecture & Pipelining

The project is divided into four highly synchronized sub-systems that work together to achieve millisecond-precision automated sorting:

### 1. The Computer Vision Pipeline (AI & Perception)
At the core of the system is the `vision_engine.py`, which acts as the "eyes" of the operation. 
- **Object & Defect Detection**: Using pre-trained deep learning models (YOLO and Caffe-based networks like `detect.caffemodel` and `sr.caffemodel`), the system continuously analyzes video frames from an overhead camera. It identifies products and runs them through `damage_detector.py` and `yolo_defect_detector.py` to spot manufacturing flaws in real-time.
- **Tracking & Identification**: As items move down the belt, `tracker.py` maintains an active lock on their spatial coordinates. An integrated QR code scanner queries specific product metadata to trace individual items precisely.

### 2. The Decision Engine & Hardware Control (Action)
Once the vision system categorizes an item ("Pass", "Defective", or a specific sort category), the backend must act.
- **Decision Engine (`decision_engine.py`)**: Evaluates the vision data in real-time. It maps the item's pixel coordinates from the camera feed to the physical kinematics/coordinates of the robotic arm.
- **Robotic Arm Service (`arm_service.py` & `serial_service.py`)**: Manages the physical hardware. When a targeted object reaches the interaction zone, the engine issues serial commands (e.g., G-Code) to the arm via USB/COM ports, safely intercepting and sorting the item.

### 3. High-Performance Backend (FastAPI)
The intelligence and hardware layers are wrapped in a robust Python **FastAPI** backend (`backend/app/main.py`).
- **REST API & WebSockets**: Standard endpoints (`routes.py`) handle system configurations, while WebSocket connections (`websockets.py`) stream high-frequency telemetry, live camera feeds, and real-time arm status down to the frontend with ultra-low latency.

### 4. Interactive Frontend Dashboard (React & Vite)
Operators manage the entire process through a sleek, web-based dashboard built with React, Vite, and Tailwind CSS.
- **Live Monitoring & Simulation**: Components like `<CameraFeed />` and `<ConveyorSimulation />` render the AI's perspective and virtual hardware state directly in the browser.
- **Manual Control & Overrides**: Human operators can override the AI via the `<ControlPanel />`, allowing them to manually jog the robotic arm, toggle the conveyor belt, and recalibrate system boundaries dynamically.

---

## ⚙️ Step-by-Step Working Flow

1. **Intake**: A product is placed on the moving conveyor belt.
2. **Perception**: The overhead camera captures the frame. The Python backend processes this frame to identify the product (via QR code or shape bounding box) and inspects it for surface defects.
3. **Telemetry Streaming**: The WebSocket server actively streams the augmented processed visual data (bounding boxes, read text) and analytics to the React dashboard.
4. **Coordinate Calculation**: If a defect is found (or a sorting rule is met), the `decision_engine` computes the exact interception trajectory based on the conveyor's ongoing speed and the arm's physical reach.
5. **Physical Execution**: Serial commands are fired to the robotic arm. The arm moves to the computed coordinates, engages its end-effector (e.g., suction cup or gripper), picks up the item, and deposits it into the designated sortation bin.

---

## 📷 Media & Screenshots

| Web Dashboard | Hardware Setup |
| ------------- | -------------- |
| ![Frontend Dashboard](ui.png) | ![Robotic Arm Setup](arm.png) |
| *Real-time React dashboard with camera feed & system logs.* | *Physical robotic arm and conveyor belt hardware.* |

---

## 🚀 Getting Started

### Prerequisites

- **Backend**: Python 3.9+
- **Frontend**: [Bun](https://bun.sh/) or Node.js (npm/yarn)
- **Hardware**: USB Web Camera, Serial-compatible Robotic Arm, Conveyor setup.

### 1. Backend Setup (AI & Robotic Arm Services)

The backend is built with FastAPI and runs the AI vision models and serial communication.

```bash
# Navigate to the backend directory
cd backend

# Create and activate a virtual environment (Windows)
python -m venv .venv
.\.venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the backend server (FastAPI)
python app/main.py
# Or use the provided batch script:
# .\run_with_camera.bat
```

> **Note on AI Models:** Ensure you have the necessary weights in `backend/app/vision/models/` (e.g., `detect.caffemodel`, `sr.caffemodel`) before starting the vision engine.

### 2. Frontend Setup (React Dashboard)

The frontend is built with React, Vite, and Tailwind CSS.

```bash
# Navigate to the frontend directory
cd frontend

# Install dependencies using Bun (recommended) or npm
bun install
# or: npm install

# Start the Vite development server
bun run dev
# or: npm run dev
```

Visit `http://localhost:5173` (or the port specified by Vite) in your browser to access the Control Panel.

---

## 🔌 Hardware Configuration

### Connecting the Robotic Arm

1. Connect your corresponding Robotic Arm to the machine via USB/Serial.
2. The `serial_service.py` is configured to communicate with the hardware automatically. Update the COM port configurations in `backend/app/core/config.py` if needed.
3. Use the **Frontend Control Panel** to manually jog the arm and verify the connection.

![Arm Calibration](control.png)

---

## 🏗️ Project Structure

- `/backend`: Python FastAPI application containing endpoints (`routes.py`), WebSocket streams, Serial commands (`arm_service.py`), and Computer Vision node (`vision_engine.py`).
- `/frontend`: React/Vite application containing interactive UI components, context providers, and real-time visualization controls.
- `/app/vision/models`: Pre-trained AI models (`.caffemodel`, `.prototxt`) used by OpenCV for spatial recognition.

## 🤝 Contributing

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
