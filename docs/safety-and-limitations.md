# Safety and Limitations

This repository describes an experimental research prototype for non-invasive glucose estimation.

## Not a medical device

The system is not intended for diagnosis, treatment decisions, insulin dosing, or replacement of an approved glucose meter or continuous glucose monitor.

## Optical measurement limitations

NIR/PPG signals are affected by:

- skin pigmentation
- tissue thickness and composition
- ambient light
- sensor placement
- mechanical pressure
- motion
- temperature
- perfusion changes
- electronic noise
- optical alignment

An anomalous optical signal does **not** mean that blood glucose actually changed.

## ML limitations

A regression model can learn subject-specific patterns instead of general glucose relationships if the evaluation design is weak. Participant-independent evaluation is therefore important.

The model output should be called **estimated glucose** rather than measured glucose.

## Data privacy

No raw human-subject/clinical dataset is distributed with this repository.

## Hardware safety

Any physical implementation involving optical emitters, batteries and human contact must be independently assessed for electrical, thermal, optical and mechanical safety before human use.
