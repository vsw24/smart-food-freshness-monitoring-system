# Smart Food Freshness Monitoring System

**Gas-sensing array with local display and remote monitoring (ESP32 + Blynk IoT)**

An ESP32-based embedded system that detects gases associated with food spoilage, classifies food as **Fresh**, **Moderate** or **Spoiled**, and reports the result locally (OLED, buzzer, fan) and remotely (Blynk IoT app).

> **Patent:** IP India Application No. **202621053314**, published on 3 July 2026.

![Prototype](images/prototype-v2.jpg)

---

## Overview

Food spoilage releases volatile gases, and temperature and humidity change how quickly it happens. This project combines a small array of gas sensors with a temperature/humidity sensor on one ESP32. The firmware turns the readings into a freshness status, shows it on the device, triggers alerts and ventilation, and sends the data to a phone over Wi-Fi.

Two hardware iterations were built and tested, improving sensor placement, power efficiency and enclosure design.

## Features

- Multi-gas sensing array (MQ-3, MQ-4, MQ-135) plus temperature and humidity (DHT22)
- Threshold-based freshness classification: **Fresh / Moderate / Spoiled**
- Local feedback on a 128x64 OLED display
- Automatic buzzer alert when food is spoiled
- Relay-controlled exhaust fan
- Remote monitoring through the Blynk IoT app over Wi-Fi
- Two hardware versions, from prototype to a more compact unit

## Hardware

| Component | Role |
|---|---|
| ESP32-WROOM | Main controller with built-in Wi-Fi |
| MQ-3 gas sensor | Alcohol / ethanol vapours |
| MQ-4 gas sensor | Methane |
| MQ-135 gas sensor | General air quality (ammonia, NOx, benzene and similar) |
| DHT22 | Temperature and humidity |
| OLED display (I2C) | Local status and readings |
| Relay module + fan | Automatic ventilation |
| Buzzer | Audible spoilage alert |

## How it works

```
 MQ-3 / MQ-4 / MQ-135 ─┐
                        ├─► ESP32 ─► threshold logic ─► Fresh / Moderate / Spoiled
 DHT22 (temp, humidity)─┘                 │
                                          ├─► OLED display
                                          ├─► Buzzer
                                          ├─► Relay → Fan
                                          └─► Wi-Fi → Blynk IoT app
```

1. The ESP32 reads the three gas sensors and the DHT22 at a fixed interval.
2. The readings are compared against the configured thresholds.
3. The system assigns one of three states:

| State | Meaning | Response |
|---|---|---|
| Fresh | Readings within normal range | Status shown, no alert |
| Moderate | Readings rising | [ADD: what the device does, e.g. warning on display] |
| Spoiled | Readings above spoilage threshold | Buzzer on, fan on, status pushed to Blynk |

4. Status and readings are shown on the OLED and sent to Blynk.

**Thresholds used:** [ADD your actual threshold values per sensor, and how you chose them]

## Wiring

[ADD circuit diagram image here: `images/circuit-diagram.png`]

| Module | ESP32 pin |
|---|---|
| MQ-3 (analog out) | [ADD pin] |
| MQ-4 (analog out) | [ADD pin] |
| MQ-135 (analog out) | [ADD pin] |
| DHT22 (data) | [ADD pin] |
| OLED SDA / SCL | [ADD pins] |
| Relay (fan) | [ADD pin] |
| Buzzer | [ADD pin] |

## Hardware versions

| | Version 1 | Version 2 |
|---|---|---|
| Photo | ![V1](images/prototype-v1.jpg) | ![V2](images/prototype-v2.jpg) |
| Changes | [ADD] | [ADD: sensor fusion, power efficiency and enclosure improvements] |

## Software setup

**Tools:** Arduino IDE with the ESP32 board package.

**Libraries:** [ADD the exact libraries from your sketch, e.g. Blynk, DHT sensor, OLED display library]

1. Open `code/` in the Arduino IDE.
2. Install the libraries listed above.
3. Create a Blynk template and device, and copy your own credentials into the sketch.
4. Enter your own Wi-Fi name and password in the sketch.
5. Select the ESP32 board and port, then upload.

> **Never commit real Wi-Fi passwords or Blynk auth tokens.** Use placeholders in the repository.

## Blynk dashboard

![Blynk dashboard](images/blynk-dashboard.png)

[ADD: which virtual pins show what, e.g. gas values, temperature, humidity, status]

## Results

![OLED output](images/oled-display.jpg)

[ADD: what you tested (foods, conditions), what the system showed for fresh vs spoiled samples, and response time]

## Limitations

- MQ-series sensors are cross-sensitive to several gases and drift with temperature, humidity and age, so they need warm-up time and calibration.
- Freshness is estimated by thresholds tuned for the tested conditions. It is an indicator, not a laboratory-grade measurement.

## Future scope

- Support for multiple food categories with separate profiles
- Smarter software-side classification using the existing hardware
- Data logging and trend analysis

## Repository structure

```
.
├── code/           Arduino firmware
├── images/         Photos, circuit diagram, dashboard screenshots
└── README.md
```

## Author

**Viraj Wadatkar**
B.Tech, Instrumentation and Control Engineering, VIT Pune
[LinkedIn]([ADD your LinkedIn profile URL])

## License

MIT License
