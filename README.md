# Smart Connected Products

Coursework for the Fontys elective *Smart Connected Products* (SCP, 5 EC).

**Project website:** <https://b-brd.github.io/smart-connected-products/>

## Repository layout

```
.
├── README.md
├── docs/                   # project website (GitHub Pages): index.html + web-sized images and animation
├── project/                # our project: Luwte, a wind- and forecast-aware smart awning
│   ├── proposal.md
│   ├── materials.md
│   └── design/             # concept render, 3D model (.glb) and animation; binaries via Git LFS
└── course/                 # course material, local only (git-ignored)
    ├── module-description.pdf
    ├── lectures/           # 00 preliminary · 01 introduction · 02 sensors · 03 gateway · 04 connectivity
    ├── project/            # project brief, assessment info, grading form
    └── embedded/           # TTGO/elevator lab manual, kit component list, supplier list
```

## Project requirements (summary)

- Work in pairs on a self-chosen topic, which must be approved before development starts.
- The system **measures** something (temperature, speed, light, …) **and actuates** something (LED, fan, motor, switch, …).
- Data goes **through a gateway to the cloud**, is processed there, and actuation comes back the same way. Local "reflex" decisions are allowed next to cloud "reasoned" decisions.
- **Power consumption** must be designed for and justified.
- Reference hardware: LilyGO TTGO T3 LoRa32 868 MHz (ESP32), with optional MCP23017 I/O expander and TB6612FNG motor driver.

### Planning

| Weeks | Focus |
|-------|-------|
| 1–7   | Define goal/topic, tackle risky parts early (LoRaWAN / The Things Network), order components before autumn break |
| 8–14  | Build the system, test, document |

### Deliverables

- Working product + demo
- Presentation (10–15 min) + defence. Cover context, IoT fit, architecture and protocols, power, hardware/languages/tooling, sensor accuracy and ADC conversion, implementation, demo, lessons learned, and do's and don'ts.
- Paper in two-column article form: abstract, introduction, technologies, architecture, discussion, individual work, conclusion, references
- Development artefacts (code, drawings, architecture)

### Grading criteria

| | |
|---|---|
| LG1 | Apply sensors/actuators |
| LG2 | Connected application |
| LG3 | Realize apps, dashboard |
| LG4 | Motivation of solution and choices |
