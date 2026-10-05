# Non-Invasive Glucose Estimation Platform
## NIR Spectroscopy, PPG and Machine Learning

**Author:** Sourav Kumar Singh  
**Project type:** Personal research and engineering project  
**Status:** Prototype completed; testing and validation phase

## 1. Overview

This project explores non-invasive glucose estimation using near-infrared optical sensing combined with photoplethysmography, analog signal conditioning and machine learning.

The work began from a 940 nm NIR sensing and data-analytics approach described in the primary literature and evolved toward a compact embedded architecture. The current design adds a second NIR channel, a custom low-noise analog front end, external ADC, motion sensing, local display and an embedded-oriented regression inference path.

The objective is not to claim a clinically certified glucose meter. The objective is to develop and document a technically coherent research platform in which the optical signal chain, signal-quality logic and ML estimation pipeline can be independently evaluated.

## 2. Sensing principle

NIR light interacts with tissue through absorption and scattering. The detected optical signal contains a large static component and a smaller pulsatile component associated with blood-volume changes.

The PPG-oriented approach separates the signal into AC and DC components and uses normalized descriptors such as AC/DC to reduce dependence on absolute optical intensity.

The primary sensing wavelength is 940 nm. A 1300 nm channel is included in the extended design as a research direction for wavelength-differential sensing.

## 3. Hardware architecture

The current architecture consists of:

- ESP32-S3-WROOM-1
- VBPW34S silicon PIN photodiodes
- 940 nm and 1300 nm NIR emitters
- AD8628 low-noise analog stages
- ADS1115 16-bit ADC
- MPU6050 motion sensor
- SSD1306 OLED
- LiPo-based power architecture

The optical detector feeds a transimpedance stage, followed by active filtering and digitization. The ESP32-S3 provides the embedded processing and connectivity layer.

## 4. Machine-learning architecture

The ML workflow is intentionally lightweight and interpretable. Ridge Regression is used as the primary model because optical features can be correlated and L2 regularization stabilizes the learned coefficients.

The conceptual workflow is:

1. Acquire optical/PPG samples.
2. Perform dark/reference correction.
3. Assess signal quality and motion.
4. Filter and normalize the signal.
5. Extract optical and physiological features.
6. Apply model-compatible scaling/calibration.
7. Run Ridge Regression inference.
8. Return estimated glucose together with signal-quality information.

The complete training code is intentionally not included in the public repository. The methodology is documented so that the research process remains understandable without distributing the training implementation or human-subject data.

## 5. Calibration and confounders

Important non-glucose factors include skin pigmentation, ambient light, pressure, sensor placement and motion. These factors can alter the measured optical waveform without representing a real change in glucose.

The system therefore treats signal quality as part of the estimation process rather than assuming every acquired window is valid.

## 6. Embedded extension

A lightweight regression model can be represented by a small coefficient set and executed locally. This supports an edge-processing direction in which raw physiological data does not have to be continuously uploaded to a cloud service for every estimate.

The design also provides an OLED for local diagnostics and a BLE-capable MCU for future telemetry.

## 7. PCB design

The extended hardware architecture was translated into a compact two-layer SMT-oriented PCB concept. The design integrates the analog front end, ADC, MCU, motion sensor, display and power architecture.

The supplied PCB preview is included in `hardware/kicad/`.

## 8. Validation

The prototype validation workflow has been completed/tested as part of the project development process. This public report intentionally omits the numerical validation results.

The evaluation methodology considers regression error, agreement analysis, clinical-error analysis where appropriate, signal-quality checks and participant-independent validation.

R² is treated as a coefficient-of-determination statistic rather than a generic accuracy percentage.

## 9. Current status

### Completed

- Research and architecture definition
- 940 nm optical sensing concept
- PPG-oriented signal processing methodology
- Analog front-end architecture
- Ridge Regression methodology
- Dual-wavelength architecture
- Compact PCB design concept
- Motion-artifact gating architecture
- OLED/BLE architecture
- Monitoring-interface demonstration
- Validation methodology

### Testing and validation

Current engineering work focuses on hardware bring-up, optical characterization, motion rejection, embedded inference verification and independent validation.

## 10. Limitations

The project is a research prototype. Optical glucose estimation is highly sensitive to physiological and environmental variability, and a model trained on one population may not generalize to another without independent evaluation.

The system is not intended for diagnosis, treatment decisions, insulin dosing or replacement of an approved glucose meter or CGM.

## 11. Research references

The primary reference is the open-access 2024 Scientific Reports work by Rajeswari and Vijayakumar on the niGLUC-2.0v sensor and data-analytic framework. Recent research on PPG/TinyML and open-source multi-wavelength sensing is also used to contextualize the engineering extensions.

Full references are maintained in `research/references.bib`.
