# InkTime Watch

Low-power open-source smartwatch hardware based on the **nRF52840**, designed around a **1.54” sunlight-readable e-paper display**, **BLE notifications**, **step counting**, **USB charging**, and a reliable **SWD recovery path**.

---

## Overview

InkTime is a time-first smartwatch optimized for:
- **low average power consumption**
- **high outdoor readability**
- **simple glanceable UI**
- **robust programming and recovery**
- **practical assembly for EVT/DVT prototypes**

### MVP features
- 1.54” e-paper display, **200 × 200 px**
- Partial display refresh **once per minute** for the watch face
- **BLE** notifications from a phone, with haptic alerts
- Always-on **step counting** using the accelerometer’s embedded pedometer
- **USB-C charging**
- **Tag-Connect TC2030** SWD programming/debug access

---

## Block Diagram

```text
USB-C 5V (VBUS)
   |
   v
+-------------------+
| BQ25180 Charger   |
| IN, BAT, SYS,I2C  |
+----+---------+----+
     |         |
   VBAT       VSYS
     |         |
     |         v
     |   +-------------------+
     |   | RT6160 Buck-Boost |
     |   | VIN -> 3V3        |
     |   +---------+---------+
     |             |
     v             v
+-----------+   +-------------------------------+
| MAX17048  |   | nRF52840                      |
| FuelGauge |   | BLE, USB, SPI, I2C, GPIO,SWD |
+-----+-----+   +--+---------+---------+--------+
      |            |         |         |
      |            |         |         |
      |            |         |         +--> Buttons x3
      |            |         |
      |            |         +------------> DRV2605L + ERM motor
      |            |
      |            +----------------------> BMA421 accelerometer
      |
      +-----------------------------------> Battery status / alerts

nRF52840 SPI + GPIO
        |
        v
+-------------------+
| PFET power switch |
| controls VEPD     |
+---------+---------+
          |
          v
+-------------------+
| 1.54" E-paper     |
| 200x200 display   |
+-------------------+

Tag-Connect TC2030 -> SWDIO / SWDCLK / nRESET / 3V3 / GND
```

---

## Repository Structure

```text
Hardware/
├── Schematic.sch
├── PCB.brd
└── Schematic.pdf

Manufacturing/
├── gerbers.zip
├── Bill_of_Materials.bom
└── Pick_and_Place.cpl

Mechanical/
├── InkTime-Model-Exploded.step
└── InkTime-Model.f3z

Images/
├── Exploaded.png
├── PCB.png
└── Watch.png

LICENSE
README.md
```

---

## Bill of Materials

> Main BOM items only. Passives, crystals, RF matching parts, and decoupling capacitors are listed in the exported manufacturing BOM.

| Qty | Function | Part | Ref. | JLC / LCSC | Datasheet |
|---:|---|---|---|---|---|
| 1 | MCU | nRF52840-QIAA-R | U1 | [C190794](https://jlcpcb.com/partdetail/NordicSemicon-NRF52840_QIAAR/C190794) | [Nordic Product Spec](https://docs.nordicsemi.com/bundle/ps_nrf52840/page/keyfeatures_html5.html) |
| 1 | Charger / power path | BQ25180YBGR | IC1 | [TI part page](https://www.ti.com/product/BQ25180/part-details/BQ25180YBGR) | [BQ25180 datasheet](https://www.ti.com/lit/gpn/BQ25180) |
| 1 | 3.3V regulator | RT6160AWSC | IC9 | [C7065276](https://www.lcsc.com/product-detail/C7065276.html) | [Richtek datasheet](https://www.richtek.com/Assets/product_file/RT6160A=RT6160B/DS6160AB-04.pdf) |
| 1 | Fuel gauge | MAX17048G+T10 | U3 | [C2682616](https://www.lcsc.com/product-detail/C2682616.html) | [MAX17048 datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf) |
| 1 | Accelerometer | BMA421 | IC3 | [C5242966 search](https://www.jlcpcb.com/parts?searchTxt=C5242966) | [BMA421 datasheet](https://files.pine64.org/doc/datasheet/pinetime/BST-BMA421-FL000.pdf) |
| 1 | Haptic driver | DRV2605LDGSR | IC2 | [C527464](https://jlcpcb.com/partdetail/C527464) | [DRV2605L datasheet](https://www.ti.com/lit/gpn/drv2605l) |
| 1 | Vibration motor | LCM1027B3605F | M1 | [C7528806 search](https://www.jlcpcb.com/parts?searchTxt=C7528806) | [JLC search page](https://www.jlcpcb.com/parts?searchTxt=LCM1027B3605F) |
| 1 | E-paper power PFET | SI2301CDS-T1-GE3 | Q1 | [C10487](https://jlcpcb.com/partdetail/VishayIntertech-SI2301CDS_T1GE3/C10487) | [Vishay datasheet](https://www.vishay.com/docs/70209/si2301cds.pdf) |
| 1 | USB ESD protection | USBLC6-2SC6Y | D3 | [JLC search](https://www.jlcpcb.com/parts?searchTxt=USBLC6-2SC6Y) | [ST datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) |
| 1 | USB-C connector | KH-TYPE-C-16P | J4 | [JLC search](https://www.jlcpcb.com/parts?searchTxt=KH-TYPE-C-16P) | [JLC search page](https://www.jlcpcb.com/parts?searchTxt=TYPE-C-16P) |
| 1 | E-paper FPC connector | 24-pin, 0.5 mm pitch | J1 | [JLC search](https://www.jlcpcb.com/parts?searchTxt=24pin%200.5mm%20FPC) | Vendor-specific |
| 1 | Debug connector footprint | Tag-Connect TC2030 | J2 | [Tag-Connect](https://www.tag-connect.com/product/tc2030-idc-nl) | [TC2030 docs](https://www.tag-connect.com/info/idc-footprint-details) |
| 3 | Buttons | SMD tactile switch | SW_UP, SW_DN, SW_ENT | [JLC search](https://www.jlcpcb.com/parts?searchTxt=EVP-AKE31A) | Vendor-specific |
| 1 | 2.4 GHz antenna | 2450AT18B100E | ANT1 | [JLC search](https://www.jlcpcb.com/parts?searchTxt=2450AT18B100E) | [Johanson datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100E.pdf) |

---

## Hardware Architecture

### 1) Power tree

The board uses five main power nodes:
- **VBUS (5V):** USB input from the Type-C connector
- **VBAT:** raw LiPo battery voltage
- **VSYS:** charger system output
- **VDD_3V3:** main regulated rail from RT6160
- **VEPD:** switched display rail, enabled through a PFET

Power path:
- **USB 5V -> BQ25180 IN -> BQ25180 SYS -> RT6160 VIN -> 3.3V rail**
- **LiPo -> BQ25180 BAT**
- **LiPo -> MAX17048 BAT/VDD**

### 2) Main controller

The **nRF52840** is the main MCU and handles:
- BLE communication
- USB detection / optional USB firmware support
- SPI for the e-paper display
- I2C for low-speed peripherals
- GPIO interrupts from buttons, charger, fuel gauge, and accelerometer
- SWD programming/debug

### 3) Power-management ICs

- **BQ25180** manages USB charging and the power path.
- **RT6160** provides a stable **3.3V** rail across battery discharge.
- **MAX17048** measures battery voltage / state-of-charge directly on **VBAT**, which is more accurate than estimating charge from the 3.3V rail.

### 4) Sensors and user feedback

- **BMA421 accelerometer** is used for always-on step counting and interrupt-based wake.
- **DRV2605L** drives the vibration motor for BLE notification alerts.
- **Three buttons** provide direct user interaction and can wake the system from low-power states.

### 5) E-paper subsystem

The e-paper display is connected over **SPI** plus dedicated control lines.
A **high-side PFET** switches the display supply (**VEPD**) so the panel can be completely powered down between updates or power-cycled during recovery.

### 6) Programming and recovery

A **Tag-Connect TC2030** footprint exposes:
- **SWDIO**
- **SWDCLK**
- **nRESET**
- **3V3 reference**
- **GND**

This guarantees a reliable programming path even if the main firmware is broken.

---

## Communication Interfaces

| Interface | Connected devices |
|---|---|
| I2C | BQ25180, RT6160, MAX17048, BMA421, DRV2605L |
| SPI | E-paper display |
| USB | USB-C connector, VBUS detect, optional USB firmware path |
| SWD | TC2030 debug/programming header |
| GPIO | Buttons, interrupt lines, PFET gate, display control |

### I2C default configuration
- **SDA:** P0.06
- **SCL:** P0.07
- Pull-ups: **10 kΩ** to 3V3
- Default bus speed: **100 kHz**
- Optional validated speed: **400 kHz**

### I2C address map

| Device | Address |
|---|---:|
| BMA421 | 0x18 |
| MAX17048 | 0x36 |
| DRV2605L | 0x5A |
| BQ25180 | 0x6A |
| RT6160 | 0x75 |

---

## nRF52840 Pin Mapping

| Pin | Signal | Connected block | Reason |
|---|---|---|---|
| P0.02 | EPD_SCK | E-paper display | SPI clock |
| P0.03 | EPD_MOSI | E-paper display | SPI data out |
| P0.05 | EPD_CS | E-paper display | Dedicated chip select |
| P0.06 | I2C_SDA | Charger, regulator, fuel gauge, IMU, haptic | Shared low-speed bus |
| P0.07 | I2C_SCL | Charger, regulator, fuel gauge, IMU, haptic | Shared low-speed bus |
| P0.08 | IMU_INT1 | BMA421 | Primary wake interrupt |
| P0.12 | HAPTIC_EN | DRV2605L | Full haptic shutdown between events |
| P0.13 | BTN_UP | Button Up | Active-low user input |
| P0.14 | BTN_DN | Button Down | Active-low user input |
| P0.15 | EPD_DC | E-paper display | Data / command select |
| P0.16 | EPD_RST | E-paper display | Hardware reset |
| P0.17 | EPD_BUSY | E-paper display | Busy feedback |
| P1.00 | BTN_ENT | Enter / Esc button | Active-low user input |
| P1.01 | EPD_PWR_GATE | PFET gate | Enables VEPD power gating |
| P1.08 | IMU_INT2 | BMA421 | Secondary interrupt / debug |
| SWDIO | SWDIO | TC2030 | Programming / debug |
| SWDCLK | SWDCLK | TC2030 | Programming / debug |
| nRESET | RESET | TC2030 | Reliable recovery |
| USBDP | USB_D+ | USB-C / ESD | Native USB |
| USBDM | USB_D- | USB-C / ESD | Native USB |
| VBUS | USB_VBUS_DET | USB-C | USB attach detection |
| XC1/XC2 | 32 MHz crystal | X1 | Main radio/system clock |
| XL1/XL2 | 32.768 kHz crystal | X2 | RTC / low-power timekeeping |
| ANT | RF | Matching network + antenna | BLE antenna feed |

---

## Power Budget Notes

The product is designed around **average current reduction**, not just low peak current.

Main strategies:
- e-paper for near-zero static display power
- partial refresh once per minute instead of continuous refresh
- accelerometer used in interrupt/pedometer mode
- haptic driver disabled when idle
- optional BLE duty cycling instead of permanent active connection
- optional **VEPD** power gating through the PFET

### Validation measurements recommended during EVT
- deep-sleep current
- current during minute-tick partial refresh
- current during BLE notification event
- current during haptic event
- 24-hour average current for a realistic user profile

---

## PCB Design Notes

- **4-layer PCB recommended** for RF behavior and power integrity
- Main components placed on the **TOP** layer
- **Power traces:** minimum **0.3 mm** where possible
- **Signal traces:** minimum **0.15 mm**
- No right-angle routing
- Local **100 nF** decoupling placed close to power pins
- Ground stitching used between planes, especially near RF
- **No copper, no routing, and no ground plane under the antenna**
- Charger/regulator kept away from antenna and RF matching network
- Accelerometer placed away from the vibration motor to reduce false motion coupling
- USB differential pair routed tightly with ESD protection close to the connector
- Test pads should be clearly labeled in silkscreen

---

## Mechanical Notes

Mechanical stack:
- top lens
- e-paper module
- PCB
- battery
- vibration motor
- back cover

Design goals:
- protect the e-paper panel from bending
- align the physical enclosure buttons with PCB switches
- keep the battery away from the antenna keepout
- mechanically couple the motor to the case for better haptic transfer
- provide access to USB-C for charging and bring-up

---

## Bring-Up Checklist

1. Verify TC2030 SWD wiring.
2. Flash basic test firmware.
3. Confirm stable 3.3V rail.
4. Measure baseline sleep current.
5. Run I2C scan and detect all devices.
6. Read MAX17048 battery data.
7. Verify BQ25180 charger status.
8. Validate RT6160 output across battery range.
9. Power up the e-paper rail and test refresh.
10. Run DRV2605L motor test.
11. Validate button interrupts and debounce.
12. Validate BMA421 wake/step interrupts.

---

## Design Decisions

- **nRF52840** chosen for BLE + USB + mature tooling
- **E-paper** chosen for sunlight readability and very low average power
- **RT6160 buck-boost** used to keep the 3.3V rail stable across battery discharge
- **MAX17048** added for direct battery monitoring
- **PFET display gating** added for leakage reduction and display recovery
- **TC2030 SWD** kept mandatory for reliable bring-up and recovery

---

## Manufacturing Outputs

The final repository should include:
- native schematic and PCB files
- schematic PDF
- Gerber archive
- BOM export
- pick-and-place / CPL export
- full 3D assembly in **STEP** and **Fusion360** formats
- rendered images of the PCB and full assembly

---

## Images

```md
![Schematic](Images/schematic.png)
![PCB 2D](Images/pcb_2d.png)
![PCB 3D](Images/pcb_3d.png)
![Exploded View](Images/exploded_view.png)
```

---

## License

Recommended: **Apache-2.0** or **MIT**.

---

## Submission Notes

Before submission, make sure the final repo contains:
- exact manufacturing exports
- final board screenshots/renders
- the 3D assembly
- the review issues raised during DVT
- any justified design-rule exceptions documented in GitHub issues or commit notes
