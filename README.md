# 🛡️ Project Sentinel: Autonomous Air Defense Turret

![Vitality Technologies](https://img.shields.io/badge/Developed_Under-Vitality_Technologies-blue?style=for-the-badge)

![Status](https://img.shields.io/badge/Status-Active_Development-success?style=for-the-badge)

![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-FF0000?style=for-the-badge&logo=yolo&logoColor=white)
![ESP32](https://img.shields.io/badge/ESP32_FreeRTOS-E7352C?style=for-the-badge&logo=espressif&logoColor=white)

## 📌 Project Overview
**Project Sentinel** is an advanced mechatronics, embedded systems, and computer vision initiative developed under **Vitality Technologies**. 

The primary objective is to design, build, and program an autonomous, closed-loop air defense turret capable of visually detecting, classifying, and kinetically intercepting aerial targets (simulated by balloons representing F16s, Helicopters, Missiles, and Drones) at ranges of 5m, 10m, and 15m[cite: 5]. To maximize efficiency and adhere to strict competition-grade constraints, the entire physical assembly remains within a $\le 60\text{ cm}$ volumetric envelope[cite: 5].

## ⚙️ System Architecture

The system is split into a High-Level AI perception pipeline and a Low-Level hard real-time control system:

### 1. Vision & Edge AI Pipeline (High-Level)
* **Compute:** Powered by an RTX 4060 GPU running a custom-trained YOLOv8 model with TensorRT optimization for high-FPS target detection and classification (140-200+ FPS)[cite: 8].
* **Sensor:** Utilizes a Global Shutter USB 3.0 Camera to eliminate rolling-shutter distortion during rapid tracking[cite: 5].
* **Identify Friend or Foe (IFF):** Implements OpenCV color-space filtering (HSV) to distinguish hostile targets (Red) from friendly units (Blue)[cite: 5].
* **Tracking Logic:** Computes bounding box centroids, calculates pixel error ($\Delta X, \Delta Y$), determines ballistic lead angles for moving targets, and transmits coordinates and firing flags over a custom 10-byte binary UART protocol[cite: 5, 8].

### 2. Embedded Real-Time Control (Low-Level)
* **Microcontroller:** ESP32 DevKit V1 (Dual-Core) running FreeRTOS for asymmetric task allocation[cite: 5, 8].
* **Core 0:** Dedicated to parsing incoming serial packets from the host and monitoring hardware Emergency Stop interrupts[cite: 5].
* **Core 1:** Dedicated to generating microsecond-accurate, jitter-free STEP and DIR pulses for the motor drivers and handling the precise millisecond timing for the firing MOSFET[cite: 5].

### 3. Motion & Kinematics (Gimbal System)
* **Pan Axis (Yaw):** Driven by a NEMA 23 Closed-Loop Stepper Motor (MKS Servo57C) via a 3:1 GT2 belt reduction, rotating on an aluminum turntable bearing[cite: 5].
* **Tilt Axis (Pitch):** Driven by a NEMA 17 Integrated Closed-Loop Stepper Motor (MKS Servo42C), providing $0^\circ$ to $+60^\circ$ elevation control via a 3:1 GT2 reduction[cite: 5].

### 4. Kinetic Firing Mechanism
* **Hardware Core:** Harvested CYMA Sport M4 Carbine 10" AEG (Version 2 Metal Gearbox) firing 0.20g/0.25g 6mm biodegradable BBs[cite: 5].
* **Actuation:** Mechanical trigger contacts are bypassed; the ESP32 controls a high-current IRLB3034 / IRF1404 power MOSFET to execute programmatic single shots (75 ms pulse) or continuous bursts[cite: 5].
* **Ammunition Feed:** Magazine winding is automated using an integrated MG90S micro-servo[cite: 5].

## 🛠️ Tech Stack & Tools
* **Software:** C/C++ (ESP-IDF, FreeRTOS), Python (OpenCV, PyTorch, PySerial).
* **AI/ML:** YOLOv8, TensorRT, CUDA 12.1.
* **CAD & Simulation:** SolidWorks (Parametric B-Rep and Motion Simulation).
* **Manufacturing:** Creality K1C 3D printing utilizing engineering-grade ASA, PETG, and PA-CF (Carbon Fiber Nylon) filaments[cite: 8].

---
*Conceptualized and engineered under **Vitality Technologies**. This repository showcases multi-disciplinary engineering bridging hardware automation and artificial intelligence.*
