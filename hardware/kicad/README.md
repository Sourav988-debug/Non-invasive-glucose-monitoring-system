# KiCad Hardware Design

## Included

- `pcb-preview.pdf`: available PCB layout preview
- `../../media/pcb-preview.png`: rendered preview for quick inspection

![PCB layout](../../media/pcb-preview.png)

## Design architecture

The documented design uses an ESP32-S3 module, ADS1115 ADC, AD8628 analog stages, VBPW34S photodiodes, 940/1300 nm emitters, SSD1306 OLED, MPU6050 IMU and LiPo power architecture.

The available project material describes a compact two-layer SMT board approximately 100 × 50 mm in size.

## Source-file note

The native KiCad project files (`.kicad_pro`, `.kicad_sch`, `.kicad_pcb`) were not present in the source files accessible during this repository build. They are therefore **not reconstructed or invented** here. The repository contains the supplied visual design evidence instead.

The preview should be treated as a design artifact for review, not evidence of production qualification or completed electrical/EMC/DRC validation.
