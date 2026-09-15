# Luwte: a wind- and forecast-aware smart awning

**Topic proposal: Smart Connected Products**
Bram Berden & _[project partner]_ · 15 September 2026 · Status: awaiting approval

> *In de luwte* (Dutch): sheltered from the wind.

## Summary

Luwte is a model-scale motorized awning (*zonnescherm*) that looks after itself. A battery-powered ESP32 node measures wind, light and temperature and rolls the awning in or out. If a gust arrives, the node retracts the awning on the spot, without waiting for the network. Slower decisions are made in the cloud. The backend receives the measurements over LoRaWAN through The Things Network, combines them with the weather forecast, and sends commands back. It extends the awning in the morning of a hot day, before the house heats up, and keeps it in when a storm is forecast. A web dashboard shows the data and allows manual override. The node is designed for low power: it sleeps between measurements, powers the motor only while moving, and runs on an 18650 cell with a small solar panel.

## Problem and context

Awnings are exposed parts of a house, and they have two recurring problems:

1. **Wind damage.** An awning left out while nobody is home can be torn off by one strong gust. Existing wind sensors only react to the moment and ignore the forecast.
2. **Too late against heat.** People extend the awning once the room is already warm. Shading helps most *before* the sun hits the façade, and that needs the forecast and the sun's position.

Awnings are mounted outside, often without a power cable and out of WiFi range. That makes them a natural fit for a battery- or solar-powered LoRaWAN node.

## What it does

| Situation | Who decides | What happens |
|---|---|---|
| Gust above threshold | **Node (reflex)** | Retracts immediately, then reports the event |
| No cloud contact for 6 h | **Node (fail-safe)** | Retracts and waits for contact |
| Hot, sunny day forecast | **Cloud (reasoned)** | Extends in the morning when the sun reaches the façade |
| Strong wind or rain forecast | **Cloud (reasoned)** | Keeps the awning in and lowers the node's gust threshold |
| User override on the dashboard | **Cloud → node** | Moves to the requested position; auto mode paused |

The split follows from the network. LoRaWAN Class A delivers a command to the node only right after the node's next uplink. Forecast decisions can wait minutes. A gust cannot.

## How it meets the project requirements

| Requirement | Luwte |
|---|---|
| Measurement | Wind speed (cup anemometer), light (BH1750), outdoor and indoor temperature, battery voltage, awning position (encoder) |
| Actuation | DC gear motor via the TB6612FNG driver from the kit; endstops for homing |
| Via a gateway to the cloud | LoRaWAN → TTN gateway → The Things Network → MQTT → backend |
| Processing and actuation back | Forecast rules in the backend → LoRaWAN downlink → motor moves |
| Local vs cloud decisions | Gust retract and fail-safe on the node; forecast logic in the cloud |
| Power | Deep sleep, motor supply switched off when idle, adaptive reporting, 18650 + solar, measured energy budget |

## Architecture

```
 NODE (TTGO T3 LoRa32, ESP32)                                     CLOUD
 ┌──────────────────────────────┐                  ┌─────────────────────────────────────┐
 │ anemometer ─ pulse input     │                  │ TTN ──MQTT──> backend (Python)      │
 │ BH1750, BME280 ─ I²C         │   LoRaWAN 868    │                  │  ├─ Open-Meteo    │
 │ DS18B20 (indoor) ─ 1-Wire    │ ───uplink──────> │   TTN gateway    │  │  forecast API  │
 │ battery ─ ADC                │ <──downlink───── │                  │  ├─ database      │
 │ TB6612FNG → gear motor + enc │                  │                  │  └─ rules engine  │
 │ endstops ─ GPIO              │                  │ web dashboard <──┘   (charts,       │
 └──────────────────────────────┘                  │                       override)     │
                                                   └─────────────────────────────────────┘
```

### Uplink payload (13 bytes)

| Bytes | Field | Encoding |
|---|---|---|
| 0 | Status flags | position state, endstops, mode, reflex fired |
| 1 | Position | 0–100 % |
| 2–3 | Wind, average | km/h × 10, uint16 |
| 4–5 | Wind, max 3-s gust | km/h × 10, uint16 |
| 6–7 | Light | lux, uint16 (BH1750 saturates at 65 535 lx) |
| 8–9 | Outdoor temperature | °C × 10, int16 |
| 10–11 | Indoor temperature | °C × 10, int16 |
| 12 | Battery | (V − 2.5) × 100 |

A 13-byte uplink takes about **62 ms of airtime at SF7**, but about **1.6 s at SF12**. TTN's fair-use limit is 30 s of uplink airtime per day. That allows roughly 480 messages a day close to the gateway, but only about 18 far away. The node therefore adapts its report interval to its data rate, and reports sooner when something changes.

### Downlink commands (max 10 per day under TTN fair use)

| Command | Payload |
|---|---|
| `0x01` Set position | 1 byte, 0–100 % |
| `0x02` Set gust threshold | km/h × 10, uint16 |
| `0x03` Set report interval | minutes, 1 byte |
| `0x04` Set mode | auto / manual / hold |

## Power design

- **Deep sleep** between measurements; onboard OLED and LEDs switched off.
- **Wind sampling experiment:** short sampling windows (for example 3 s every 30 s) versus continuous pulse counting on the ESP32's ULP coprocessor. We measure both and compare energy use against gust reaction time.
- **Motor supply switched off** when idle: boost converter disabled, TB6612FNG in standby.
- **Report by exception:** regular heartbeat, plus an extra message only when wind, position or mode changes.
- **Energy source:** one protected 18650 Li-ion cell (the TTGO has no battery protection). A small solar panel with its own charger comes in phase 2. The TTGO's onboard charger only runs from USB, so the two charging paths are never used at the same time.
- **Measured budget:** sleep current with a µA meter, energy per awning movement with an INA219, adding up to a daily energy budget and expected battery life.

Provisional target, fixed after the baseline measurement in week 5: **≥ 7 days on one 18650 without sun**, and energy-neutral on a sunny day with the solar panel.

## Sensor accuracy and calibration

| Sensor | Plan |
|---|---|
| Anemometer | 3D-printed cups, one hall-sensor pulse per revolution. Find the pulses/s → km/h factor by calibrating against a handheld reference anemometer. Report the average and the maximum **3-second gust** (the meteorological gust definition). |
| BH1750 | 16-bit, up to 65 535 lx, while direct sun can exceed 100 000 lx. Use a diffuser, and use light mainly as "sun on the façade: yes/no", cross-checked with forecast radiation. |
| BME280 | Place in a small radiation shield (a mini Stevenson screen) so sunlight doesn't heat the sensor. |
| DS18B20 | ±0.5 °C, 12-bit resolution (0.0625 °C). |
| Battery voltage | ESP32 ADC is non-linear: calibrate with the ESP-IDF ADC calibration and compare against a multimeter. |
| Position | Count encoder pulses; home on the endstop at boot. |

## Tools

- **Firmware:** C++ with PlatformIO (Arduino framework for ESP32), LoRaWAN stack MCCI LMIC or RadioLib
- **Network:** The Things Network (v3), OTAA join
- **Backend:** Python (MQTT client, Open-Meteo API, rules engine), time-series database, Docker
- **Dashboard:** web app with charts and override controls
- **Collaboration:** Git/GitHub

## Success criteria

1. A gust above the threshold starts retraction within **5 s**, including with the gateway switched off.
2. A cloud command is executed within **one report interval**; final position within **±5 %**.
3. Wind speed within **±10 %** of the reference anemometer after calibration.
4. Energy budget measured and documented; battery life meets the target set in week 5.
5. The dashboard shows live and historic data next to the forecast, and override works.
6. The fail-safe retracts the awning after **6 h** without cloud contact.

## Planning

| Week | Milestone |
|---|---|
| 3 | Proposal approved, parts ordered |
| 4 | **Risk 1:** TTGO joins TTN (OTAA), uplink visible, downlink received |
| 5 | **Risk 2:** baseline sleep current measured; anemometer pulse counting works |
| 6 | Backend skeleton: TTN MQTT → decoder → database; Open-Meteo fetch |
| 7 | Architecture and payload frozen; all parts in; model design sketched |
| — | *Autumn break* |
| 8–9 | Motor, encoder and endstops; model awning built; gust reflex and fail-safe |
| 10–11 | Forecast rules and downlinks; dashboard with override |
| 12 | Power optimisation, solar charging, energy measurements |
| 13 | Calibration, testing against the success criteria, paper |
| 14 | Presentation, demo rehearsal, do's and don'ts |

## Task division

| | Student A: the node | Student B: the cloud |
|---|---|---|
| Focus | Firmware, sensors, motor control, LoRaWAN, power | TTN integration, backend, forecast rules, dashboard |
| Paper section | Node design and power | Cloud processing and dashboard |
| Shared | Architecture, payload format, model build, testing, paper, presentation | |

## Risks

| Risk | Mitigation |
|---|---|
| No TTN coverage where we test | Check TTN Mapper; test at the Fontys gateway; fall back to a TTN indoor gateway |
| Class A downlink delay | Time-critical logic runs on the node; the cloud sends settings, not real-time control |
| Dev board sleep current higher than the bare ESP32 | Measure in week 5; disable the OLED and LEDs; discuss a custom board in the paper |
| TB6612FNG is rated for 4.5–13.5 V motor supply, the cell gives 3.7 V | 6 V boost converter with enable pin. A disabled boost passes the battery voltage through, so the driver's STBY is held low as well. |
| No ready-made cup anemometer found at EU shops | 3D-printed cups with a hall sensor and magnet, calibrated against a reference; the SparkFun Weather Meter Kit is the fallback |
| Pololu parts ship from the US | Order them first, in week 3 |
| Mechanical model eats time | Keep it simple: dowel roller, fabric, 3D-printed brackets |
| Airtime fair use exceeded | Adaptive interval based on the data rate; report by exception |

## Out of scope

Full-size awning, native mobile app, fleets of multiple awnings (discussed in the paper as scaling), our own gateway hardware.

## Questions for the teacher

1. Is LoRaWAN/TTN required, and may we use the Fontys TTN gateway?
2. Is a model-scale awning acceptable for the demo?
3. Is there a budget for parts beyond the kit, and can we borrow a µA meter or power profiler and a reference anemometer?

## Materials

See [materials.md](materials.md).
