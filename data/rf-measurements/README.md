# RF Measurements

NanoVNA-H4 S-parameter measurements for the 1090 MHz filter-LNA-filter front end characterization.

## Files

- `antenna_direct.csv` — S21/S11 direct antenna measurement (full band)
- `antenna_direct_g40.csv` — S21/S11 direct antenna with 40 dB gain setting
- `through_module.csv` — S21/S11 measurement through complete filter-LNA-filter module
- `through_module_g40.csv` — S21/S11 measurement through module with 40 dB gain setting

## Measurement Setup

- **VNA**: NanoVNA-H4
- **Reference plane**: 50Ω at SMA connectors
- **Frequency span**: 0.5–2.0 GHz (5000 points)
- **Port 2 attenuation**: 30 dB pad used for LNA gain measurements to protect VNA input
- **Calibration**: SOLT calibration performed over full sweep range with attenuator in place

## Notes

Antenna direct measurements characterize the SAW filter response in isolation. Through-module measurements show the complete signal chain performance including the PMA4-33GLN+ LNA stage.
