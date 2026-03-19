# AI-Driven Platform and Hardware System for Ghanaian Agriculture
## System Architecture

---

## Architecture Diagram

```mermaid
flowchart TD
    subgraph HW["🌱 Hardware Layer (IoT Field Unit)"]
        S1[Soil Moisture Sensor]
        S2[Temperature Sensor]
        S3[NPK Sensor]
        S4[Humidity Sensor]
        ESP[ESP32 Microcontroller]
        PWR[Battery Power Supply]

        S1 --> ESP
        S2 --> ESP
        S3 --> ESP
        S4 --> ESP
        PWR -.-> ESP
    end

    subgraph COMM["📡 Communication Layer"]
        WIFI[Wi-Fi / Cellular]
        SMS_GW[SMS Gateway]
        ESP --> WIFI
        ESP --> SMS_GW
    end

    subgraph CLOUD["☁️ Cloud & Backend Layer (Google Compute Engine + Supabase)"]
        DB[(Cloud Database\nSupabase)]
        API[REST API Server]
        WIFI --> API
        API <--> DB
    end

    subgraph AI["🤖 AI / ML Processing Layer"]
        YP[Yield Prediction Model\nRandom Forest / XGBoost]
        PD[Pest & Disease Detection\nCNN / YOLOv8]
        REC[Input Recommendation\nEngine]
        DB --> YP
        DB --> PD
        DB --> REC
    end

    subgraph APP["💻 Application Layer"]
        WEB[Web Dashboard\nFarm Records & Analytics]
        FARM[Farm Data Management\nCrop Records & History]
        TEST[Controlled Test Environment\nPre-Season Simulation]
        AI_ADV[AI Advisory Interface\nRecommendations & Alerts]

        API --> WEB
        API --> FARM
        API --> TEST
        YP --> AI_ADV
        PD --> AI_ADV
        REC --> AI_ADV
        AI_ADV --> WEB
    end

    subgraph FARMER["👨‍🌾 Farmer Interface"]
        PHONE[Basic Mobile Phone\nSMS Alerts]
        BROWSER[Web Browser\nDashboard Access]
        CAM[Camera / Image Upload\nPest & Disease Photos]

        SMS_GW --> PHONE
        WEB --> BROWSER
        CAM --> PD
    end

    style HW fill:#e8f5e9,stroke:#388e3c,color:#000
    style COMM fill:#e3f2fd,stroke:#1976d2,color:#000
    style CLOUD fill:#fff3e0,stroke:#f57c00,color:#000
    style AI fill:#fce4ec,stroke:#c2185b,color:#000
    style APP fill:#ede7f6,stroke:#7b1fa2,color:#000
    style FARMER fill:#f1f8e9,stroke:#558b2f,color:#000
```

---

## Component Descriptions

### 1. Hardware Layer (IoT Field Unit)
The physical sensing unit deployed in the farm or controlled test environment. An **ESP32 microcontroller** (USD 3–8) aggregates readings from:
- **Soil Moisture Sensor** — detects water levels to trigger irrigation alerts
- **Temperature Sensor** — monitors ambient and soil temperature
- **NPK Sensor** — measures Nitrogen, Phosphorus, and Potassium levels
- **Humidity Sensor** — tracks atmospheric moisture

The ESP32 was selected for its dual-core processing, built-in Wi-Fi/Bluetooth, and low power draw (~240 mA), making it ideal for resource-constrained field deployments.

---

### 2. Communication Layer
- **Wi-Fi / Cellular** — transmits sensor data to the cloud backend with latency as low as 1.18 seconds and uptime above 99%.
- **SMS Gateway** — sends real-time threshold alerts (e.g., critically low soil moisture) directly to farmers' basic mobile phones, requiring no internet connection.

---

### 3. Cloud & Backend Layer
- **Google Compute Engine** — hosts the application server and AI inference services.
- **Supabase** — provides the cloud database for storing farm records, sensor readings, user data, and historical seasonal data.
- **REST API Server** — mediates communication between the frontend, database, and AI models.

---

### 4. AI / ML Processing Layer
| Module | Algorithm(s) | Purpose |
|---|---|---|
| Yield Prediction | Random Forest, XGBoost, LSTM | Forecast crop yields from soil, climate, and management data |
| Pest & Disease Detection | CNN (MobileNet, ResNet), YOLOv8 | Classify and locate pests/diseases from uploaded images |
| Input Recommendation Engine | Random Forest, Gradient Boosting | Recommend seed variety, fertilizer type, and planting time |

---

### 5. Application Layer
- **Web Dashboard** — visualises sensor readings, farm analytics, and AI recommendations.
- **Farm Data Management** — enables digital recording of crop records, seasonal history, and financial data.
- **Controlled Test Environment** — allows farmers to simulate crop seasons and input choices before committing real resources.
- **AI Advisory Interface** — aggregates outputs from all AI modules into actionable, farmer-friendly guidance.

---

### 6. Farmer Interface
- **Basic Mobile Phone (SMS)** — receives real-time sensor alerts and weekly advisory summaries; works without internet or smartphones.
- **Web Browser** — provides access to the full dashboard for farmers or extension officers with internet access.
- **Camera / Image Upload** — farmers photograph crop symptoms; images are sent to the pest and disease detection model.

---

## Data Flow Summary

```
Field Sensors → ESP32 → Wi-Fi → Cloud API → Database
                       ↓
                  SMS Gateway → Farmer's Phone

Database → AI Models → Recommendations → Web Dashboard → Farmer
Farmer Camera → Disease Detection Model → Diagnosis + Guidance
```

---

## Key Design Principles
- **Affordability** — built on low-cost hardware (ESP32) and open/affordable cloud tools
- **Offline-friendly** — SMS alerts function without internet; designed for low-connectivity rural Ghana
- **Locally adapted** — AI models trained on Ghanaian crop and soil data (tomato, cabbage, maize)
- **Integrated** — combines IoT sensing, farm records, yield prediction, pest detection, and advisory in one platform
- **Accessible** — web dashboard for connected users; SMS fallback for all others
