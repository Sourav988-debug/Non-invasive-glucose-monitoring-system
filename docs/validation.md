# Validation Methodology

## Status

The prototype validation workflow has been completed/tested as part of the project development process. Numerical validation results are intentionally omitted from this public repository.

The purpose of this document is to explain **how the system should be evaluated**, not to present a clinical performance claim.

## Evaluation layers

### 1. Hardware validation

Check:

- optical emitter operation
- photodiode response
- TIA stability
- ADC communication
- analog noise floor
- saturation behavior
- power stability
- I2C integrity

### 2. Signal validation

Check:

- baseline stability
- pulsatile waveform quality
- AC/DC extraction
- heart-rate plausibility
- ambient-light rejection
- pressure sensitivity
- motion sensitivity

### 3. ML validation

Recommended metrics include:

- MAE
- RMSE
- MSE
- R²
- bias
- Bland–Altman agreement
- Clarke Error Grid where a suitable reference and clinical evaluation protocol are available

R² is a coefficient-of-determination statistic. It should not be described as generic clinical "accuracy."

## Subject separation

For wearable/physiological ML, windows from one participant can be highly correlated. A random window split can therefore leak subject-specific characteristics into the test set.

A stronger evaluation uses subject-aware grouping, for example:

- GroupKFold
- Leave-One-Subject-Out (LOSO)
- another explicitly participant-independent split

## Reference labels

Reference glucose is the prediction target. It must not be included as an input feature.

```text
Optical + physiological/context features → model → estimated glucose
                                              ↑
                                     reference glucose = y
```

## Clinical interpretation

Regression statistics alone do not establish clinical safety. Clarke Error Grid or another accepted clinical-error framework can provide additional context, but it also does not transform an experimental prototype into a certified medical device.

## Public-results policy

The repository deliberately does not publish the project's numerical validation results. This keeps the public documentation focused on the engineering methodology and avoids presenting historical/reference metrics as a claim of current clinical performance.
