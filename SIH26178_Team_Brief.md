# SIH26178 — Team Brief

Purpose of this doc: get everyone on the same page on what the problem statement actually asks for, what we're actually attempting, and where we stand against what already exists. No pitch language — just the facts, so the whole team is arguing from the same ground truth.

---

## 1. The official problem statement (verbatim scope, not our idea)

- **PS number:** SIH26178
- **Category:** Hardware
- **Submitting org:** Qualcomm Inc
- **Theme:** Disaster Management
- **Deadline:** 20 September 2026

**Title:** *"A resilient, AI-powered environmental monitoring network that provides early detection, localized intelligence, and actionable alerts for floods, forest fires, pollution events, and other environmental hazards common in India, enabling authorities and communities to shift from reactive disaster response to proactive risk prevention."*

**What Qualcomm's own "Expected Solution" section asks for:**

1. **Distributed smart sensor nodes** — water level, rainfall, temp, humidity, smoke, air quality (PM2.5/PM10), gas leakage, soil moisture, vibration. Solar-powered, low-maintenance, remote-deployable.
2. **On-device AI analytics** — real-time anomaly detection at the edge for flood risk, wildfire indicators, air-quality deterioration, landslide warning signs. Must work without continuous cloud connectivity.
3. **Multi-hazard early warning** — floods, forest fires, hazardous pollution episodes, extreme weather, industrial safety incidents.
4. **Regional risk mapping** — geospatial visualization, dynamic hotspot maps, integration with emergency dashboards.
5. **Community + authority notification** — mobile/web alerts, severity-based prioritization.
6. **Cloud + edge hybrid architecture** — edge for immediate decisions, cloud for trend analysis and forecasting.
7. **Scalable, cost-effective deployment** — modular, village-to-statewide, LoRaWAN/NB-IoT/Wi-Fi/5G.

**One fact that changes our strategy:** this PS is posted **by Qualcomm**, not a generic ministry. That's a strong signal the judging panel will specifically be looking for genuine use of Snapdragon / Qualcomm AI Hub on-device inference — not just "a phone we happened to use." Leaning into the Snapdragon NPU story isn't a nice-to-have here, it's aligned with who wrote the PS.

---

## 2. What we're actually building (be honest about the cut)

The PS lists **7 hazard categories**: flood, forest fire, air pollution, extreme heat, landslide, industrial leak, water quality.

**We are building 2 of 7: flood and forest fire.**

That's the honest scope. We are not pretending to cover pollution, heat, landslide, or industrial leaks. What we're actually demonstrating is:

> A working, physically-real slice of the "Environmental Intelligence Network" the PS describes — proving the architecture (distributed edge nodes → phone-based on-device AI fusion → offline-first alerting → cloud dashboard) works end to end, on two of the specified hazards, for ₹4,000.

Hardware: 2 ESP32 sensor nodes (fire node: gas + BME280; flood node: water level + BME280) talking to a borrowed Snapdragon Android phone over Wi-Fi/LoRa.
Software (the actual product): on-phone vision AI (smoke + waterline from camera), a sensor fusion/risk engine, explainable alerts, offline-first local alerting with cloud sync when internet returns.

**Say this scope cut out loud to judges before they ask.** Teams that quietly narrow scope and hope no one notices look evasive. Teams that state the cut and explain why look like engineers who understand tradeoffs.

**Gap to flag:** the PS explicitly expects *solar-powered* nodes. Our current plan doesn't include solar. Worth adding before the final demo, or at least having a straight answer ready for why it's not there yet.

---

## 3. Market research — what already exists (no pitch, just facts)

This is the uncomfortable but necessary part. None of the individual pieces of our idea are novel. Here's what's already live, funded, and in some cases already deployed in India.

### Wildfire/gas-sensor detection — Dryad Networks (Silvanet)
The direct commercial analogue to our fire node. Solar-powered gas sensors (CO, VOC, particulates) on trees, LoRaWAN mesh network, on-sensor ML to distinguish real fire signatures from noise, detects fires in the **smoldering phase** (before visible smoke), 10–15 year maintenance-free operation. As of their Gen-4-Pro launch (May 2026) they've added pollution monitoring and direct-to-satellite connectivity. This is what a fully-funded, VC-backed version of "Node 01" looks like. We will not out-engineer this with a ₹150 MQ sensor — we're not trying to.

### Flood monitoring — already government infrastructure in India
CWC (Central Water Commission) and IMD already run a national network of hydrometeorological stations, river/reservoir sensors, and satellite-fed flood alerts. This isn't a gap we're filling from zero:
- Karnataka's KSNDMC has already installed **132 water-level sensors** across flood-vulnerable areas.
- NGO-run systems (ICIMOD/Aranyak/SEE) already provide community-based flood early-warning in the Himalayan region using transmitter/receiver units on riverbanks.

The known gap the PS is actually pointing at is **hyperlocal, low-cost, edge-AI intelligence** — not "no flood sensors exist in India."

### Pollution/environmental IoT — Oizom (Ahmedabad)
An Indian company already doing almost exactly what the PS's "environmental sensor node" section describes: solar-powered, multi-protocol (LoRa/NB-IoT/GSM/Wi-Fi) air quality + weather monitoring hardware. 3,000+ devices deployed, 200M+ people covered across 65+ cities, already contracted by Pune Smart City and used internationally (Heathrow Airport). This is the pollution leg of the PS — the one we're explicitly not attempting — already being solved commercially, in India, at scale.

### Camera-based fire/vision detection
Vision-based smoke/fire detection is a mature commercial category globally (multiple companies run camera networks with AI smoke classifiers for wildfire response). Our camera-based smoke and waterline detection isn't a new technique — it's a known approach we're applying cheaply.

### The honest conclusion
Every individual sensor, every individual AI technique, and the general "edge AI disaster network" concept already exists — some of it built by companies with tens of millions in funding, some already contracted by Indian state governments. **We will not win on inventing something new.**

What's plausibly ours to win on, at a ₹4,000 budget:
- Proving a **borrowed consumer Snapdragon phone** can do the on-device fusion + vision job that these companies do with dedicated hardware and cloud backends.
- **Multi-source explainability** (showing *why* an alert fired, not just that it fired) — most of the systems above surface a score or a binary alert, not a reasoned evidence trail.
- **Genuine offline-first behavior demonstrated live** (pull the internet cable, alert still fires) — a concrete demo most teams talk about but don't actually show.
- Radical cost compression — turning a problem multiple funded companies solve with specialized, expensive hardware into something a village-level deployment could realistically afford.

That's the real pitch, and it's a fair one: not "we invented this," but "we're showing this is buildable at 1/100th the cost using hardware that already exists in most pockets."

---

## 4. Risks the team should walk in knowing

- **Scope honesty**: 2 of 7 hazards. Have the answer ready, don't get caught flat-footed.
- **No solar yet**: PS explicitly expects it; we don't have it in the current BOM.
- **Domain access**: companies like Dryad and Oizom have real deployment data and domain partnerships; we have none. Our edge has to be engineering execution and demo quality, not domain credibility.
- **Judging alignment**: Qualcomm as the sponsoring org means genuine NPU/AI Hub usage matters more here than in a generic PS — don't let this slip to "just running a model on the CPU."
- **Deadline**: 20 September 2026 for PS-level submission on the SIH portal — check your specific college's internal hackathon timeline against this separately, since internal shortlisting usually happens well before the national deadline.

---

## 5. Source

Official PS text sourced from the SIH 2026 problem statement archive (CC BY 4.0, mirrored from sih.gov.in): https://sih2026.vuce.in/en/ps/SIH26178
