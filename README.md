AI Driven Robotic Arm Optimization in Conveyor Belt

Project Banner
Overview

AI Driven Robotic Arm Optimization in Conveyor Belt is a full-stack automated sortation and quality control system. It integrates computer vision, a real-time decision engine, and a web-based control dashboard to identify, track, and sort items on a moving conveyor belt with a robotic arm.

This project uses advanced object detection (YOLO/Caffe models) to inspect products, read QR codes, and intelligently guide a serial-controlled robotic arm to optimize sorting processes.

System Architecture
Features

    📸 Computer Vision Pipeline: Real-time object tracking, damage detection, and defect identification.
    🦾 Robotic Arm Control: Serial communication to a robotic arm for automated sorting and picking (via decision_engine and arm_service).
    🖥️ Interactive Web Dashboard: React-based frontend to monitor the camera feed, view logs, simulate the conveyor belt, and control system operations.
    ⚡ Real-time WebSockets: Low-latency communication between the AI backend and the frontend interface.
    🔍 QR Scanning: Native integrations for tracking individual products throughout the cycle.

🧠 Detailed System Architecture & Pipelining

The project is divided into four highly synchronized sub-systems that work together to achieve millisecond-precision automated sorting:
1. The Computer Vision Pipeline (AI & Perception)

At the core of the system is the vision_engine.py, which acts as the "eyes" of the operation.

    Object & Defect Detection: Using pre-trained deep learning models (YOLO and Caffe-based networks like detect.caffemodel and sr.caffemodel), the system continuously analyzes video frames from an overhead camera. It identifies products and runs them through damage_detector.py and yolo_defect_detector.py to spot manufacturing flaws in real-time.
    Tracking & Identification: As items move down the belt, tracker.py maintains an active lock on their spatial coordinates. An integrated QR code scanner queries specific product metadata to trace individual items precisely.

2. The Decision Engine & Hardware Control (Action)

Once the vision system categorizes an item ("Pass", "Defective", or a specific sort category), the backend must act.

    Decision Engine (decision_engine.py): Evaluates the vision data in real-time. It maps the item's pixel coordinates from the camera feed to the physical kinematics/coordinates of the robotic arm.
    Robotic Arm Service (arm_service.py & serial_service.py): Manages the physical hardware. When a targeted object reaches the interaction zone, the engine issues serial commands (e.g., G-Code) to the arm via USB/COM ports, safely intercepting and sorting the item.

3. High-Performance Backend (FastAPI)

The intelligence and hardware layers are wrapped in a robust Python FastAPI backend (backend/app/main.py).

    REST API & WebSockets: Standard endpoints (routes.py) handle system configurations, while WebSocket connections (websockets.py) stream high-frequency telemetry, live camera feeds, and real-time arm status down to the frontend with ultra-low latency.

4. Interactive Frontend Dashboard (React & Vite)

Operators manage the entire process through a sleek, web-based dashboard built with React, Vite, and Tailwind CSS.

    Live Monitoring & Simulation: Components like <CameraFeed /> and <ConveyorSimulation /> render the AI's perspective and virtual hardware state directly in the browser.
    Manual Control & Overrides: Human operators can override the AI via the <ControlPanel />, allowing them to manually jog the robotic arm, toggle the conveyor belt, and recalibrate system boundaries dynamically.

⚙️ Step-by-Step Working Flow

    Intake: A product is placed on the moving conveyor belt.
    Perception: The overhead camera captures the frame. The Python backend processes this frame to identify the product (via QR code or shape bounding box) and inspects it for surface defects.
    Telemetry Streaming: The WebSocket server actively streams the augmented processed visual data (bounding boxes, read text) and analytics to the React dashboard.
    Coordinate Calculation: If a defect is found (or a sorting rule is met), the decision_engine computes the exact interception trajectory based on the conveyor's ongoing speed and the arm's physical reach.
    Physical Execution: Serial commands are fired to the robotic arm. The arm moves to the computed coordinates, engages its end-effector (e.g., suction cup or gripper), picks up the item, and deposits it into the designated sortation bin.

📷 Media & Screenshots
Web Dashboard 	Hardware Setup
Frontend Dashboard 	Robotic Arm Setup
Real-time React dashboard with camera feed & system logs. 	Physical robotic arm and conveyor belt hardware.
PresentAR

PresentAR is a comprehensive interactive presentation and 3D application system. It orchestrates a web-based frontend dashboard, a Python backend for file and model handling, and ESP32-based firmware using LVGL for a rich hardware interface.
� Core Idea

The core concept of PresentAR is to bridge the gap between physical presentation tools and immersive digital content. By utilizing an M5Stack CoreS3 SE as a tactile, interactive remote and extended display, users can seamlessly control, rotate, and interact with 3D models and digital presentations on a web dashboard. It brings a new layer of engagement to presentations by combining physical hardware inputs with augmented/3D web rendering.
⚙️ How It Works (Working)

    [Insert System Architecture Diagram Here] (e.g., ![Architecture Diagram](docs/images/architecture.png))

    Hardware Interaction: The presenter uses the M5Stack CoreS3 SE device to interact with the system. The device's touch screen and built-in sensors capture inputs, rendering a local UI using LVGL.
    Backend Communication: The firmware communicates over Wi-Fi/UART with a local Python backend. The backend acts as the central hub, processing commands, and managing static assets like 3D models and recording files.
    Real-time Web Dashboard: The frontend (powered by Vite and Bun) fetches this real-time state and media from the backend. When the presenter interacts with the M5Stack device, the web dashboard immediately updates—rotating 3D models, changing slides, or playing media.
    Recording & Saving: Presenters can record their live sessions directly through the dashboard. The system captures the presentation flow, slide transitions, and 3D manipulations, saving the recordings locally to the backend (/recordings) for future playback, review, or export.

✨ Benefits

    Enhanced Engagement: Moves beyond traditional slide clickers by allowing physical manipulation of digital 3D space.
    Session Recording: Easily capture, save, and export complete presentations for asynchronous sharing or archiving.
    Real-Time Synchronization: Instantaneous feedback between the physical device and the web application.
    Portable & Wireless: Built on the ESP32-S3 architecture, the system is designed to be untethered over Wi-Fi.
    Rich Visuals: Combines the embedded graphics capability of LVGL on the device with high-fidelity web-based 3D rendering.

🛠 Hardware Specifications
M5Stack CoreS3 SE (Target Device)

    [Insert Image of M5Stack CoreS3 SE Device Here] (e.g., ![M5Stack CoreS3 SE](docs/images/device.png))

The firmware is designed around the features and capabilities of the M5Stack CoreS3 SE.

    Microcontroller (MCU): ESP32-S3 @ 240MHz, dual-core Xtensa LX7
    Wireless Connectivity: 2.4 GHz Wi-Fi & Bluetooth 5.0 (LE)
    Memory & Storage: 8MB PSRAM + 16MB Flash Memory
    Display: 2.0-inch IPS capacitive touch screen (resolution: 320x240)
    Power Management: AXP2101 PMU, built-in RTC (BM8563)
    Audio: Built-in 1W speaker (AW88298 amplifier), built-in dual microphones
    External Sensors: MPU6500 (6-axis Gyroscope/Accelerometer) connected via I2C
    Expansion/I/O:
        1x USB Type-C port (Power/Programming)
        GROVE/I2C Expansion Ports
        MicroSD (TF Card) Slot for expanded storage

📂 Project Structure

This repository is divided into three primary components:

    [Insert Detailed Flow/Component Diagram Here] (e.g., ![Flow Diagram](docs/images/diagram.png))

1. Frontend (/frontend)

    [Insert Dashboard Screenshot Here] (e.g., ![Web Dashboard](docs/images/dashboard.png))

The web-based user interface and dashboard built with modern web tools.

    Tech Stack: Bun, Vite, TypeScript, Tailwind CSS
    Purpose: Serve as the control center, viewing 3D models, recordings, and analytics.

2. Backend (/backend)

The core server handling API requests, file uploads, and session management.

    Tech Stack: Python
    Purpose: Provide API endpoints and manage static media like 3D models (/3dmodels), preview states (/previews), and video components (/recordings).

3. Firmware (/firmware)

Embedded C++ and Arduino code designed for the ESP32-S3 architecture.

    Tech Stack: C++, Arduino Framework, LVGL (Light and Versatile Graphics Library)
    Purpose: Drive the M5Stack CoreS3 SE UI, communicate over WiFi/UART, interfaces with various sensors (I2C/SPI), and renders real-time graphical components.

🚀 Getting Started
Prerequisites

    Backend: Python 3.9+
    Frontend: Bun or Node.js (npm/yarn)
    Hardware: USB Web Camera, Serial-compatible Robotic Arm, Conveyor setup.

1. Backend Setup (AI & Robotic Arm Services)

The backend is built with FastAPI and runs the AI vision models and serial communication.

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

    Note on AI Models: Ensure you have the necessary weights in backend/app/vision/models/ (e.g., detect.caffemodel, sr.caffemodel) before starting the vision engine.

2. Frontend Setup (React Dashboard)

The frontend is built with React, Vite, and Tailwind CSS.

# Navigate to the frontend directory
cd frontend

# Install dependencies using Bun (recommended) or npm
bun install
# or: npm install

# Start the Vite development server
bun run dev
# or: npm run dev

Visit http://localhost:5173 (or the port specified by Vite) in your browser to access the Control Panel.
🔌 Hardware Configuration
Connecting the Robotic Arm

    Connect your corresponding Robotic Arm to the machine via USB/Serial.
    The serial_service.py is configured to communicate with the hardware automatically. Update the COM port configurations in backend/app/core/config.py if needed.
    Use the Frontend Control Panel to manually jog the arm and verify the connection.

Arm Calibration
🏗️ Project Structure

    /backend: Python FastAPI application containing endpoints (routes.py), WebSocket streams, Serial commands (arm_service.py), and Computer Vision node (vision_engine.py).
    /frontend: React/Vite application containing interactive UI components, context providers, and real-time visualization controls.
    /app/vision/models: Pre-trained AI models (.caffemodel, .prototxt) used by OpenCV for spatial recognition.

🤝 Contributing

    Fork the Project
    Create your Feature Branch (git checkout -b feature/AmazingFeature)
    Commit your Changes (git commit -m 'Add some AmazingFeature')
    Push to the Branch (git push origin feature/AmazingFeature)
    Open a Pull Request

    Bun or Node.js for the Frontend.
    Python 3.10+ for the Backend.
    PlatformIO or Arduino IDE (with ESP32 board support) for the Firmware.

Running the Frontend

cd frontend
bun install
bun run dev

Running the Backend

cd backend
python -m venv .venv
# Activate environment (Windows: .venv\Scripts\activate | macOS/Linux: source .venv/bin/activate)
pip install -r requirements.txt
python main.py

Flashing the Firmware

Depending on your toolchain of choice, open the /firmware directory in your IDE, verify your board is set to M5Stack CoreS3 / ESP32-S3, compile, and push to the device via USB Type-C.
📄 License

Distributed under the MIT License. See LICENSE for more information.

[Add License Details Here]
