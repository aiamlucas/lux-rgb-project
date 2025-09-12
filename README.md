# Portable RGB LED (10 W) – KiCad Learning Project

This repository contains the schematic, PCB design and supporting files for a making a **portable 10 W RGB LED lamp**.  
The goal of the project is to **learn KiCad** while giving some colorful light as "Hello World"

---

## Project Overview

- **What it does**  
	- A small portable lamp powered by a **USB-C power bank**.  
	- Three potentiometers control the brightness of **red, green, and blue channels** independently.  
	- The control circuit generates **PWM signals** using a 555 timer and LM339 comparators, which then drive three MOSFETs switching each LED color.
---

## Components

### Power & I/O
- USB-C PD trigger board (set to **12 V output**)
- USB-C panel-mount female pigtail
- Polyfuse 2–3 A (e.g. MF-R200/MF-R300)
- Power switch (≥2 A @ 12 V)
- Capacitors: 100 µF/25 V electrolytic, 0.1 µF ceramic

### Control (PWM + comparators)
- TLC555 timer (CMOS 555)
- LM339 quad comparator
- Potentiometers 10 k linear ×3 (RGB controls)
- Resistors (¼ W, unless noted):
  - 3.3 kΩ, 33 kΩ, 10 kΩ (ladder ×3, pull-ups ×3, series, etc.)
  - Optional 1 MΩ (hysteresis feedback ×3)
- Capacitor: 10 nF (timing), 0.1 µF decoupling

### Switching stage
- Logic-level N-MOSFETs ×3 (e.g. AO3400, IRLZ44N, FQP30N06L)
- Gate resistors: 100 Ω ×3
- Gate pulldowns: 100 kΩ ×3

### LED & Current limiting
- 10 W RGB COB LED
- **Option A (simple):**  
  - Red: 28 Ω 5 W resistor  
  - Green: 24 Ω 5 W resistor  
  - Blue: 24 Ω 5 W resistor  
- **Option B (preferred):**  
  - 3× LM317 configured as constant-current drivers  
  - Rsense ≈ 3.6–4.2 Ω (2 W) for 300–350 mA per channel

### Thermal
- Heatsink ~75×75×25 mm aluminum finned (or similar, ~3–4 °C/W)
- Thermal paste or silicone pad
- Screws (M2/M2.5) or thermal epoxy

### Misc
- Screw terminals or JST/XT connectors
- Panel-mount knobs ×3
- Indicator LED + 1 kΩ resistor (optional)
- Enclosure (printed, aluminum, or project box)

---

## Design Notes
- PWM frequency: ~1 kHz (555 astable)  
- Comparator range: pots span ~⅓–⅔ Vcc (≈4–8 V at 12 V supply) for smooth brightness control  
- Power bank runtime: ~3 h at full 10 W, longer at reduced brightness  
- Thermal management is critical: always mount the COB LED on a proper heatsink

---
