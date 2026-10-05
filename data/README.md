# Data Policy and Expected Dataset

No raw participant or clinical dataset is included in this repository.

## Why

Human-subject physiological and glucose measurements require appropriate consent, privacy controls and data-governance procedures. Publishing a dataset simply because it was used during development would not be appropriate without the required permissions.

## Expected structure

A future research dataset can conceptually contain:

| Field group | Examples |
|---|---|
| Optical signal | raw ADC samples, NIR channel values |
| PPG features | AC, DC, AC/DC, pulse timing |
| Wavelength features | 940 nm response, 1300 nm response, differential/ratio features |
| Signal quality | SNR, saturation, motion score |
| Context | measurement site, controlled acquisition conditions, relevant demographic/context variables |
| Target | reference glucose label |

The reference glucose value is the target label and must not be included among model input features.

## Reproduction

Researchers can reproduce the analysis methodology using an appropriately collected and ethically governed dataset matching the documented feature definitions.
