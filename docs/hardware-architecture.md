# Hardware Architecture

## Overview

The hardware architecture is designed around a weak optical signal chain. The engineering objective is to preserve a measurable physiological signal while reducing optical, electrical and motion-related interference.

![Architecture reference](../media/phase2-architecture.png)

The architecture shown in the available project material combines the integrated hardware signal path with a compact PCB design concept.

## Signal chain

```text
940 / 1300 nm NIR source
          ↓
     Human tissue
          ↓
   VBPW34S photodiode
          ↓
   Transimpedance stage
          ↓
   HPF / LPF conditioning
          ↓
      ADS1115 ADC
          ↓
      ESP32-S3 MCU
          ↓
Signal processing / ML inference
          ↓
       OLED / BLE
```

## Main components

| Component | Role |
|---|---|
| ESP32-S3-WROOM-1 | Embedded processing, connectivity and control |
| VBPW34S | NIR optical detection |
| 940 nm LED | Primary optical excitation |
| 1300 nm LED | Secondary wavelength for differential sensing research |
| AD8628 | Low-noise TIA and analog filtering stages |
| ADS1115 | External high-resolution ADC over I2C |
| MPU6050 | Motion detection / signal-quality gating |
| SSD1306 | Local display |
| LiPo + charging/power stage | Portable power architecture |

## Analog front end

The photodiode produces a small photocurrent. A transimpedance amplifier converts this current to a voltage, after which filtering reduces baseline drift and out-of-band noise.

The project uses a low-noise amplifier architecture with active filtering to protect the useful optical/PPG component before digitization.

## Dual-wavelength extension

The original sensing concept centered on 940 nm. The extended architecture adds a 1300 nm channel to investigate wavelength-differential features.

This should be treated as an experimental design extension. The presence of a second wavelength does not, by itself, prove improved glucose estimation.

## Motion sensing

The MPU6050 is used as an independent quality signal. A motion-contaminated optical window can be rejected or marked as unreliable instead of being treated as a genuine glucose event.

## PCB status

The repository includes the available PCB preview. The design is presented as a prototype-stage engineering design, not as a production-ready or medically certified PCB.

Native KiCad project files were not available among the source assets accessible during repository preparation, so the repository does not fabricate or reconstruct `.kicad_pro` / `.kicad_pcb` files that were not actually supplied.
