# Contributing

This started as a student project at Panimalar Engineering College and is open to contributions as it moves from design to working prototype.

## Areas currently needing work

See the **Project Status** checklist in the main [README](README.md) for the up-to-date list. Broadly:

- ESP32 sensor driver implementation (`firmware/esp32_main/`)
- Firebase dashboard build-out (`dashboard/`)
- Twilio SMS integration (`alerts/twilio_sms/`)
- Enclosure CAD finalization (`hardware/enclosure/`)

## How to contribute

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/flow-sensor-driver`)
3. Commit your changes with a clear message
4. Open a pull request describing what you changed and why

## Code style

- Firmware: standard Arduino/C++ conventions, keep sensor read/logic/communication functions separated as in the existing scaffold
- Docs: Markdown, keep architecture docs in sync with `docs/architecture/`
