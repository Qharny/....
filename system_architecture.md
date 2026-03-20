# System Architecture
## AI-Driven Platform and Hardware System for Ghanaian Agriculture

---

```mermaid
flowchart TD
    subgraph FIELD["🌱 Hardware / Field Layer"]
        SENSORS[Soil, Temperature,\nNPK & Humidity Sensors]
        ESP[ESP32 Microcontroller]
        SENSORS --> ESP
    end

    subgraph COMM["📡 Communication Layer"]
        ESP -->|Wi-Fi / Cellular| CLOUD
        ESP -->|SMS| PHONE[📱 Farmer's Mobile Phone]
    end

    subgraph CLOUD["☁️ Cloud Backend\nGoogle Compute Engine + Supabase"]
        API[API Server]
        DB[(Database)]
        API <--> DB
    end

    subgraph AI["🤖 AI / ML Layer"]
        YP[Yield Prediction]
        PD[Pest & Disease Detection]
        REC[Input Recommendations]
    end

    subgraph APP["💻 Platform / Frontend"]
        DASH[Web Dashboard]
        FARM[Farm Records]
        TEST[Test Environment]
    end

    CLOUD --> AI
    AI --> APP
    CLOUD --> APP

    FARMER[👨‍🌾 Farmer] -->|Uploads crop photo| PD
    FARMER -->|Views dashboard| APP
```

---

## Layer Overview

| Layer | Components | Role |
|---|---|---|
| **Hardware** | ESP32, Soil/NPK/Temp/Humidity Sensors | Collect real-time field data |
| **Communication** | Wi-Fi, SMS Gateway | Transmit data to cloud; alert farmers |
| **Cloud Backend** | GCE, Supabase, REST API | Store data and serve the platform |
| **AI / ML** | Yield Prediction, Pest Detection, Recommendations | Generate intelligent insights |
| **Platform** | Web Dashboard, Farm Records, Test Environment | Farmer-facing interface |
