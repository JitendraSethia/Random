# SIH 2026 — 26178
# Low-Cost Hierarchical Edge-AI Environmental Intelligence Network

> **Working execution plan**
>
> **Budget target:** ~₹4,000 out-of-pocket, assuming we can borrow a Snapdragon Android phone and use existing laptops/tools.
>
> **Prototype focus:** Fire/smoke + flood/rising water, supported by low-cost environmental anomaly sensing.
>
> **Core idea:** Tiny sensor nodes perform local sensing/anomaly detection; a Snapdragon Android phone acts as the edge gateway, combining camera intelligence + sensor evidence into an explainable risk decision that continues working when internet connectivity is lost.

---

## 1. What are we actually building?

We are **not** trying to build a complete commercial disaster-management product in a few weeks.

We are building a **working proof-of-concept of a resilient environmental intelligence network**.

```text
                 ENVIRONMENT
                     │
          ┌──────────┼──────────┐
          │          │          │
        FIRE       FLOOD      WEATHER/
       SIGNALS    /WATER       AIR
          │          │          │
          ▼          ▼          ▼
       SENSOR      SENSOR      SENSOR
        NODE        NODE        DATA
          │          │          │
          └────── LoRa / Wi-Fi ──────┐
                                     │
                                     ▼
                         ┌────────────────────┐
                         │ SNAPDRAGON PHONE   │
                         │                    │
                         │ Camera AI          │
                         │ Sensor fusion      │
                         │ Risk engine        │
                         │ Local storage      │
                         │ Alert engine       │
                         └─────────┬──────────┘
                                   │
                         Internet available?
                           /              \
                         YES               NO
                          │                 │
                          ▼                 ▼
                     Dashboard          Local alert
                     + cloud            + local storage
```

### Mental model

- **Sensor nodes = ears and nose**
- **Camera = eyes**
- **LoRa/Wi-Fi = nervous system**
- **Snapdragon phone = brain**
- **Risk engine = decision maker**
- **Dashboard = control room**

---

## 2. Why this architecture?

The SIH problem is broader than “detect fire.” It calls for a distributed environmental monitoring network with local intelligence, multi-hazard awareness, alerts, risk mapping, resilience during connectivity failures, and scalable communication.

Existing companies already solve individual pieces of this problem: environmental IoT monitoring, wildfire sensing, camera-based wildfire detection, flood/water-level monitoring, cloud dashboards and LoRa networks.

Therefore our claim is **not**:

> “We invented fire detection.”

Our defensible claim is:

> **We are demonstrating a low-cost, hierarchical edge-intelligence architecture designed for resource-constrained deployment: inexpensive sensor nodes make local decisions, while a Snapdragon edge gateway fuses sensor evidence with visual AI to produce explainable, offline-capable disaster risk decisions.**

---

# 3. What we will demonstrate

## Primary hazard 1 — Fire / smoke

The gateway camera watches a controlled demonstration area.

The camera produces a visual smoke/fire probability. An environmental node independently observes changes such as gas/smoke response and temperature/humidity trends.

Example:

```text
Camera smoke probability       0.91
Gas anomaly                    0.88
Temperature trend              0.82
Humidity trend                 0.79

Final fire risk                0.94
Decision                       CRITICAL
```

The system should then:

- trigger a local alarm;
- display the event;
- show the contributing evidence;
- store the event locally;
- continue functioning when internet is disconnected.

## Primary hazard 2 — Flood / rising water

A camera looks at a marked staff gauge / water-level scale. The vision system estimates the current level and trend. A second sensor independently measures water level or detects abnormal water presence.

Example:

```text
Visual water level             42 cm
Rate of rise                   +4.2 cm/min
Water sensor anomaly           0.91
Rain/environment anomaly       0.78

Final flood risk               HIGH
```

The dashboard must show **why** the system reached that decision.

## Supporting capability — environmental anomaly sensing

We will **not** pretend hobby sensors are regulatory-grade environmental instruments.

Instead, we demonstrate:

> **low-cost environmental anomaly detection**

Examples: temperature, humidity, pressure and smoke/gas response.

Where appropriate, the dashboard will label these as **prototype/anomaly indicators**, not certified environmental measurements.

---

# 4. Scope firewall — what we are NOT building

We will explicitly avoid:

- seven physical hazards;
- an expensive Qualcomm development board;
- industrial-grade sensors;
- a full commercial LoRaWAN infrastructure;
- a large cloud platform;
- a huge custom AI model;
- a production-grade Android app;
- solar-powered field deployment in the first prototype;
- regulatory-grade PM2.5/PM10 measurements;
- false precision from cheap MQ sensors.

Our objective is a **strong, measurable, working architecture**, not maximum component count.

---

# 5. System architecture

## Layer A — Sensor nodes

Each node contains:

- ESP32;
- environmental sensor(s);
- hazard-specific sensor;
- radio;
- local filtering/anomaly logic.

The node should not continuously dump every raw reading to the gateway.

Instead:

```text
Raw sensor data
      ↓
Filtering
      ↓
Feature extraction
      ↓
Local anomaly score
      ↓
Send event / summary
```

Example message:

```json
{
  "node_id": "NODE_01",
  "timestamp": 1724567890,
  "temperature": 34.2,
  "humidity": 52.7,
  "gas_anomaly": 0.88,
  "local_anomaly": 0.81,
  "status": "WARNING"
}
```

The payload can be simplified during the first prototype.

---

## Layer B — Snapdragon edge gateway

The Android phone is the central intelligent gateway.

It provides:

- camera;
- Snapdragon compute;
- AI inference;
- local storage/database;
- network connectivity;
- optional GPS;
- alert generation.

### Gateway responsibilities

1. **Camera inference** — smoke/fire analysis.
2. **Water-level vision** — read gauge / estimate waterline.
3. **Receive sensor messages.**
4. **Sensor fusion** — combine independent evidence.
5. **Risk scoring** — NORMAL / WATCH / WARNING / CRITICAL.
6. **Offline operation** — keep sensing, inference, alerts and local storage working without internet.
7. **Synchronization** — upload buffered events when connectivity returns.

---

## Layer C — Dashboard

Keep the dashboard simple and operational.

Show:

```text
REGION STATUS
🟢 NORMAL

NODE 01   ONLINE
NODE 02   ONLINE
CAMERA    ONLINE
GATEWAY   ONLINE

Temperature     31.2°C
Humidity        64%
Water level     22 cm
Gas anomaly     0.07

🔥 FIRE       08%   NORMAL
🌊 FLOOD      12%   NORMAL
```

During an event:

```text
🔴 FIRE EVENT

Confidence: 94%

WHY?
✓ Smoke detected visually
✓ Gas anomaly detected
✓ Temperature rising
✓ Multiple sources agree

Network: OFFLINE
Decision: LOCAL
```

The **WHY** section is a core feature.

---

# 6. AI strategy

## Principle

**Use AI where it is useful, not everywhere.**

### AI component 1 — Visual smoke/fire detection

```text
Camera frame
    ↓
Resize / preprocess
    ↓
Lightweight model
    ↓
Smoke/fire probability
```

Do not start by training a giant model from scratch. First benchmark a lightweight existing approach against our own footage.

### AI component 2 — Water-level vision

```text
Camera
  ↓
Gauge region
  ↓
Waterline detection
  ↓
Pixel-to-height conversion
  ↓
Water level + trend
```

This can initially be a classical CV pipeline rather than a neural network.

### AI component 3 — Local sensor anomaly detection

```text
temperature
humidity
pressure
gas signal
water measurement
       ↓
feature extraction
       ↓
small anomaly model / statistical detector
       ↓
0–1 anomaly score
```

Do not force a large neural network onto the ESP32.

### AI component 4 — Sensor fusion

Start with a transparent weighted model.

Example:

```text
fire_risk =
    0.35 × visual_smoke
  + 0.25 × gas_anomaly
  + 0.20 × temperature_trend
  + 0.20 × humidity_trend
```

Starting thresholds:

```text
risk >= 0.80  → CRITICAL
risk >= 0.55  → WARNING
risk >= 0.30  → WATCH
risk <  0.30  → NORMAL
```

These are **starting values**, not scientific calibration. Tune them using our own test data.

---

# 7. False-alarm strategy

This is a major judging opportunity.

### Visual-only anomaly

```text
Camera smoke           0.91
Gas anomaly            0.07
Temperature anomaly    0.04

Result:
🟡 VISUAL ANOMALY
Awaiting corroboration
```

### Corroborated event

```text
Camera smoke           0.91
Gas anomaly            0.88
Temperature trend      0.82
Humidity trend         0.79

Result:
🔴 FIRE RISK 94%
```

The system should demonstrate that it does not blindly trust one sensor.

---

# 8. Communication architecture

## Development phase

Start with:

```text
ESP32 → Wi-Fi → phone/laptop
```

This lets us debug sensing, payloads, gateway software and the dashboard before RF complexity is introduced.

## Final prototype

```text
ESP32
  ↓
LoRa
  ↓
Gateway receiver
  ↓
Snapdragon phone
```

For India-oriented deployment, the final radio design must use an appropriate **865–867 MHz / IN865-compatible** solution rather than blindly using 433 MHz hobby modules.

---

# 9. Hardware plan

## Borrowed / existing

### 1 × Snapdragon Android phone

**Target cost: ₹0**

Before committing, record:

```text
Phone model:
Snapdragon chipset:
Android version:
Camera:
AI Hub/device support:
Potential NPU/accelerator path:
```

Do not assume that a phone being branded “Snapdragon” guarantees our model will run on the NPU.

## Sensor hardware

### 2 × ESP32 development boards

Target: **~₹600 total**

### 2 × BME280

Temperature / humidity / pressure.

Target: **~₹400 total**

### 1–2 × low-cost smoke/gas sensors

MQ-series or similar.

Target: **~₹150–₹300 total**

Use only as a relative/anomaly signal.

### 1 × water-level sensing component

Choose the simplest reliable part for the demo.

Target: **~₹100–₹300**

### 2 × appropriate LoRa radios

Target: **~₹1,300–₹1,600 total**

Select only after checking:

- frequency/regional compatibility;
- ESP32 library support;
- antenna availability;
- gateway integration.

### Miscellaneous

Wires, connectors, perfboard, LEDs, buzzer and simple enclosure material: **~₹600–₹800**.

---

# 10. Target ₹4,000 BOM

| Category | Target |
|---|---:|
| ESP32 ×2 | ₹600 |
| BME280 ×2 | ₹400 |
| MQ sensor(s) | ₹150–₹300 |
| Water sensor | ₹100–₹300 |
| LoRa radios ×2 | ₹1,300–₹1,600 |
| Antennas | ₹150–₹250 |
| Wiring / connectors / prototyping | ₹300–₹400 |
| LEDs / buzzer | ₹100 |
| Enclosure/demo material | ₹200 |
| Spare budget | ₹300 |
| **Target total** | **~₹3,600–₹4,000** |

Prices are planning estimates, not a purchase quotation. Confirm the exact component and seller before ordering.

---

# 11. Development sequence

## Phase 0 — Freeze the environment

### Goal
Know what hardware/software we actually have.

### Tasks

- identify available Snapdragon phone(s);
- record exact chipset and Android version;
- verify AI Hub/device support;
- inventory existing ESP32s/sensors/tools;
- select exact LoRa module;
- freeze procurement list.

### Exit condition
A written hardware inventory + final purchase list.

---

## Phase 1 — Sensor node MVP

### Goal
One ESP32 reads sensors reliably.

### Deliverable

```text
ESP32
  ↓
BME280 / smoke / water
  ↓
structured readings
```

### Test
Run for several hours and prove stable readings, correct units, timestamps and predictable behavior.

---

## Phase 2 — Communication MVP

### Goal
Make distributed nodes talk to the gateway.

First:

```text
ESP32 → Wi-Fi → phone/laptop
```

Then:

```text
ESP32 → LoRa → receiver
```

Measure:

- packet success rate;
- latency;
- usable range in our environment;
- behavior under packet loss.

Do not invent range numbers. Measure them.

---

## Phase 3 — Gateway data pipeline

### Goal
The phone receives, stores and displays node information.

```text
Node
 ↓
Gateway
 ↓
Local database
 ↓
Dashboard
```

Remove the internet and prove local ingestion still works.

---

## Phase 4 — Fire vision

### Goal
Run a lightweight smoke/fire vision model or pipeline on the Snapdragon phone.

Tasks:

- collect controlled smoke/fire footage;
- collect negative examples;
- benchmark candidate lightweight models;
- measure latency;
- verify the actual execution path;
- record honest metrics.

Deliverable:

```text
Live camera
     ↓
Smoke probability: 0–1
```

---

## Phase 5 — Flood vision

### Goal
Read a marked water-level gauge.

Build:

```text
transparent container
+
marked gauge
+
controlled water level
+
camera
```

Calibrate:

```text
pixel coordinate → centimetres
```

Deliver:

```text
Water level: 42 cm
Trend: +4.2 cm/min
```

with repeatable tests.

---

## Phase 6 — Sensor fusion

### Goal
Combine independent signals into a transparent risk decision.

Test:

- true event;
- visual-only false alarm;
- sensor-only anomaly;
- missing sensor;
- noisy data.

---

## Phase 7 — Offline resilience

### Test sequence

1. Start normally.
2. Disconnect internet.
3. Trigger event.
4. Confirm local inference.
5. Confirm local alert.
6. Store event locally.
7. Restore internet.
8. Synchronize stored event.

This should become one of the main demo moments.

---

## Phase 8 — Dashboard + map

Only after the core system works.

Show:

- node positions;
- node status;
- current measurements;
- fire risk;
- flood risk;
- recent alerts;
- confidence;
- contributing evidence;
- connectivity state.

Avoid unnecessary UI work.

---

## Phase 9 — Integration

```text
NODE 01 ─┐
         │
NODE 02 ─┼─→ GATEWAY → AI → FUSION → ALERT
         │                    │
CAMERA ──┘                    └→ DASHBOARD
```

Then freeze the architecture. Only reliability improvements should be allowed after this point.

---

## Phase 10 — Stress testing

Intentionally break the system:

- internet loss;
- sensor disconnect;
- packet loss;
- noisy sensor;
- camera obstruction;
- lighting change;
- false smoke-like scene;
- invalid sensor value;
- gateway restart;
- node restart;
- multiple events.

The system should be able to explain how it fails.

---

# 12. Team structure for two people

Do **not** split the project into two isolated halves immediately. First, both people should understand the whole architecture.

## You — System / AI / Integration Lead

Own:

- overall architecture;
- AI and vision;
- sensor-fusion logic;
- gateway;
- Qualcomm integration;
- backend integration;
- final integration;
- judging/demo narrative.

## Friend — Hardware / Embedded / Network Lead

Own:

- ESP32 nodes;
- sensor wiring;
- embedded code;
- communication;
- LoRa;
- node reliability;
- physical demo setup;
- basic power/enclosure work.

## Shared

Both must understand:

- why the project exists;
- system architecture;
- data flow;
- demo flow;
- limitations;
- failure modes.

---

# 13. Friend onboarding session

Explain the project in this order:

1. **Problem:** disasters are difficult to monitor continuously, especially when connectivity is unreliable.
2. **Idea:** cheap distributed nodes observe the environment.
3. **Why nodes are cheap:** they do basic sensing and local anomaly detection.
4. **Why the gateway is stronger:** a Snapdragon phone combines camera + compute + connectivity + storage.
5. **Why fusion matters:** one modality can false-alarm; independent evidence can corroborate it.
6. **Why offline matters:** connectivity may fail exactly when an alert matters.
7. **What we physically demonstrate:** two nodes + camera + Snapdragon gateway + dashboard + offline alert.

Then trace one complete event on paper:

```text
Sensor event
    ↓
ESP32
    ↓
Radio
    ↓
Gateway
    ↓
AI / fusion
    ↓
Risk
    ↓
Alert
    ↓
Dashboard
```

Both people should be able to explain this without notes before specialization.

---

# 14. Final 3-minute demo

## 0:00–0:25 — Explain

> “This is a distributed environmental intelligence system designed for low-cost deployment. Sensor nodes monitor the environment while the Snapdragon edge gateway combines those observations with camera intelligence.”

## 0:25–0:50 — Normal state

Show nodes online, normal readings, camera normal, dashboard green.

## 0:50–1:20 — Smoke event

Introduce controlled smoke. Show the visual anomaly without immediately declaring a confirmed fire if corroboration is absent.

## 1:20–1:40 — Confirmed fire event

Environmental evidence changes. Show:

```text
Camera + sensor evidence
       ↓
Risk fusion
       ↓
94% fire risk
       ↓
Alarm
```

## 1:40–2:10 — Cut internet

Disconnect internet. Trigger another event.

Show:

```text
Internet: OFFLINE
AI: RUNNING
Alert: TRIGGERED
Event: STORED LOCALLY
```

## 2:10–2:40 — Flood

Pour water into the controlled gauge. Camera estimates level. Water sensor independently confirms the change.

## 2:40–3:00 — Close

Show:

- prototype cost;
- offline capability;
- distributed architecture;
- modular hazard support;
- measured performance.

Closing line:

> “The goal is not to replace every specialized disaster-monitoring system. It is to demonstrate how a low-cost edge-intelligence architecture can combine distributed sensing, visual evidence and local decision-making into a deployable early-warning prototype.”

---

# 15. Metrics we MUST measure

Do not use vague claims.

Create a final results table:

| Metric | Result |
|---|---|
| Sensor packet success | Measure |
| Node-to-gateway latency | Measure |
| Fire inference latency | Measure |
| Water-level error | Measure |
| False alarm rate | Measure |
| Event-to-alert latency | Measure |
| Offline detection success | Measure |
| Local event storage | Demonstrate |
| Sync after reconnect | Demonstrate |
| Prototype BOM | ~₹4,000 |

Only measured numbers go into the final presentation.

---

# 16. Major risks and mitigation

## Snapdragon path fails

**Risk:** phone cannot execute the intended AI workload properly.

**Mitigation:** identify exact device first; verify Qualcomm AI Hub/device support; benchmark before locking the model.

## LoRa consumes too much time

**Mitigation:** Wi-Fi first; LoRa second.

## AI model performs badly

**Mitigation:** narrow the scope, collect our own footage, use lightweight models and prioritize a reliable system over a flashy model.

## Sensors are noisy

**Mitigation:** filtering, calibration, trend analysis and transparent labeling of prototype limitations.

## Dashboard becomes a time sink

**Mitigation:** stop UI work once it clearly communicates status, risk, evidence and connectivity.

## Scope explosion

**Mitigation:** enforce this scope firewall:

> **Two physical nodes + one Snapdragon gateway + two hazard demonstrations + offline operation.**

Anything beyond this is optional.

---

# 17. How we compete with teams that spend more

Do not try to win by having:

- the biggest model;
- the most sensors;
- the most expensive hardware;
- the prettiest dashboard.

We should win on:

### 1. Cost efficiency

A ~₹4,000 prototype demonstrates a system that is genuinely resource-conscious.

### 2. Architecture

Hierarchical edge intelligence rather than “everything goes to the cloud.”

### 3. Multimodal evidence

Camera + sensors + temporal trends.

### 4. Resilience

Critical decisions continue during connectivity loss.

### 5. Explainability

The operator can see **why** an alert happened.

### 6. Measurement

Latency, packet delivery, false alarms, water-level error and offline behavior are tested rather than claimed.

---

# 18. Defensible innovation statement

Use this formulation:

> **“Our innovation is not inventing a new smoke sensor or flood algorithm. It is designing a low-cost hierarchical edge-intelligence architecture in which resource-constrained sensor nodes perform local anomaly detection and a Snapdragon edge gateway performs higher-level multimodal reasoning, allowing immediate and explainable alerts even when network connectivity is unavailable.”**

Do not claim that each individual technique is novel.

Our strength is the **combination, implementation quality, cost discipline and measurable prototype**.

---

# 19. Research facts we should remember

Individual pieces of our architecture are already proven commercially or academically:

- environmental IoT monitoring;
- distributed wildfire sensing;
- embedded wildfire intelligence + LoRaWAN;
- camera-based wildfire intelligence;
- camera/staff-gauge flood monitoring;
- local/edge AI;
- offline buffering and synchronization.

Therefore our differentiation should be:

```text
Low cost
+
Hierarchical edge intelligence
+
Multimodal fusion
+
Offline-first response
+
Explainability
+
Modularity
```

---

# 20. Immediate next 48-hour objective

## You

- [ ] Identify available Snapdragon phone(s).
- [ ] Record exact model/chipset/Android version.
- [ ] Check Qualcomm AI Hub/device compatibility.
- [ ] Set up project repository.
- [ ] Create gateway software skeleton.
- [ ] Decide first fire-vision benchmark.

## Friend

- [ ] Inventory existing ESP32s and sensors.
- [ ] Get one ESP32 reading BME280.
- [ ] Get smoke/gas sensor readings.
- [ ] Investigate water-level sensing.
- [ ] Build clean sensor logging.
- [ ] Document wiring.

## Both

- [ ] Draw the complete architecture together.
- [ ] Trace one complete sensor-event message end-to-end.
- [ ] Freeze the first procurement list.
- [ ] Agree on the scope firewall.

---

# 21. Definition of MVP complete

The MVP is complete when this works end-to-end:

```text
             NODE 01
           /          \
       smoke        environment
          \             /
           \           /
            └→ gateway
                 │
              camera
                 │
              Snapdragon
                 │
             risk fusion
                 │
        ┌────────┴────────┐
        │                 │
      FIRE             FLOOD
        │                 │
      ALERT            ALERT
        │                 │
        └──────┬──────────┘
               │
          OFFLINE WORKS
               │
          LOCAL STORAGE
```

At this point we have a credible SIH prototype. Everything after that is optimization and hardening.

---

# 22. Project philosophy

We are operating under a hard budget constraint.

Every component and every software feature must justify itself.

Do not ask:

> “What cool technology can we add?”

Ask:

> **“What is the cheapest architecture that produces a more reliable decision?”**

That mindset is the core of the project.

A ₹30,000 project with more sensors is not automatically better.

A ₹4,000 project that can demonstrate distributed sensing, local intelligence, visual AI, sensor corroboration, explainable risk, offline operation, measurable latency and honest testing can make a much stronger engineering case.

---

# 23. One-sentence project description

> **A low-cost distributed environmental intelligence network where ESP32 sensor nodes detect local anomalies and a Snapdragon edge gateway fuses sensor and camera evidence to generate explainable, offline-capable fire and flood risk alerts.**

# 24. One-sentence competitive positioning

> **Instead of trying to outperform expensive systems sensor-by-sensor or model-by-model, we optimize the entire edge architecture for low-cost, resilient, multimodal decision-making.**

---

## Source / research basis

The project plan is grounded in the SIH 26178 problem statement and the earlier research pass covering existing environmental monitoring, wildfire sensing, camera-based detection, flood/water-level computer vision, Qualcomm AI/edge tooling, and India-specific LoRa considerations.

The attached Claude research also established the original low-budget direction: a small number of sensor nodes plus a single intelligent gateway, with fire/flood as the practical demonstration scope. See the source conversation excerpt for the original physical-system framing and budget constraints: `turn0file0`.
