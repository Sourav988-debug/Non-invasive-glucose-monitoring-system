# Machine-Learning Pipeline

## Purpose

The ML layer converts processed optical/PPG measurements into an estimated glucose value. The repository documents the pipeline architecture and model rationale, but intentionally does not publish the complete training implementation.

![Research monitoring interface](../media/software-dashboard.jpg)

*Software interface used to demonstrate the end-to-end analysis concept. The displayed values are demo/software outputs, not clinical hardware measurements.*

## Pipeline

```text
Raw optical / PPG samples
        │
        ├── dark/reference correction
        ├── signal-quality assessment
        └── motion / saturation checks
                │
                ▼
        Preprocessing
        ├── baseline removal
        ├── band-pass filtering
        └── normalization
                │
                ▼
        Feature extraction
        ├── DC level
        ├── AC amplitude
        ├── AC/DC ratio
        ├── pulse / heart-rate features
        ├── wavelength-differential features
        └── quality/context features where justified
                │
                ▼
        Calibration / feature scaling
                │
                ▼
        Ridge Regression
                │
                ▼
        Estimated glucose
        + signal quality
        + uncertainty / prediction interval
```

## Why Ridge Regression?

Optical and physiological features are often correlated. Ridge Regression adds an L2 penalty to the ordinary least-squares objective:

[
min_{eta} ||y-Xeta||_2^2 + lambda ||eta||_2^2
]

where `lambda` controls the strength of regularization.

This makes Ridge attractive for a compact, interpretable baseline that can be represented efficiently in embedded inference.

## Feature preparation

The project explores features including:

- DC baseline level
- pulsatile AC amplitude
- AC/DC ratio
- pulse timing / heart-rate-related descriptors
- optical-channel or wavelength ratios
- signal-quality indicators
- motion indicators from the IMU
- context variables only when they are supported by the training data

The reference research also emphasizes handling skin colour, ambient light and sensor pressure before prediction.

## Reference glucose is the target

The reference glucose value is the **target label `y`**, not an input feature. It must never be allowed to leak into the feature vector used to make the prediction.

Conceptually:

```text
Sensor / context features  →  X
Reference glucose           →  y

X  →  trained model  →  estimated glucose
```

## Signal quality before ML

A key design principle is that an anomalous optical waveform should not automatically be interpreted as a glucose change.

The pipeline therefore evaluates:

- excessive motion
- optical saturation
- weak pulsatile amplitude
- implausible waveform morphology
- unstable baseline
- insufficient signal-to-noise ratio

A poor-quality window can be rejected or assigned reduced reliability before ML inference.

## Validation philosophy

For a health-related regression problem, a single train/test split can give a misleading impression if windows from the same person appear in both sets. The public project methodology therefore emphasizes subject-aware evaluation, such as GroupKFold or leave-one-subject-out validation, for future/current independent evaluation.

The repository intentionally does not publish numerical model results. It documents the evaluation methodology instead.

## Embedded inference direction

The regression model is small enough to be represented by a coefficient vector and intercept. This makes it suitable for an embedded implementation where inference consists primarily of feature normalization followed by a weighted sum.

The project therefore explores local inference rather than requiring cloud connectivity for every estimate.

## Important terminology

Use **estimated glucose** for the model output.

Do not describe the output as a directly measured blood-glucose concentration. The optical sensor measures an electrical/optical proxy, and the ML model estimates glucose from that proxy.
