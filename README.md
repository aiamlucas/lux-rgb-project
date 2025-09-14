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

## Power Input
- Power source: 12 V PD powerbank
- USB-C PD trigger board (fixed 12 V output, e.g. ZY12PDN or similar)
- Polyfuse resettable fuse 2–3 A (MF-R200 or MF-R300)
- Toggle or rocker switch, rated ≥2 A @ 12 V


### Power Input
- 1× 12 V PD powerbank  
- 1× USB-C PD trigger board (fixed 12 V output, e.g. ZY12PDN or similar)  
- 1× Polyfuse resettable fuse 2–3 A (MF-R200 or MF-R300)  
- 1× Toggle or rocker switch, rated ≥2 A @ 12 V  
- 1× Bulk capacitor 100 µF / ≥25 V electrolytic  
- 1× Indicator LED (3 or 5 mm)  
- 1× Resistor 1 kΩ (for indicator LED)  
---

### LED and Thermal
- 1× 10 W RGB COB LED  
- 1× Thermal paste or silicone pad  
- 1× Heatsink ~75×75×25 mm, aluminum finned or old CPU cooler  
- 4× M2/M2.5 screws + washers (or thermal epoxy) 

---

### Control Circuit
- 1× TLC555 (CMOS 555 timer, DIP-8)  
- 1× LM339 quad comparator (DIP-14)  
- 3× Potentiometer 10 kΩ linear, panel-mount, 6 mm shaft  
- 3× Knobs for potentiometers  
- 1× IC socket DIP-8  
- 1× IC socket DIP-14

---

### Timing and Reference Components
- 1× Resistor 3.3 kΩ (¼ W)  
- 1× Resistor 33 kΩ (¼ W)  
- 1× Capacitor 10 nF film/NPO (timing capacitor)  
- 6× Resistor 10 kΩ (¼ W)  
  - 3 for voltage ladder  
  - 3 as comparator pull-ups  
- 3× Resistor 1 MΩ (¼ W) *(optional, hysteresis feedback)*  
- 4× Capacitor 0.1 µF ceramic (decoupling at ICs)  
- 1× Capacitor 10 µF electrolytic (local decoupling near ICs)

---

### Switching Stage
- 3× Logic-level N-MOSFET (e.g., FQP30N06L, TO-220)  
- 3× Gate resistor 100 Ω (¼ W)  
- 3× Gate pulldown resistor 100 kΩ (¼ W)  

**Series resistors for LED current limiting**  
- 1× Red channel resistor: 18 Ω, ≥3 W  
- 1× Green channel resistor: 8.2 Ω, ≥2 W  
- 1× Blue channel resistor: 6.8 Ω, ≥2 W  

---

### Protection (recommended)
- 1× Schottky diode, ≥5 A, ≥20 V, in series with +12 V input  
- 1× TVS diode 15 V (e.g., SMBJ15A) across +12 V and GND  


```
USB-C PD Powerbank ──> PD Trigger (12 V out)
                           │
                           ▼
                     [ Main SWITCH ]
                           │
                      [ Polyfuse ]
                           │
                     [ Schottky diode ]
                           │
                     +12V_BUS ─────────────────────────────────────────────────────────────────────────────────┐
                           │                                                                                   │
                           │                                                                                   │
                     [ Bulk cap 100 µF ]                                                                       │
                     [ TVS diode 15 V ]                                                                        │
                           │                                                                                   │
                           ├───► Indicator LED (3 or 5 mm) + 1 kΩ ───► GND                                     │
                           │                                                                                   │
                           │                    ┌────────────────────────────────┐                             │
                           │                    │  TLC555 astable (~1 kHz)       │                             │
                           │                    │  VCC=+12V, GND                 │                             │
                           │                    │  RAMP = pins 2/6 node          │                             │
                           │                    └───────────┬────────────────────┘                             │
                           │                                │ RAMP (≈4–8 V)                                    │
                           │                                │                                                  │
                           │                  ┌─────────────┴─────────────────┐                                │
                           │                  │   3-resistor ladder           │                                │
                           │                  │  12V─10k─[8V]─10k─[4V]─10k─G  │                                │
                           │                  └────────┬──────────┬───────────┘                                │
                           │                           │          │                                            │
                           │                        top of    bottom of                                        │
                           │                         pots       pots                                           │
                           │                           │          │                                            │
                           │                ┌──────────┴──────────┴────────────┐                               │
                           │                │       3× potentiometers          │                               │
                           │                │   10k lin: top=8V, bot=4V        │                               │
                           │                │   wipers = VREF_R/G/B            │                               │
                           │                └───────┬─────────┬────────────────┘                               │
                           │                        │         │                                                │
                           │            ┌───────────┴─────┐ ┌──┴───────────┐ ┌───────────┴───────────┐         │
                           │            │   LM339 Chan 1  │ │ LM339 Chan 2 │ │   LM339 Chan 3        │         │
                           │            │ (+)=VREF_R      │ │ (+)=VREF_G   │ │ (+)=VREF_B            │         │
                           │            │ (−)=RAMP        │ │ (−)=RAMP     │ │ (−)=RAMP              │         │
                           │            │ OUT → pull-up → │ │ OUT → pull-up│ │ OUT → pull-up         │         │
                           │            └──────┬──────────┘ └─────┬────────┘ └─────────┬─────────────┘         │
                           │                   │100Ω gate         │100Ω gate           │100Ω gate              │
                           │            100k↓  │                  100k↓                100k↓                   │
                           │                   ▼                   ▼                   ▼                       │
                           │                Gate R MOSFET      Gate G MOSFET       Gate B MOSFET               │
                           │                   │                   │                   │                       │
                           │                Drain               Drain               Drain                      │
                           │                   │                   │                   │                       │
                           │   +12V → R_series → LED_R+   +12V → G_series → LED_G+   +12V → B_series → LED_B+  │
                           │                 LED_R− ───────┘    LED_G− ──────────────┘   LED_B− ───────────────┘
                           │                                                                                   │
                           └───────────────────────────────────────────────────────────────────────────────── GND
```
