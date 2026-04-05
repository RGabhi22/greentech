# 🌱 Green Tech: Edge-Computed Crop Monitoring

An end-to-end, localized agricultural monitoring Progressive Web App (PWA). This system captures real-time video from an ESP32 edge server, processes it locally using YOLOv8 for threat detection, and features an AI agronomist powered by the Groq Llama-4-Scout model.

Designed for zero-latency edge environments, it completely bypasses third-party cloud IoT limits by relying on WebSockets, local SQLite, and strict concurrency.

## 🚀 Key Features
* **Zero-Latency Real-Time Alerts:** Bidirectional WebSockets trigger massive, auto-resolving lock-screen alerts on the React frontend the millisecond a fire or intruder is verified.
* **YOLOv8 Anti-Spam State Machine:** Custom 3.0-second grace-period logic prevents system spam from camera micro-blinks or dropped frames.
* **Telegram Image Attachments:** The backend intercepts the YOLO frame and dynamically pushes annotated image evidence to a Telegram bot when a threat is verified.
* **Pan/Tilt Servos:** Control camera orientation directly from the React dashboard via REST endpoints routed to the ESP32.
* **Local Fallback Mechanics:** OpenCV automatically fails over to local webcam hardware if the ESP32 mDNS stream drops.
* **AI Agronomist:** A dedicated chat interface piping base64 clean frames and SQLite sensor history to `meta-llama/llama-4-scout-17b-16e-instruct` for actionable insights.

## 🧠 System Architecture

```mermaid
graph TD
    E[ESP32 Local Server] -->|Sensor Data JSON| API(Python FastAPI)
    E -->|MJPEG Stream| CV(OpenCV + YOLOv8 Thread)
    CV -->|State Change| SM{Persistence Logic}
    API <-->|Polls & Stores| DB[(SQLite Local DB)]
    SM -->|WebSocket| UI[React Frontend]
    SM -->|Alert + Image| TG[Telegram API]
    API -->|Base64 Frame| LLM[Groq Vision API]
