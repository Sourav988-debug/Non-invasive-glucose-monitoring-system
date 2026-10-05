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

The system investigates whether useful glucose-related information can be extracted from weak optical variations in tissue and mapped to an estimated glucose value through a trained regression model.

The overall concept is:

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

The reference research underlying the initial data-analytic approach uses a 940 nm NIR emitter, optical detection, preprocessing, feature engineering and regression, with specific attention to skin colour, ambient light and sensor pressure. The primary paper is openly available and is included in `research/papers/`.

## Current architecture

### Optical sensing

- Primary NIR channel: **940 nm**
- Extended architecture: **940 nm + 1300 nm**
- Silicon PIN photodiodes for optical detection
- Fingertip transmission / reflective sensing concepts
- PPG-style AC/DC signal analysis

### Analog front end

- Low-noise transimpedance amplification
- Active high-pass / low-pass filtering
- Ambient-light / baseline compensation concepts
- Dedicated ADC rather than relying only on the microcontroller ADC

### Embedded hardware design

- ESP32-S3-WROOM-1
- ADS1115 16-bit ADC
- AD8628 low-noise op-amp stages
- VBPW34S photodiodes
- 940 nm and 1300 nm NIR emitters
- SSD1306 OLED
- MPU6050 IMU for motion-artifact gating
- LiPo power architecture

The PCB design is documented in `hardware/kicad/` and the available layout preview is shown below.

![PCB preview](media/pcb-preview.png)

## Machine-learning pipeline

The ML component is deliberately documented as a **pipeline and methodology**, not as a public training-code dump.

The intended flow is:

1. Acquire raw optical/PPG signals.
2. Perform dark/reference correction and signal-quality checks.
3. Remove baseline drift and out-of-band noise.
4. Extract stable optical and physiological features such as AC amplitude, DC level, AC/DC ratio and heart-rate-related features.
5. Apply calibration/context handling where justified by the training data.
6. Pass the feature vector to a regularized regression model.
7. Produce an **estimated glucose** value rather than presenting it as a direct measurement.
8. Attach signal-quality and uncertainty information to the estimate.

**Primary model:** Ridge Regression.

Ridge was selected because optical features can be correlated, while L2 regularization helps stabilize the regression coefficients. The public documentation explains the model and validation design without exposing the underlying training implementation.

![ML monitoring interface](media/software-dashboard.jpg)

The screenshot above is a software-interface demonstration of the research pipeline. Displayed values are software/demo outputs and should not be interpreted as clinical measurements.

See [`docs/ml-pipeline.md`](docs/ml-pipeline.md) for the detailed methodology.

## Engineering advancements

### 1. On-device ML inference

The project architecture moves lightweight regression inference toward the embedded device rather than requiring a permanently connected desktop/cloud system.

### 2. Dual-wavelength optical architecture

A secondary 1300 nm optical channel was added to the original 940 nm concept to investigate wavelength-differential sensing. This is an engineering extension; improved accuracy must be established experimentally rather than assumed.

### 3. Compact SMT PCB architecture

The earlier benchtop sensing concept was translated into a compact two-layer SMT-oriented PCB architecture integrating the optical AFE, ADC, MCU and peripherals.

### 4. Local display and wireless telemetry

An SSD1306 OLED provides local diagnostic/readout capability, while the ESP32-S3 architecture provides a path for BLE telemetry.

### 5. Motion-artifact rejection

An MPU6050 IMU is used as a signal-integrity gate. Windows contaminated by excessive movement can be rejected or downgraded instead of being blindly passed to the regression model.

### 6. Hardware-independent calibration / analysis workflow

A separate analysis concept allows signal-processing and calibration logic to be tested independently of the final physical enclosure and PCB.

## Hardware design

The current design is documented as an engineering prototype rather than a production medical device.

| Subsystem | Current design |
|---|---|
| MCU | ESP32-S3-WROOM-1 |
| Optical detector | VBPW34S silicon PIN photodiodes |
| NIR sources | 940 nm + 1300 nm |
| AFE | AD8628-based TIA/filter stages |
| ADC | ADS1115, 16-bit |
| Motion sensing | MPU6050 |
| Display | SSD1306 OLED |
| Power | LiPo-based architecture |
| PCB | Compact 2-layer SMT design |

See [`hardware/bom.md`](hardware/bom.md) and [`hardware/kicad/README.md`](hardware/kicad/README.md).

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
- uncertainty / prediction intervals where the calibration data supports them

The public repository does **not** publish the numerical validation results.

See [`docs/validation.md`](docs/validation.md).

## Research foundation

The primary reference is:

> S. V. K. R. Rajeswari and P. Vijayakumar, “Development of sensor system and data analytic framework for non-invasive blood glucose prediction,” *Scientific Reports*, vol. 14, Art. 9206, 2024. DOI: 10.1038/s41598-024-59744-7.

The article is open access under CC BY 4.0 and is included in this repository with attribution.

Recent literature also supports the project's directions in PPG-based estimation, embedded/TinyML inference and multi-wavelength optical sensing. A 2025 Scientific Reports study investigated PPG glucose estimation with embedded TinyML deployment, while another 2025 Scientific Reports study presented an open-source multi-wavelength NIR/visible system.

See [`research/README.md`](research/README.md) for the curated literature list and [`research/references.bib`](research/references.bib) for citation metadata.

## Repository structure

```text
.
├── README.md
├── CITATION.cff
├── LICENSE
├── docs/
│   ├── personal-technical-report.md
│   ├── personal-technical-report.pdf
│   ├── ml-pipeline.md
│   ├── hardware-architecture.md
│   ├── validation.md
│   ├── project-status.md
│   └── safety-and-limitations.md
├── hardware/
│   ├── bom.md
│   └── kicad/
│       ├── README.md
│       └── pcb-preview.pdf
├── research/
│   ├── README.md
│   ├── references.bib
│   └── papers/
│       └── rajeswari_vijayakumar_2024.pdf
├── data/
│   └── README.md
└── media/
    ├── pcb-preview.png
    ├── phase2-architecture.png
    └── software-dashboard.jpg
```

## Data policy

No raw participant or clinical dataset is included in this repository. The repository documents the expected data structure and analysis methodology without publishing sensitive human-subject measurements.

See [`data/README.md`](data/README.md).

## Reproducibility and scope

This repository is intended to make the **engineering reasoning, architecture, research basis and validation methodology** inspectable. It is not presented as a turnkey medical-device implementation.

The current public scope deliberately excludes the full ML training code and raw participant data. The model methodology, features, validation design and engineering decisions are documented instead.

## Safety and scientific limitations

Non-invasive glucose estimation is a difficult measurement problem. Optical signals are influenced by tissue composition, water absorption, pigmentation, pressure, sensor placement, ambient light, motion, temperature and other physiological variables. A corrupted sensor signal is not evidence that a person's glucose actually changed.

This project therefore treats signal quality and uncertainty as first-class outputs and does not recommend using the prototype for medical decisions.

See [`docs/safety-and-limitations.md`](docs/safety-and-limitations.md).

## License

Original project documentation is released under the license in [`LICENSE`](LICENSE). Third-party papers and other referenced material retain their respective licenses.

## Citation

If you reference this project in technical work, see [`CITATION.cff`](CITATION.cff).