# JATAYU Controller

<p align="center">
  <strong>An ESP32-C3 powered, ESP-NOW drone remote for the JATAYU Flight Controller.</strong><br />
  Compact in your hands. Fast on the link. Built for flight.
</p>

<p align="center">
  <a href="#hardware"><img src="https://img.shields.io/badge/MCU-ESP32--C3-E7352C?style=for-the-badge&logo=espressif&logoColor=white" alt="ESP32-C3" /></a>
  <a href="#the-link"><img src="https://img.shields.io/badge/link-ESP--NOW-111827?style=for-the-badge" alt="ESP-NOW" /></a>
  <a href="#power"><img src="https://img.shields.io/badge/power-1S%20LiPo-22C55E?style=for-the-badge" alt="1S LiPo" /></a>
  <img src="https://img.shields.io/badge/status-Hardware%20Design-8B5CF6?style=for-the-badge" alt="Hardware design" />
</p>

<p align="center"><img src="ASSets/Front.png" alt="JATAYU Controller hero image - assembled remote" width="800" /></p>

## Built for the JATAYU ecosystem

JATAYU Controller is a purpose-built handheld transmitter designed to talk directly to the **JATAYU Flight Controller**. It reads pilot inputs from twin analog joysticks and tactile controls, then sends compact control data over **ESP-NOW** - Espressif's low-latency, connectionless wireless protocol.

No router. No Bluetooth pairing screen. Just power on, link up, and fly.

> This repository contains the KiCad hardware design for the controller. Firmware, final protocol definitions, pin mappings, and flight-safety behaviour should be kept versioned alongside the matching JATAYU Flight Controller project.

## The link

<p align="center"><img src="ASSets/FRONT%20FRONT.png" alt="ESP-NOW telemetry flow - range test or protocol graphic" width="700" /></p>

```text
[JATAYU Controller] -- ESP-NOW control packets --> [JATAYU Flight Controller]
[JATAYU Controller] <-- optional telemetry ----- [JATAYU Flight Controller]
```

ESP-NOW is a strong fit for a dedicated RC link because it operates without a conventional Wi-Fi access point and keeps the radio exchange focused on the aircraft. The exact packet format, peer MAC addresses, update rate, channel, checksum, and failsafe timeout are firmware decisions - document them here when the flight stack is locked.

## Hardware

<p align="center"><img src="ASSets/BACK.png" alt="PCB render or assembled controller photo" width="700" /></p>

| Feature | Design choice |
| --- | --- |
| Brain | ESP32-C3-WROOM-02U - RISC-V MCU with 2.4 GHz radio |
| Flight controls | Two 2-axis analog joysticks |
| Extra input | Four tactile push buttons |
| Charging | USB-C input with TP4056 single-cell LiPo charger |
| Battery protection | DW01A + FS8205A protection stage |
| Regulation | AP2114H 3.3 V LDO |
| Feedback | Charge/status LEDs |

## Power

The controller is designed around a **single-cell 3.7 V LiPo battery**. USB-C feeds the onboard charger, while the battery protection circuit guards against over-charge, over-discharge, and over-current conditions. The 3.3 V regulator supplies the ESP32-C3 and controls.

**Treat lithium batteries with care:** use a protected, suitable 1S cell; inspect wiring and polarity before power-up; and never leave charging unattended.

## Repository map

```text
JATAYU-Controller.kicad_pro   KiCad project
JATAYU-Controller.kicad_sch   Schematic
JATAYU-Controller.kicad_pcb   PCB layout
JATAYU-Controller-BOM.csv     Bill of materials
GERBER/                       Manufacturing outputs
3D Files/                     Mechanical / enclosure assets
```

## Getting started

1. Open `JATAYU-Controller.kicad_pro` in KiCad.
2. Review the schematic, footprints, board rules, and antenna keep-out before ordering.
3. Generate fresh manufacturing files from the validated board revision.
4. Assemble and bring up the power rails first, then program and test the ESP-NOW firmware with the JATAYU Flight Controller.
5. Validate radio range, packet loss, neutral-stick calibration, arming logic, and failsafe behaviour **with props removed** before any flight test.

## Flight safety

This is an experimental flight-control device. A radio link must never be the only safety layer. The aircraft firmware should implement a conservative failsafe when packets stop arriving, reject malformed packets, require deliberate arming, and be tested in a controlled environment. Always follow applicable local regulations.

## Image checklist

The three placeholder images above are ready to be replaced with your own project shots:

1. A clean, assembled-controller hero photo.
2. An ESP-NOW architecture, telemetry, or range-test graphic.
3. A KiCad 3D render or close-up PCB photo.

---

<p align="center">Made for the <strong>JATAYU</strong> flight platform.</p>
