# Non-Invasive Glucose Estimation Platform Using NIR Spectroscopy, PPG, and Machine Learning

![Project phase](https://img.shields.io/badge/phase-prototype%20complete%20%7C%20testing%20%26%20validation-blue)
![Research](https://img.shields.io/badge/type-personal%20research%20%26%20engineering-6f42c1)
![Primary sensing](https://img.shields.io/badge/sensing-NIR%20%2B%20PPG-0b7285)
![ML](https://img.shields.io/badge/model-Ridge%20Regression-2f9e44)

A personal research and engineering project exploring non-invasive glucose estimation from optical signals using near-infrared (NIR) sensing, photoplethysmography (PPG), analog front-end design, signal processing, and machine learning.

**Author:** Sourav Kumar Singh  
**Repository:** `Sourav988-debug/Non-invasive-glucose-monitoring-system`

> **Research prototype disclaimer:** This project is not a medical device and must not be used for diagnosis, treatment decisions, insulin dosing, or replacement of an approved blood-glucose meter.

## Project status

**Prototype implementation: completed. Testing and validation: ongoing.**

The project has progressed from a 940 nm optical sensing concept and data-analytics workflow toward a more integrated architecture containing a custom analog front end, higher-resolution ADC, dual-wavelength optical channels, motion sensing, local display/telemetry, and an embedded-oriented ML inference path.

The repository intentionally documents the engineering architecture and validation methodology without publishing numerical clinical-performance results.

## What the project does

The system investigates whether useful glucose-related information can be extracted from weak optical variations in tissue and mapped to an **estimated glucose** value through a trained regression model.

```text
NIR emitters
     ↓
Tissue / vascular bed
     ↓
Photodiode optical detection
     ↓
Transimpedance amplifier
     ↓
Analog filtering
     ↓
High-resolution ADC
     ↓
Signal-quality checks + preprocessing
     ↓
PPG / optical feature extraction
     ↓
Context + calibration features
     ↓
Ridge Regression inference
     ↓
Estimated glucose + quality / uncertainty information
```

## Current architecture

### Optical sensing

- Primary NIR channel: **940 nm**
- Extended architecture: **940 nm + 1300 nm**
- Silicon PIN photodiodes
- Fingertip transmission / reflective sensing concepts
- PPG-style AC/DC signal analysis

### Analog front end

- Low-noise transimpedance amplification
- Active high-pass / low-pass filtering
- Ambient-light / baseline compensation concepts
- Dedicated external ADC

### Embedded hardware

- ESP32-S3-WROOM-1
- ADS1115 16-bit ADC
- AD8628 low-noise op-amp stages
- VBPW34S photodiodes
- 940 nm and 1300 nm NIR emitters
- SSD1306 OLED
- MPU6050 IMU for motion-artifact gating
- LiPo power architecture

See [`docs/hardware-architecture.md`](docs/hardware-architecture.md) and [`hardware/kicad/README.md`](hardware/kicad/README.md).

## Machine-learning pipeline

The ML component is documented as a **pipeline and methodology**, not as a public training-code dump.

1. Acquire raw optical/PPG signals.
2. Perform dark/reference correction and signal-quality checks.
3. Remove baseline drift and out-of-band noise.
4. Extract optical and physiological features such as AC amplitude, DC level, AC/DC ratio and pulse-related features.
5. Apply calibration/context handling where justified by the training data.
6. Pass the feature vector to Ridge Regression.
7. Produce an **estimated glucose** value.
8. Attach signal-quality and uncertainty information.

### Why Ridge Regression?

Optical features can be correlated. Ridge Regression adds L2 regularization to stabilize the coefficient estimates while retaining an interpretable, lightweight model suitable for embedded inference.

The reference glucose value is the target label `y`, not an input feature.

See [`docs/ml-pipeline.md`](docs/ml-pipeline.md).

## Engineering advancements

1. **On-device ML inference**  
   Lightweight regression inference is designed to move toward the embedded device rather than requiring permanent cloud/desktop execution.

2. **Dual-wavelength optical architecture**  
   A secondary 1300 nm channel extends the original 940 nm sensing concept for wavelength-differential investigation. Improved accuracy is not assumed without experimental evidence.

3. **Compact SMT PCB architecture**  
   The optical AFE, ADC, MCU and peripherals were translated into a compact two-layer SMT-oriented design.

4. **Local display and wireless telemetry**  
   SSD1306 provides local diagnostics/readout, while the ESP32-S3 provides a BLE-capable telemetry path.

5. **Motion-artifact rejection**  
   MPU6050 motion information can be used to reject or downgrade corrupted signal windows.

6. **Hardware-independent calibration / analysis**  
   Signal-processing and calibration logic can be tested independently of the final physical enclosure and PCB.

## Validation approach

Validation is treated as a separate engineering stage rather than as a single accuracy number.

The methodology considers:

- MAE
- MSE / RMSE
- R² as a regression-fit statistic, not a generic "accuracy" percentage
- Bland–Altman analysis
- Clarke Error Grid where clinically appropriate
- signal-quality rejection
- subject-aware evaluation to reduce leakage between training and evaluation subjects
- uncertainty / prediction intervals where supported by the calibration data

**Numerical validation results are intentionally not published in this repository.**

See [`docs/validation.md`](docs/validation.md).

## Research foundation

The primary reference is:

> S. V. K. R. Rajeswari and P. Vijayakumar, “Development of sensor system and data analytic framework for non-invasive blood glucose prediction,” *Scientific Reports*, vol. 14, Art. 9206, 2024. DOI: 10.1038/s41598-024-59744-7.

Official paper: https://doi.org/10.1038/s41598-024-59744-7

The article is open access under CC BY 4.0.

Recent research relevant to this project includes PPG/TinyML deployment, open-source multi-wavelength optical sensing, participant-independent wearable evaluation and personalization.

See [`research/README.md`](research/README.md) and [`research/references.bib`](research/references.bib).

## Data policy

No raw participant or clinical dataset is included.

The repository documents the expected data structure and ML methodology without publishing sensitive human-subject measurements.

See [`data/README.md`](data/README.md).

## Repository structure

```text
.
├── README.md
├── CITATION.cff
├── LICENSE
├── .gitignore
├── docs/
│   ├── personal-technical-report.md
│   ├── ml-pipeline.md
│   ├── hardware-architecture.md
│   ├── validation.md
│   ├── project-status.md
│   └── safety-and-limitations.md
├── hardware/
│   ├── bom.md
│   └── kicad/
│       └── README.md
├── research/
│   ├── README.md
│   ├── references.bib
│   └── papers/
│       └── README.md
└── data/
    └── README.md
```

## Scope and limitations

This repository is intended to make the **engineering reasoning, architecture, research basis and validation methodology** inspectable. It is not presented as a turnkey medical-device implementation.

The current public scope deliberately excludes the full ML training code and raw participant data.

NIR/PPG signals can be affected by skin pigmentation, tissue properties, ambient light, pressure, placement, motion, temperature, perfusion and electronic noise. A corrupted sensor signal is not evidence that a person's glucose actually changed.

The model output should be called **estimated glucose**, not measured glucose.

## Project phase

**Prototype completed → testing and validation phase**

Current engineering focus:

- hardware bring-up
- PCB-level verification
- optical characterization
- signal-quality and motion rejection
- embedded inference verification
- independent subject-aware validation

See [`docs/project-status.md`](docs/project-status.md).

## Citation

If you reference this project in technical work, see [`CITATION.cff`](CITATION.cff).
