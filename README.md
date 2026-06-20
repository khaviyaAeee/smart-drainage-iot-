# Smart IoT-Based Drainage Blockage Detection and Alert System

A low-cost IoT sensor network that monitors urban drainage in real time and alerts municipal teams before a blockage turns into a flood.

**Project Developer: Khaviya A**
Panimalar Engineering College, Poonamallee, Chennai

---

## Why this project exists

Chennai floods almost every monsoon, and it's rarely because of the rain alone. Drains get choked with plastic waste and silt long before the water rises, but nobody finds out until a street is already underwater. Municipal teams currently have no way to check a drain's condition without physically opening the manhole — which means blockages are discovered the same day as the flooding, not weeks before it.

This project puts a sensor node inside the manhole itself. It watches water level and flow continuously, tells a real blockage apart from ordinary heavy rain, and texts a response team the moment something needs attention — GPS pin included, so nobody has to go check it in person first.

![Comparison of reactive drainage management versus this proactive sensor-based system](docs/images/problem-comparison.png)

## How the detection logic works

A single water-level sensor can't tell the difference between heavy rainfall and a blockage — both raise the water level. The trick is watching **flow** at the same time. When it's raining hard, the level rises but water is still moving through the pipe at a normal rate. When the drain is actually blocked, the level rises but flow drops to almost nothing, because the water has nowhere to go. That gap between the two readings is what triggers an alert, and it's also what keeps the system from sending a false alarm every time it rains.

If the gas sensor picks up dangerous methane or H₂S levels at the same time, that manhole gets flagged as unsafe to enter on the dashboard — independent of the blockage logic — so sanitation crews know before they lift the cover, not after.

## System architecture

![System architecture diagram showing sensors feeding the ESP32, which routes data through MQTT to Firebase and Twilio, ending in a dashboard and SMS alert](docs/images/system-architecture.png)

**Sensing layer (inside the manhole)**
- JSN-SR04T waterproof ultrasonic sensor — measures water depth without touching the water
- Hall-effect flow sensor — measures how fast the water is moving
- MQ-135 gas sensor — checks for methane and H₂S buildup
- Neo-6M GPS module — tags the node's exact location

**Edge processing**
- ESP32 runs the level-vs-flow comparison locally, so the device only transmits when something's actually worth reporting. It also runs a watchdog timer that resets the node if it ever locks up, and spends most of its time in deep sleep to stretch battery life.

**Communication and cloud**
- Data goes out over MQTT, which holds up better than plain HTTP on the weak, intermittent connections you get underground
- Firebase Realtime Database stores the readings and powers a live map for the municipal dashboard
- Twilio sends the actual SMS alert once a blockage or unsafe gas reading is confirmed

## Built to survive a manhole, not a lab

Electronics don't usually last long in a sewer. Submersion, corrosive gas, and zero access to power are the three things that kill most prototypes before they ever get field-tested — so each one got a specific design answer.

![Hardware specification card showing IP68 submersion rating, corrosion resistance, battery life, deployment cost, and sensor stack](docs/images/hardware-specs.png)

The enclosure is IP68-rated, sealed against full submersion up to 1.5 meters, with PG7 nylon cable glands at every wire entry so water can't creep in along the cabling. The ultrasonic sensor is the industrial-grade version of the JSN-SR04T specifically because it holds up against sulfuric acid vapor, which standard sensors degrade under within months. And since there's no power outlet inside a manhole, running the ESP32 in deep sleep between readings is what gets the battery estimate up to 6–9 months per charge — the difference between something a city can actually maintain and something that needs monthly site visits.

## Cost

Priced for city-scale deployment, not just a one-off prototype: roughly **₹3,500 per unit**, sensors and enclosure included. Full breakdown is in [`hardware/bom/bill-of-materials.md`](hardware/bom/bill-of-materials.md).

## Where this fits into bigger goals

Beyond the immediate flood-prevention angle, the project lines up with a few UN Sustainable Development Goals:

- **Clean Water & Sanitation** — stops sewage from backing up into clean water sources during floods
- **Sustainable Cities** — keeps roads and transit usable instead of paralyzed by waterlogging
- **Good Health & Well-being** — removes the need for sanitation workers to manually check or enter manholes, which is how a lot of preventable injuries happen
- **Climate Action** — gives cities a way to adapt existing infrastructure to heavier, more frequent rainfall

## Repository layout

```
smart-drainage-iot/
├── firmware/             ESP32 code — sensor reads, blockage logic, MQTT
├── dashboard/            Firebase dashboard (GIS map of all nodes)
├── alerts/               Twilio SMS integration
├── hardware/             Bill of materials and enclosure design notes
├── docs/                 Architecture notes, images, research references
└── scripts/              Setup utilities
```

## Where things stand right now

The system design, sensor selection, architecture, and enclosure spec are complete — that's what's documented in this repo. The firmware in [`firmware/esp32_main/`](firmware/esp32_main/) is scaffolded with the full program structure and clearly marked TODOs, but the sensor drivers, MQTT integration, and dashboard still need to be built out.

- [x] Problem research and literature review
- [x] System architecture
- [x] Hardware selection and bill of materials
- [x] Enclosure durability design (IP68)
- [x] SDG impact mapping
- [ ] ESP32 firmware implementation
- [ ] Firebase dashboard
- [ ] Twilio SMS integration
- [ ] Field prototype testing

## Research this was based on

A few papers shaped specific design decisions — see [`docs/research/references.md`](docs/research/references.md) for full citations. The dual-sensor logic in particular came out of reading how other IoT flood-management systems handled the false-alarm problem during heavy rainfall.

## Developer

**Khaviya A**
Panimalar Engineering College, Poonamallee, Chennai

## License

[MIT](LICENSE)
